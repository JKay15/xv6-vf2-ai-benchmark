# VisionFive 2 Port Plan for xv6-k210 Baseline

## 1. Goal

Port the current `xv6-k210` codebase to run on StarFive VisionFive 2 in a staged way that is suitable both for engineering execution and for AI-agent benchmarking.

The immediate goal is not “full lab parity on day one.” The immediate goal is:

1. Reach deterministic serial output on VisionFive 2.
2. Establish a clean handoff from U-Boot to the candidate OS.
3. Build toward trap, timer, and first user-mode execution.

## 2. Current Gap

The current baseline is not a VF2 codebase.

Observable facts from the current repo:

- The build logic only exposes `qemu` and `k210` paths.
- The memory layout is for `QEMU virt` and `K210`, not JH7110.
- The board drivers and bootloader support are K210-oriented.

Representative files:

- `/home/ubuntu/xv6-k210-submit-20260305/Makefile`
- `/home/ubuntu/xv6-k210-submit-20260305/kernel/include/memlayout.h`
- `/home/ubuntu/xv6-k210-submit-20260305/bootloader/SBI/rustsbi-k210`

Therefore, the VF2 work is a real board port, not a simple deployment exercise.

## 3. External Platform Facts

Official VF2 boot flow:

- `BootROM > SPL + Open SBI + U-Boot > Kernel + File System > Boot Complete`

Official references:

- Boot flow:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/boot_flow.html`
- U-Boot on VF2:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/u_boot.html`
- Boot mode settings:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/VisionFive2_SDK_QSG/boot_mode_settings.html`
- Boot address allocation:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/boot_address_allocation.html`
- U-Boot `bootelf`:
  - `https://docs.u-boot.org/en/latest/usage/cmd/bootelf.html`

Important implications:

- VF2 boot and memory assumptions are not the same as K210.
- The most practical path is to keep a known-good firmware stack and load the experimental OS payload from U-Boot.

## 4. Porting Strategy

### 4.1 Freeze firmware, port the OS payload

Do not start by rewriting or replacing SPL/OpenSBI/U-Boot.

Instead:

1. Keep a known-good VF2 firmware stack.
2. Use U-Boot to load a custom ELF payload.
3. Make the OS self-contained enough to initialize hardware after U-Boot handoff.

This isolates the porting problem to the OS.

### 4.2 Prefer `bootelf`

Recommended handoff:

1. Load the candidate ELF with TFTP or FAT.
2. Load the VF2 DTB if needed.
3. Launch with `bootelf`.

Standard U-Boot example:

```text
load mmc 0:1 ${loadaddr} /kernel.elf
load mmc 0:1 ${fdt_addr_r} /soc-board.dtb
bootelf -d ${fdt_addr_r} ${loadaddr} ${loadaddr}
```

This is based on U-Boot documentation and should be treated as the reference launch pattern, adjusted for the actual VF2 storage or TFTP path.

## 5. Recommended Phase Breakdown

### Phase A: Infrastructure Freeze

Goal:

- Make the board environment deterministic before touching the OS port.

Required outputs:

- Verified serial access from Mac.
- Verified U-Boot prompt access.
- Verified deploy path:
  - TFTP preferred
  - FAT-on-SD acceptable
- Verified board recovery notes.

Required host-side scripts:

- `build.sh`
- `deploy_tftp.sh`
- `deploy_sd.sh`
- `capture_serial.sh`
- `interrupt_uboot.expect`
- `run_uboot_script.expect`

Exit criteria:

- The operator can power-cycle the board, interrupt U-Boot, and load a known file on demand.

### Phase B: Early ELF Entry

Goal:

- Get the custom OS image loaded and executing from U-Boot.

Tasks:

1. Add a `platform=visionfive2` or equivalent target.
2. Introduce a VF2 linker layout.
3. Ensure the entry point is valid for the handoff path.
4. Replace early assumptions that only hold on K210/QEMU.
5. Print a deterministic early banner.

Exit criteria:

- Serial prints a banner after `bootelf`.

### Phase C: Early Console

Goal:

- Bring up UART output under the OS itself.

Tasks:

1. Identify the VF2 UART used for boot logs.
2. Add a minimal polled UART driver.
3. Route `printf` or an equivalent early logger to that UART.

Exit criteria:

- Output is produced without relying on Linux or Debian services.

### Phase D: Trap and Timer Skeleton

Goal:

- Establish the kernel’s trap entry and periodic timer behavior.

Tasks:

1. Define the supervisor trap path for the VF2 environment.
2. Understand the timer source available after U-Boot handoff.
3. Rebuild the minimal interrupt/timer plumbing.
4. Add instrumentation for trap cause and timer ticks.

Exit criteria:

- A periodic tick or equivalent trap activity appears in logs.

### Phase E: Address Space and Memory Layout

Goal:

- Replace K210/QEMU-specific address assumptions with VF2-safe ones.

Tasks:

1. Redefine physical memory constants.
2. Redefine MMIO base addresses used by the kernel.
3. Re-check page-table setup against the new layout.
4. Validate the trampoline, trapframe, kernel stack, and user memory boundaries.

Exit criteria:

- The kernel survives page table install and continues printing.

### Phase F: Scheduler and First Process

Goal:

- Reach `userinit` or an equivalent first process.

Tasks:

1. Validate `procinit`, stacks, and context switching.
2. Validate user page table creation.
3. Validate trap return path to user mode.
4. Use an intentionally tiny first user program.

