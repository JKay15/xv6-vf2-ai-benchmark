# VisionFive 2 Benchmark Starting Assets Manifest

## 1. Stage 1 Benchmark Baseline

This benchmark intentionally uses one shared Stage 1 starter package:

- Framework repo:
  - `https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git`
- Branch:
  - `master`
- Commit:
  - `d740928263beb3350c490128735ba63cb923a8c8`
- Distributed source package:
  - `stage1-common-src-xv6-k210-template-master-d740928.tar.gz`

Why this is the chosen baseline:

- It is the common framework snapshot from which the remaining work can be compared under one fixed starting point.
- It avoids splitting the benchmark into four separate per-part starter packages.

Important clarification:

- The course PDFs for Part 4/5/6/8 do mention part-specific starter branches.
- This repository does **not** package those branches as Stage 1 inputs.
- That is a benchmark design choice, not a claim about the original course workflow.

## 2. Explicitly Not Used as Stage 1 Input

Do not use the completed repository below as a blind starting asset:

- `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os.git`
- branch `main`
- tag `part4-8-all-pass`
- commit `4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3`

That repository is a solved outcome and can only be used as a reference result outside blind evaluation.

## 3. Stage 1 Asset Bundle

Every Stage 1 participant should receive the same package.

### 3.1 Source package

File:

- `stage1-common-src-xv6-k210-template-master-d740928.tar.gz`

### 3.2 Course document bundle

File:

- `stage1-course-docs-20260320.tar.gz`

Contents come from:

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

### 3.3 Stage 1 testsuite bundle

File:

- `stage1-syscall-testsuite.tar.gz`

Contents come from:

- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing`

Use:

- local reproducibility aid
- reference for the syscall testing environment

### 3.4 Stage 1 evaluation metadata

Include a short manifest with:

- repo URL
- fixed branch
- exact commit
- local test entrypoints
- allowed tools
- interaction budget

Recommended filename:

- `stage1-eval-manifest.json`

## 4. Stage 2 Asset Bundle

Stage 2 should start from:

- one frozen Stage 1 snapshot per agent
- one shared board-operation kit
- one shared official VF2 reference bundle

### 4.1 Board-operation kit

Prepare once and reuse:

- known-good VisionFive 2 board
- known-good power adapter
- known-good USB-to-UART adapter
- known-good microSD card
- known-good Ethernet path if TFTP is used

### 4.2 Host-side Mac toolkit

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

### 4.3 Official VF2 reference bundle

Collect the official links into one Markdown manifest.

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

## 5. Stage 3 Asset Bundle

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

## 6. Packaging Commands

### 6.1 Clone the framework repo

```bash
git clone https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git
cd xv6-k210-template
```

### 6.2 Create Stage 1 source tarball

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-common-src-xv6-k210-template-master-d740928.tar.gz \
  d740928263beb3350c490128735ba63cb923a8c8
```

### 6.3 Create course docs tarball

```bash
cd /home/ubuntu/docs && tar -czf /home/ubuntu/stage1-course-docs-20260320.tar.gz .
```

### 6.4 Create testsuite tarball

```bash
cd /home/ubuntu/testsuits-for-oskernel && \
tar -czf /home/ubuntu/stage1-syscall-testsuite.tar.gz riscv-syscalls-testing
```

## 7. Frozen Environment Data

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
