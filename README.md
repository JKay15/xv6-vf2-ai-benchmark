# xv6-vf2-ai-benchmark

Benchmarking AI agents on xv6 lab completion and VisionFive 2 board porting.

## Overview

This repository packages the official xv6 lab starter branches, course documents, local tests, and benchmark protocols for evaluating AI agents across three stages:

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

- `stage1-common-src-xv6-k210-template-master-d740928.tar.gz`
- `stage1-common-src-xv6-k210-template-master-d740928.bundle`
- `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.tar.gz`
- `stage1-starter-xv6-k210-template-part4-scheduler-6d7ede9.bundle`
- `stage1-starter-xv6-k210-template-part5-mem-c93a75a.tar.gz`
- `stage1-starter-xv6-k210-template-part5-mem-c93a75a.bundle`
- `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.tar.gz`
- `stage1-starter-xv6-k210-template-part6-page-replace-0db77e0.bundle`
- `stage1-starter-xv6-k210-template-part8-sem-79ea868.tar.gz`
- `stage1-starter-xv6-k210-template-part8-sem-79ea868.bundle`
- `stage1-course-docs-20260320.tar.gz`
- `stage1-syscall-testsuite.tar.gz`

These assets correspond to the official framework repository and starter branches named in the course PDFs:

- Repo URL: `https://gitlab.eduxiji.net/pku2301210666/xv6-k210-template.git`
- Common framework branch: `master`
- Common framework commit: `d740928263beb3350c490128735ba63cb923a8c8`
- Official starter branches:
  - `part4-scheduler` at `6d7ede98c1c8d1a285f64a7b23ba82b9a30a4f36`
  - `part5-mem` at `c93a75a553456a18c09cbea74509bb313198e14b`
  - `part6-page-replace` at `0db77e036a603437e2b965ef0733ec82655b730c`
  - `part8-sem` at `79ea868c2e134144b155bfa4e035c1015ae10cc6`

The solved repository `xjk_2000012515_os.git@4c3c4b9` is a reference outcome, not a blind Stage 1 starting point.

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
