# VisionFive 2 Operator Protocol Template

## Operator Role

The operator executes only fixed actions and returns raw outputs. The operator does not debug for the agent.

## Allowed Commands

### Host-side

- `build`
- `deploy_sd`
- `deploy_tftp`
- `show_host_file <path>`

### Board-side

- `reset_board`
- `power_cycle`
- `capture_serial <seconds>`
- `interrupt_uboot`
- `run_uboot_script <script-name>`
- `show_board_log <path>`
- `recover_board`

## Response Format

Every operator response must include:

1. action name
2. exact command(s) executed
3. exact stdout/stderr or serial output
4. success/failure result

## Forbidden Operator Behavior

The operator must not:

- patch code
- edit configuration silently
- summarize raw logs unless the agent explicitly asks for a summary
- swap hardware or firmware baselines for one agent only

## Recovery Rule

If the board becomes unbootable:

1. follow the published recovery procedure
2. record that a recovery attempt was consumed
3. return the full recovery log

## Log Retention

Preserve for every run:

- build log
- deploy log
- serial log
- U-Boot transcript
- recovery log if any

