# VisionFive 2 AI Benchmark Protocol

## 1. Purpose

This document defines a fair benchmark for comparing multiple AI agents on:

1. Finishing the xv6-k210 lab codebase.
2. Porting the resulting OS to the StarFive VisionFive 2 board.
3. Demonstrating stable execution and measurable hardware-side behavior.

The benchmark is intentionally split into three stages. If these stages are mixed together, the result mostly reflects board bring-up luck, operator intervention, and firmware recovery skill, rather than OS reasoning quality.

## 2. Scope

The benchmark has two different source identities and they must not be mixed:

1. Official Stage 1 starter:
   - repo: `https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git`
   - common framework branch: `master`
   - common framework commit: `d740928`
   - official remaining-part starter branches:
     - `part4-scheduler` at `6d7ede9`
     - `part5-mem` at `c93a75a`
     - `part6-page-replace` at `0db77e0`
     - `part8-sem` at `79ea868`
2. Solved reference outcome:
   - repo: `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os.git`
   - branch: `main`
   - tag: `part4-8-all-pass`
   - commit: `4c3c4b9`

The solved repository is not a Stage 1 input. It is a completed outcome and should only be used as a reference outside blind evaluation.

The upstream coursework codebase is an `xv6-k210` tree that currently supports `qemu` and `k210`, not VisionFive 2. This matters because Stage 2 and Stage 3 are not “flash the existing image to VF2”; they are a real board port.

## 3. Stage Design

### Stage 1: Lab Correctness

Goal:

- Compare the agents on the original course task under the same repository, documents, and submission target.

Input:

- The exact official starter branch for the part being evaluated.
- Or, if using a convenience package, the template `master` plus the official starter bundles.
- The exact course PDFs and PPT.
- The exact local and platform test information.

Output:

- A branch or patchset intended for course submission.

Primary metric:

- Platform pass/fail and total passed cases.

Secondary metrics:

- Number of user-agent rounds.
- Time to first passing submission.
- Number of regressions introduced while fixing other parts.

Important rule:

- Stage 1 should be run as four course-faithful subtasks unless there is a strong reason not to:
  - Part 4 from `part4-scheduler`
  - Part 5 from `part5-mem`
  - Part 6 from `part6-page-replace`
  - Part 8 from `part8-sem`
- Do not give agents the solved `4c3c4b9` repository as their blind starting point.

### Stage 2: Board Bring-up on VisionFive 2

Goal:

- Compare the agents on minimal real-board bring-up, without requiring full file system support.

Output requirement:

- A VF2-specific build target or equivalent scripts.
- A repeatable boot path using an already working VisionFive 2 firmware stack.
- A serial log proving the OS reaches defined milestones.

Primary metric:

- Milestone score, listed in Section 8.

Important rule:

- Stage 2 does not require the full lab surface area on the board.
- Storage and full syscall coverage are explicitly deferred unless they are needed for the selected milestone.

### Stage 3: Full Hardware Function

Goal:

- Compare the agents on turning the VF2 port from “boots and prints” into “usable OS behavior on hardware.”

Output requirement:

- Stable bring-up and representative functionality on the real board.
- Measured logs for UART, timer/trap, user-mode entry, and storage-backed or memory-backed workloads.

Primary metric:

- Weighted functionality score, listed in Section 9.

## 4. Required Starting Assets

These assets must be frozen before any agent starts. Every agent gets the same package.

### 4.1 Baseline source package

Provide:

1. The common framework package:
   - `stage1-common-src-xv6-k210-template-master-d740928.tar.gz`
   - `stage1-common-src-xv6-k210-template-master-d740928.bundle`
2. The official starter-branch packages:
   - `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.tar.gz`
   - `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.bundle`
   - `stage1-starter-xv6-k210-template-part5-mem-c93a75a.tar.gz`
   - `stage1-starter-xv6-k210-template-part5-mem-c93a75a.bundle`
   - `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.tar.gz`
   - `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.bundle`
   - `stage1-starter-xv6-k210-template-part8-sem-79ea868.tar.gz`
   - `stage1-starter-xv6-k210-template-part8-sem-79ea868.bundle`

Do not package the solved `4c3c4b9` repository as a Stage 1 baseline.

### 4.2 Course documents

Provide these exact local documents:

- `/home/ubuntu/docs/Part 7 系统调用：文件系统.pdf`
- `/home/ubuntu/docs/运行基础知识.pdf`
- `/home/ubuntu/docs/OS实验课.pptx`

Also include the repository’s own documentation directory:

- none required from the template repo itself

Do not include the solved repository's `doc/` directory in blind Stage 1 if it contains post-hoc explanations or solution-like material.

### 4.3 Test assets for Stage 1

