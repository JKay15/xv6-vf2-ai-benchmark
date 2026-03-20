# VisionFive 2 Benchmark Starting Assets Manifest

## 1. Baseline Identity

Use this exact baseline for every agent:

- Repository URL:
  - `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os.git`
- Branch:
  - `main`
- Tag:
  - `part4-8-all-pass`
- Full commit:
  - `4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3`
- Short commit:
  - `4c3c4b9`

Suggested GitLab links:

- Repo:
  - `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os`
- Branch tree:
  - `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os/-/tree/main`
- Tag:
  - `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os/-/tags`
- Commit:
  - `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os/-/commit/4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3`

## 2. Stage 1 Asset Bundle

Every Stage 1 participant should receive the same asset bundle.

### 2.1 Source bundle

Choose one standard distribution format and use it for all agents:

1. Git clone + fixed commit
2. Tarball from the fixed commit
3. Git bundle from the fixed commit

Recommended filenames:

- `stage1-src-xv6-k210-4c3c4b9.tar.gz`
- `stage1-src-xv6-k210-4c3c4b9.bundle`

### 2.2 Course document bundle

Include these local files:

- `/home/ubuntu/docs/0-2026-春季-课程简介-20260305上课版.pptx`
- `/home/ubuntu/docs/OS Lab希冀平台提交指南.pdf`
- `/home/ubuntu/docs/OS Lab本地环境配置简要指导.pdf`
- `/home/ubuntu/docs/OS实验课.pptx`
- `/home/ubuntu/docs/Part 4 功能性：进程调度算法.pdf`
- `/home/ubuntu/docs/Part 5 功能性：懒分配和写时复制.pdf`
- `/home/ubuntu/docs/Part 6 功能性：页面替换算法.pdf`
- `/home/ubuntu/docs/Part 7 系统调用：文件系统.pdf`
- `/home/ubuntu/docs/Part 8 功能性：IPC与信号量.pdf`
- `/home/ubuntu/docs/运行基础知识.pdf`

Recommended filename:

- `stage1-course-docs-20260320.zip`

### 2.3 Repository documentation bundle

Include the repository documentation tree:

- `/home/ubuntu/xv6-k210-submit-20260305/doc`

Notable files:

- `/home/ubuntu/xv6-k210-submit-20260305/doc/内核原理-内存管理.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/内核原理-系统调用.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/内核原理-进程管理.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/构建调试-系统调用.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/构建调试-文件系统.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/构建调试-进程管理.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/构建调试-时钟中断.md`
- `/home/ubuntu/xv6-k210-submit-20260305/doc/构建调试-开机启动.md`

Recommended filename:

- `stage1-repo-docs-4c3c4b9.zip`

### 2.4 Stage 1 testsuite bundle

Include the syscall testsuite tree:

- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing`

Important files:

- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/README.md`
- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/user/build-oscomp.sh`
- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/user/src/oscomp/test_runner.py`
- `/home/ubuntu/xv6-k210-submit-20260305/run-local-tests.sh`

Recommended filename:

- `stage1-syscall-testsuite.zip`

### 2.5 Stage 1 evaluation metadata

Include a short manifest with:

- required repo commit
- platform submission target description
- local test entrypoints
- allowed tools
- interaction budget

Recommended filename:

- `stage1-eval-manifest.json`

## 3. Stage 2 Asset Bundle

Stage 2 should not start from scratch. It should start from:

- one frozen Stage 1 snapshot per agent
- one shared board-operation kit
- one shared official VF2 reference bundle

### 3.1 Board-operation kit

Prepare once and reuse:

- known-good VisionFive 2 board
- known-good power adapter
- known-good USB-to-UART adapter
- known-good microSD card
- known-good Ethernet path if TFTP is used

### 3.2 Host-side Mac toolkit

Prepare once and freeze:

- `build.sh`
- `deploy_tftp.sh`
- `deploy_sd.sh`
- `capture_serial.sh`
- `interrupt_uboot.expect`
- `run_uboot_script.expect`
- `collect_logs.sh`
- `recover_vf2.md`

These scripts are benchmark infrastructure, not agent output.

### 3.3 Official VF2 reference bundle

Collect the official links into one PDF or Markdown manifest.

Must include:

- Boot flow
- Boot mode settings
- Boot address allocation
- U-Boot documentation
- Mac/Linux serial setup
- VF2 datasheet
- U-Boot `bootelf` command reference

Recommended filename:

- `stage2-vf2-official-links.md`

## 4. Stage 3 Asset Bundle

Stage 3 should reuse all Stage 2 assets and add:

- the frozen Stage 2 working branch per agent
- a stable workload definition
- a stable scoring script

Suggested workload bundle contents:

- serial demo workload
- timer/trap demo workload
- first-user-process demo workload
- storage-backed or memory-backed demo workload
- soak-run harness

Recommended filename:

- `stage3-workloads-and-scorer.zip`

## 5. Packaging Commands

### 5.1 Create source tarball

```bash
git -C /home/ubuntu/xv6-k210-submit-20260305 archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-src-xv6-k210-4c3c4b9.tar.gz \
  4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3
```

### 5.2 Create source bundle

```bash
git -C /home/ubuntu/xv6-k210-submit-20260305 bundle create \
  /home/ubuntu/stage1-src-xv6-k210-4c3c4b9.bundle \
  4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3
```

### 5.3 Create course docs zip

```bash
cd /home/ubuntu/docs && zip -r /home/ubuntu/stage1-course-docs-20260320.zip .
```

### 5.4 Create repo docs zip

```bash
cd /home/ubuntu/xv6-k210-submit-20260305 && \
zip -r /home/ubuntu/stage1-repo-docs-4c3c4b9.zip doc
```

### 5.5 Create testsuite zip

```bash
cd /home/ubuntu/testsuits-for-oskernel && \
zip -r /home/ubuntu/stage1-syscall-testsuite.zip riscv-syscalls-testing
```

## 6. Frozen Environment Data

Before benchmarking, record and distribute:

- Host machine:
  - model
  - macOS version
- UART adapter model
- Board revision
- Boot mode switch state
- Deployment method:
  - TFTP or SD
- Serial settings:
  - baud rate
  - data bits
  - parity
  - stop bits

This data should live in a shared file:

- `benchmark-host-and-board-baseline.md`

## 7. Agent Input Contract

For fairness, every agent should receive the same written prompt contract:

- task goal
- allowed artifacts
- allowed operator actions
- forbidden hidden human fixes
- time budget
- turn budget
- success criteria

Recommended filename:

- `agent-input-contract.md`

## 8. Minimum Distribution Set

If you want the smallest practical package, distribute at least:

1. `stage1-src-xv6-k210-4c3c4b9.tar.gz`
2. `stage1-course-docs-20260320.zip`
3. `stage1-repo-docs-4c3c4b9.zip`
4. `stage1-syscall-testsuite.zip`
5. `agent-input-contract.md`
6. `benchmark-host-and-board-baseline.md`
7. `stage2-vf2-official-links.md`

That is the minimum bundle that still preserves fairness across agents.