Exit criteria:

- The board reaches the first user-space milestone deterministically.

### Phase G: Storage and File System

Goal:

- Decide whether to support real storage on VF2 or temporarily bypass it.

Recommended benchmark choice:

- Do not require this phase for Stage 2.
- Make it part of Stage 3.

Tasks if pursued:

1. Choose the storage path:
   - SD/MMC
   - NVMe
   - temporary initramfs-style or memory-backed workaround
2. Add or adapt block I/O support.
3. Decide whether FAT32 remains the file system baseline.

Exit criteria:

- A repeatable user payload can be loaded from the chosen medium or in-memory scheme.

## 6. Concrete Technical Worklist

### 6.1 Build system

Create a new platform branch in the build logic:

- Add `platform=visionfive2`
- Separate compiler and linker flags if needed
- Emit:
  - ELF payload
  - optional flat binary
  - map file
  - disassembly

Suggested artifacts:

- `target/vf2/kernel-vf2.elf`
- `target/vf2/kernel-vf2.bin`
- `target/vf2/kernel-vf2.asm`
- `target/vf2/kernel-vf2.map`

### 6.2 Board configuration layer

Add a minimal VF2 board abstraction layer for:

- memory map
- UART
- timer source
- reset/shutdown path

Avoid scattering board constants across generic files.

Suggested file split:

- `kernel/include/platform_vf2.h`
- `kernel/platform_vf2.c`
- `kernel/uart_vf2.c`
- `kernel/timer_vf2.c`

### 6.3 Linker and entry

Create a VF2-specific linker script or linker fragment.

Need to define:

- load address
- text/data placement
- BSS placement
- stack assumptions
- any reserved memory conflicts

Do not reuse K210 addresses blindly.

### 6.4 Serial-first debug model

Before enabling complex features, instrument:

- entry banner
- page-table installed
- trap entered
- timer initialized
- scheduler entered
- userinit started

Without this, the bring-up will degenerate into blind hangs.

### 6.5 U-Boot launch contract

Freeze one launch contract and keep it stable.

Recommended contract:

- payload format: ELF
- deploy method: TFTP first
- launch command: `bootelf`
- optional FDT handoff if needed

If the OS does not consume FDT initially, document that explicitly instead of silently ignoring it.

## 7. Suggested Non-Goals for First Bring-up

Do not require these in the first successful VF2 boot:

- full FAT32 support
- full Linux syscall compatibility
- networking inside xv6
- SMP
- advanced storage
- all lab tests on board

These are later concerns.

## 8. Risks

### R1. Memory layout mismatch

Symptom:

- immediate hang or exception after handoff

Cause:

- invalid load address or overlap with firmware-reserved regions

Mitigation:

- validate linker placement and boot logs first

### R2. UART mismatch

Symptom:

- image runs but prints nothing

Cause:

- wrong UART base or pin path

Mitigation:

- confirm which UART path U-Boot is using on the board

### R3. Timer assumptions copied from K210

Symptom:

- scheduler or sleep path never advances

Cause:

- wrong timer source or interrupt configuration

Mitigation:

- keep timer work separate from console bring-up

### R4. Over-scoping too early

Symptom:

- agent spends all time on storage or firmware replacement

Cause:

- no staged plan

Mitigation:

- freeze milestone order and non-goals

## 9. Recommended Host Workflow on Mac

### Option A: TFTP-based workflow

Use when Ethernet is available.

Flow:

1. Build ELF on the Mac or copy the built ELF from another machine.
2. Place the ELF and optional DTB in the TFTP directory.
3. Power-cycle VF2.
4. Interrupt U-Boot.
5. Load files with `tftpboot`.
6. Launch with `bootelf`.
7. Capture serial log.

Advantages:

- fast iteration
- less SD wear
- easier scripting

### Option B: FAT-on-SD workflow

Use when TFTP is inconvenient.

Flow:

1. Mount the VF2 boot partition on the Mac.
2. Copy ELF and optional DTB.
3. Reinsert SD card.
4. Interrupt U-Boot.
5. Load from FAT.
6. Launch with `bootelf`.
7. Capture serial log.

Advantages:

- simple physical model

Disadvantages:

- slower
- more operator variance

## 10. First Executable Milestone

The first milestone should be intentionally small:

- U-Boot loads `kernel-vf2.elf`
- `bootelf` transfers control
- the kernel prints:
  - banner
  - hart id if available
  - “paging off/on” state if relevant
  - end-of-early-init marker

That is enough for the first success.

## 11. Acceptance Criteria Per Phase

### Phase A accepted when

- serial from Mac is proven
- U-Boot prompt access is proven
- deploy path is proven

### Phase B accepted when

- custom ELF starts after `bootelf`

### Phase C accepted when

- UART output is produced by the custom OS

### Phase D accepted when

- timer or trap activity is observable

### Phase E accepted when

- page tables install without killing the kernel

### Phase F accepted when

- first user payload runs

### Phase G accepted when

- a repeatable storage-backed or documented memory-backed payload path works

## 12. Recommended File/Artifact Bundle for Each Run

Each bring-up attempt should archive:

- git commit id
- diff from baseline
- ELF image
- map file
- U-Boot command transcript
- serial log
- deploy method
- board boot mode
- pass/fail milestone

Without this archive, cross-agent comparison becomes unreliable.