Provide:

- The syscall testsuite tree:
  - `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing`

Use this as a reproducibility aid for the already-present syscall and filesystem layer. It should not be confused with the remaining unfinished kernel tasks.

### 4.4 VisionFive 2 board package for Stage 2 and 3

Provide:

- One specific VisionFive 2 board.
- One known-good USB-to-UART adapter.
- One known-good power adapter.
- One known-good microSD card.
- One known-good Ethernet setup if TFTP is used.

The operator must also freeze:

- Board revision.
- Boot mode switch position.
- Current firmware version and recovery method.
- Host machine model and OS version.

### 4.5 VisionFive 2 reference documents

Provide at minimum these official references:

- Boot flow:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/boot_flow.html`
- Boot mode settings:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/VisionFive2_SDK_QSG/boot_mode_settings.html`
- Boot address allocation:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/boot_address_allocation.html`
- U-Boot overview:
  - `https://doc-en.rvspace.org/VisionFive2/Boot_UG/JH7110_SDK/u_boot.html`
- USB-to-serial on Mac/Linux:
  - `https://doc-en.rvspace.org/VisionFive2/Quick_Start_Guide/VisionFive2_QSG/for_maclinux2%20-%20vf2.html`
- Required hardware:
  - `https://doc-en.rvspace.org/VisionFive2/Quick_Start_Guide/VisionFive2_QSG/required_hardware.html`
- VF2 datasheet:
  - `https://doc-en.rvspace.org/VisionFive2/PDF/VisionFive2_Datasheet.pdf`
- U-Boot `bootelf` command:
  - `https://docs.u-boot.org/en/latest/usage/cmd/bootelf.html`

### 4.6 Recovery package

This must be prepared before the benchmark begins.

Provide:

- A known-good VisionFive 2 image or firmware package.
- A written recovery procedure.
- A pre-tested serial capture setup.
- A pre-tested “enter U-Boot prompt” procedure.

Without this, one bad run can destroy fairness for all later agents.

## 5. Fixed Benchmark Rules

### 5.1 Same-start rule

All agents must start from:

- The same repo snapshot.
- The same document bundle.
- The same board package.
- The same operator protocol.

### 5.2 No hidden human patch rule

The human operator may not silently fix code, boot configuration, or board state on behalf of one agent.

Allowed operator actions:

- Executing fixed protocol commands.
- Returning raw logs.
- Resetting the board.
- Re-imaging the board only through the published recovery procedure.

### 5.3 Same interaction budget

For each agent, freeze:

- Max wall-clock time.
- Max turns.
- Max board deployment attempts.
- Max recovery attempts.

Suggested defaults:

- Stage 1: 2 hours
- Stage 2: 4 hours
- Stage 3: 4 hours

### 5.4 Log preservation rule

For every run, preserve:

- Full build logs
- Full serial logs
- U-Boot command transcript
- Deployment logs
- Git diff or patchset

## 6. Operator Protocol

To keep the benchmark fair, the human should behave like a deterministic tool runner.

Agents may request only these actions:

- `build`
- `deploy_sd`
- `deploy_tftp`
- `reset_board`
- `power_cycle`
- `capture_serial <seconds>`
- `interrupt_uboot`
- `run_uboot_script <script-name>`
- `show_host_file <path>`
- `show_board_log <path>`
- `recover_board`

The operator response must contain:

- Exact command run
- Exact stdout/stderr or serial output
- Exact success/failure state

The operator must not:

- Rewrite logs
- Summarize instead of returning raw output unless explicitly asked
- Change commands ad hoc for one agent only

## 7. Deployment Strategy for Stage 2 and 3

The benchmark should freeze a working firmware stack and compare only the OS payload.

Recommended strategy:

1. Keep a known-good VF2 firmware state.
2. Do not ask agents to first replace SPL/OpenSBI/U-Boot.
3. Use U-Boot to load and run the candidate OS image.

Why:

- Official StarFive documentation shows the boot chain is `BootROM > SPL + Open SBI + U-Boot > Kernel + File System` and that U-Boot can load payloads via network, UART, QSPI, SDIO, or NVMe.
- U-Boot official documentation also supports booting ELF payloads with `bootelf`.

Recommended deployment priority:

1. TFTP via U-Boot
2. FAT partition on microSD
3. Only if necessary, direct firmware updates

Rationale:

- TFTP minimizes repeated SD card handling.
- It reduces operator variance.
- It is easier to automate and compare.

## 8. Stage 2 Milestones and Scoring

Stage 2 score: 35 points total

### M1. Build target exists: 5 points

Requirement:

- The repo can produce a distinct VF2 image target or equivalent script output.

Evidence:

