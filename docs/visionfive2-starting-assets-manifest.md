# VisionFive 2 Benchmark Starting Assets Manifest

## 1. Correct Stage 1 Baseline

The completed repository below is **not** a Stage 1 starting asset:

- `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os.git`
- branch `main`
- tag `part4-8-all-pass`
- commit `4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3`

That repository is a solved outcome. It already contains the completed kernel work and should only be treated as a reference result outside blind evaluation.

The real course starter comes from the official framework repository named in the Part 4/5/6/8 PDFs:

- Framework repo:
  - `https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git`
- Common framework branch:
  - `master`
- Common framework commit:
  - `d740928263beb3350c490128735ba63cb923a8c8`

Official remaining-lab starter branches:

- Part 4:
  - branch `part4-scheduler`
  - commit `6d7ede98c1c8d1a285f64a7b23ba82b9a30a4f36`
- Part 5:
  - branch `part5-mem`
  - commit `c93a75a553456a18c09cbea74509bb313198e14b`
- Part 6:
  - branch `part6-page-replace`
  - commit `0db77e036a603437e2b965ef0733ec82655b730c`
- Part 8:
  - branch `part8-sem`
  - commit `79ea868c2e134144b155bfa4e035c1015ae10cc6`

Course-document evidence:

- `Part 4 功能性：进程调度算法.pdf`
- `Part 5 功能性：懒分配和写时复制.pdf`
- `Part 6 功能性：页面替换算法.pdf`
- `Part 8 功能性：IPC与信号量.pdf`

Each of those PDFs explicitly names the framework repo and its required starter branch.

## 2. How Stage 1 Should Be Run

Do **not** give agents the solved `4c3c4b9` repository as their Stage 1 input.

Use one of these two fair modes:

1. Course-faithful mode
   - Run four independent Stage 1 subtasks.
   - Each subtask starts from its official branch:
     - Part 4 from `part4-scheduler`
     - Part 5 from `part5-mem`
     - Part 6 from `part6-page-replace`
     - Part 8 from `part8-sem`
2. Shared-framework mode
   - Give agents the template `master` branch as the common framework snapshot.
   - Also provide the four official starter-branch bundles.
   - When scoring correctness, still map each result back to the official part branch it should have started from.

Recommendation:

- Use course-faithful mode for formal comparisons.
- Use shared-framework mode only when you want a single package for convenience.

## 3. Stage 1 Asset Bundle

Every Stage 1 participant should receive the same package.

### 3.1 Common framework source package

Files:

- `stage1-common-src-xv6-k210-template-master-d740928.tar.gz`
- `stage1-common-src-xv6-k210-template-master-d740928.bundle`

Purpose:

- Represents the common framework state after the earlier course material.
- This matches the observation that Part 0-3 and Part 7 are already present in the template-level codebase.

### 3.2 Official starter-branch packages

Files:

- `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.tar.gz`
- `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.bundle`
- `stage1-starter-xv6-k210-template-part5-mem-c93a75a.tar.gz`
- `stage1-starter-xv6-k210-template-part5-mem-c93a75a.bundle`
- `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.tar.gz`
- `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.bundle`
- `stage1-starter-xv6-k210-template-part8-sem-79ea868.tar.gz`
- `stage1-starter-xv6-k210-template-part8-sem-79ea868.bundle`

Purpose:

- These are the official course starting points for the remaining kernel lab parts.
- They should be the primary correctness baselines for AI comparison.

### 3.3 Course document bundle

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

- `stage1-course-docs-20260320.tar.gz`

### 3.4 Stage 1 testsuite bundle

Include the syscall testsuite tree:

- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing`

Important files:

- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/README.md`
- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/user/build-oscomp.sh`
- `/home/ubuntu/testsuits-for-oskernel/riscv-syscalls-testing/user/src/oscomp/test_runner.py`

Use this as:

- a reference for the Part 7 syscall environment already reflected in the template framework
- a reproducibility aid for local checking

Do not treat it as proof that Part 7 is still an unfinished Stage 1 task.

Recommended filename:

- `stage1-syscall-testsuite.tar.gz`

### 3.5 Explicitly excluded from blind Stage 1

Do not distribute these as starting assets during blind evaluation:

- the solved repository `xjk_2000012515_os.git@4c3c4b9`
- the solved repository's `doc/` directory if it contains explanation or solution-like material
- any all-pass patch bundle produced after the lab was completed

### 3.6 Stage 1 evaluation metadata

Include a short manifest with:

- repo URL
- allowed starter branch
- exact commit
- platform submission target description
- local test entrypoints
- allowed tools
- interaction budget

Recommended filename:

- `stage1-eval-manifest.json`

## 4. Stage 2 Asset Bundle

Stage 2 should not start from scratch. It should start from:

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

### 6.1 Clone the official framework repo

```bash
git clone https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git
cd xv6-k210-template
```

### 6.2 Create common framework tarball

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-common-src-xv6-k210-template-master-d740928.tar.gz \
  d740928263beb3350c490128735ba63cb923a8c8
```

### 6.3 Create common framework bundle

```bash
git bundle create \
  /home/ubuntu/stage1-common-src-xv6-k210-template-master-d740928.bundle \
  master
```

### 6.4 Create official starter-branch tarballs

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.tar.gz \
  6d7ede98c1c8d1a285f64a7b23ba82b9a30a4f36
```

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-starter-xv6-k210-template-part5-mem-c93a75a.tar.gz \
  c93a75a553456a18c09cbea74509bb313198e14b
```

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.tar.gz \
  0db77e036a603437e2b965ef0733ec82655b730c
```

```bash
git archive \
  --format=tar.gz \
  --output=/home/ubuntu/stage1-starter-xv6-k210-template-part8-sem-79ea868.tar.gz \
  79ea868c2e134144b155bfa4e035c1015ae10cc6
```

### 6.5 Create official starter-branch bundles

```bash
git bundle create \
  /home/ubuntu/stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.bundle \
  origin/part4-scheduler
```

```bash
git bundle create \
  /home/ubuntu/stage1-starter-xv6-k210-template-part5-mem-c93a75a.bundle \
  origin/part5-mem
```

```bash
git bundle create \
  /home/ubuntu/stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.bundle \
  origin/part6-page-replace
```

```bash
git bundle create \
  /home/ubuntu/stage1-starter-xv6-k210-template-part8-sem-79ea868.bundle \
  origin/part8-sem
```

### 6.6 Create course docs tarball

```bash
cd /home/ubuntu/docs && tar -czf /home/ubuntu/stage1-course-docs-20260320.tar.gz .
```

### 6.7 Create testsuite tarball

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
