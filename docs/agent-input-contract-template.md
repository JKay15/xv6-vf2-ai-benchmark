# Agent Input Contract Template

## Task

You are given a fixed baseline repository, a fixed document bundle, and a fixed operator protocol.

Your goals are:

1. Complete the lab tasks on the baseline repository.
2. Port the resulting OS to VisionFive 2.
3. Demonstrate the required milestones under the benchmark protocol.

## Fixed Inputs

- Baseline repo URL: `REPLACE_ME`
- Baseline branch: `REPLACE_ME`
- Baseline tag: `REPLACE_ME`
- Baseline commit: `REPLACE_ME`
- Course docs bundle: `REPLACE_ME`
- Repo docs bundle: `REPLACE_ME`
- Testsuite bundle: `REPLACE_ME`
- VisionFive 2 reference bundle: `REPLACE_ME`

## Allowed Actions

You may:

- inspect files
- edit files
- build the repo
- ask the operator to perform actions from the operator protocol
- request raw build logs and raw serial logs

## Forbidden Actions

You may not:

- assume hidden human fixes
- assume hidden firmware changes
- ask the operator to perform actions outside the published operator protocol

## Operator Actions Available

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

## Time Budget

- Stage 1 budget: `REPLACE_ME`
- Stage 2 budget: `REPLACE_ME`
- Stage 3 budget: `REPLACE_ME`

## Turn Budget

- Max agent turns: `REPLACE_ME`
- Max deployment attempts: `REPLACE_ME`
- Max recovery attempts: `REPLACE_ME`

## Stage 1 Success

- Defined by: `REPLACE_ME`

## Stage 2 Success

- Defined by milestone set: `REPLACE_ME`

## Stage 3 Success

- Defined by workload and scoring set: `REPLACE_ME`

## Output Requirements

You must produce:

- a branch or patchset
- clear build/deploy instructions
- explanation of assumptions
- raw-log-based evidence for milestone claims