- Build log
- Artifact list

### M2. U-Boot can load the image: 5 points

Requirement:

- The operator can load the candidate payload from TFTP or SD into RAM.

Evidence:

- U-Boot transcript

### M3. U-Boot can transfer control: 5 points

Requirement:

- `bootelf` or equivalent handoff reaches the candidate image.

Evidence:

- U-Boot transcript
- Immediate serial output or crash signature

### M4. Early serial output works: 5 points

Requirement:

- The candidate image prints a deterministic banner.

Evidence:

- Raw serial log

### M5. Trap/timer path works: 5 points

Requirement:

- One explicit timer or trap milestone is logged.

Evidence:

- Raw serial log

### M6. First user-mode path works: 5 points

Requirement:

- The OS reaches `userinit` or an equivalent first user payload.

Evidence:

- Raw serial log

### M7. Reproducibility: 5 points

Requirement:

- The same image reaches the same milestone in 10 cold boots with at least 8 successes.

Evidence:

- Boot matrix
- Serial logs

## 9. Stage 3 Functionality and Scoring

Stage 3 score: 25 points total

### F1. Stable UART console: 4 points

Requirement:

- Bidirectional serial interaction works reliably across repeated boots.

### F2. Timer and scheduler behavior: 5 points

Requirement:

- The board runs timer-driven scheduling or an equivalent periodic event stably.

### F3. Memory management path: 4 points

Requirement:

- At least one representative memory feature works on board:
  - page allocation
  - page table install
  - trap-return loop
  - user memory access

### F4. User-space program execution: 4 points

Requirement:

- A trivial user payload runs and exits or loops deterministically.

### F5. Storage-backed or memory-backed workload: 4 points

Requirement:

- Either:
  - a real storage path works, or
  - a well-defined temporary memory-backed path is used and documented.

### F6. Soak run: 4 points

Requirement:

- The board runs the selected demo workload for 5 minutes without crash or manual intervention.

## 10. Engineering Quality Score

Engineering quality score: 10 points total

### Q1. Intervention count: 3 points

- Fewer operator interventions score higher.

### Q2. Reproducible scripts: 3 points

- Better scripted deployment and logging score higher.

### Q3. Debugging quality: 2 points

- Better use of logs, smaller blind trial-and-error loops score higher.

### Q4. Change clarity: 2 points

- Clear commit history, diffs, and rationale score higher.

## 11. Deliverables Per Agent

Every agent submission should include:

- Final Git branch or patchset
- Build script
- Deploy script
- Serial capture script
- Recovery notes if the board became unbootable
- Stage 2 log bundle
- Stage 3 log bundle
- A short porting note explaining assumptions and residual gaps

## 12. Mac Host Toolkit

Prepare one fixed host-side toolkit before the benchmark starts:

- `build.sh`
- `deploy_tftp.sh`
- `deploy_sd.sh`
- `capture_serial.sh`
- `interrupt_uboot.expect`
- `run_uboot_script.expect`
- `collect_logs.sh`
- `recover_vf2.md`

These files are not part of the agent score if they are frozen and shared equally. They are part of benchmark infrastructure.

## 13. Practical VF2 Notes

The official references support the following operational assumptions:

- VisionFive 2 requires a USB-to-UART adapter for serial work on Mac/Linux.
- The USB-C port is for power supply; it must not be treated as the normal data path.
- StarFive documents the boot mode pins and recommends 1-bit QSPI NOR flash mode.
- U-Boot on VF2 can load payloads via network and other media.
- `bootelf` is the cleanest standard mechanism for launching a custom ELF payload from U-Boot.

Inference from the above:

- The safest benchmark setup is to keep board firmware working, then load the experimental OS as a payload from U-Boot.
- This avoids turning the benchmark into a SPL/OpenSBI/U-Boot flashing contest.

## 14. Recommended Benchmark Order

Run the benchmark in this order:

1. Stage 1 on all agents.
2. Freeze Stage 1 result snapshots.
3. Run Stage 2 on all agents using the same VF2 infrastructure.
4. Freeze Stage 2 result snapshots.
5. Run Stage 3 only for agents that reached at least Stage 2 milestone M4.

This avoids spending most of the board budget on agents that never reached even early serial output.

## 15. Minimal Asset Checklist

Before starting any agent, confirm:

- Baseline repo snapshot exists.
- Official starter-branch bundles exist.
- Course docs bundle exists.
- Local testsuite exists.
- VF2 board and power supply are stable.
- USB-to-UART adapter is tested.
- Serial logging from Mac is tested.
- Known-good firmware recovery path is tested.
- TFTP or SD deployment path is tested.
- Boot mode switch positions are documented.
- A single operator protocol is frozen.
