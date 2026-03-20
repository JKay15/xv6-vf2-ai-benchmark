# xv6-vf2-ai-benchmark

Benchmarking AI agents on xv6 lab completion and VisionFive 2 board porting.

## Overview

This repository packages a fixed xv6 lab baseline, course documents, local tests, and benchmark protocols for evaluating AI agents across three stages:

1. Lab correctness on the original xv6-k210 coursework.
2. Minimal real-board bring-up on StarFive VisionFive 2.
3. Full hardware-side functionality and stability evaluation.

The benchmark is intentionally split this way so that lab-solving ability, board bring-up ability, and end-to-end engineering quality can be compared separately.

## Repository Layout

- `baseline/`
  - frozen Stage 1 starting assets distributed to every agent
- `docs/`
  - benchmark protocol, VF2 port plan, templates, and official reference links
- `host-tools/`
  - Mac-side helper scripts for build, deploy, serial capture, and U-Boot interaction
- `agents/`
  - per-agent notes, branches, or result manifests
- `results/`
  - collected logs, score sheets, and reproduced runs

## Included Stage 1 Assets

The `baseline/` directory currently contains:

- `stage1-src-xv6-k210-4c3c4b9.tar.gz`
- `stage1-src-xv6-k210-4c3c4b9.bundle`
- `stage1-course-docs-20260320.tar.gz`
- `stage1-repo-docs-4c3c4b9.tar.gz`
- `stage1-syscall-testsuite.tar.gz`

These assets correspond to the fixed xv6-k210 baseline:

- Repo URL: `https://gitlab.eduxiji.net/pku2000012515/xjk_2000012515_os.git`
- Branch: `main`
- Tag: `part4-8-all-pass`
- Commit: `4c3c4b9a670aaf9f795912caebd3ffcf4edc4cd3`

## Key Documents

Start with:

- `docs/visionfive2-ai-benchmark.md`
- `docs/visionfive2-port-plan.md`
- `docs/visionfive2-starting-assets-manifest.md`

Supporting templates:

- `docs/agent-input-contract-template.md`
- `docs/benchmark-host-and-board-baseline-template.md`
- `docs/visionfive2-operator-protocol-template.md`
- `docs/vf2-official-links.md`

## Notes

- The upstream baseline is an `xv6-k210` tree, not a VisionFive 2 port.
- VisionFive 2 support is therefore a real board-porting task, not a simple deployment step.
- The benchmark assumes a fixed, known-good VF2 firmware chain and compares the OS payload path on top of it.

