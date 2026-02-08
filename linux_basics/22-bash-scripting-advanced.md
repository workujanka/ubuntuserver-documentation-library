# Bash Scripting (Advanced)
# -------------------------
# This lesson covers advanced Bash scripting concepts:
#   - traps & signals
#   - background jobs & parallel execution
#   - subshells
#   - process substitution
#   - advanced error handling
#   - script optimization
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Traps & Signals
# ---------------------------------------------------------------
# A trap lets your script respond to signals such as:
#   - SIGINT (Ctrl+C)
#   - SIGTERM (kill)
#   - EXIT (script ending)

## Example: cleanup on exit
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tempfile
}
trap cleanup EXIT

## Example: ignore Ctrl+C
trap "" SIGINT

## Example: custom message on Ctrl+C
trap "echo 'Ctrl+C pressed!'" SIGINT

# ---------------------------------------------------------------
# 2. Background Jobs (&)
# ---------------------------------------------------------------

## Run a command in the background
sleep 10 &

## Get job ID
jobs

## Bring job to foreground
fg %1

## Send job to background
bg %1

# ---------------------------------------------------------------
# 3. Parallel Execution
# ---------------------------------------------------------------

## Run tasks in parallel
task1() { sleep 2; echo "Task 1 done"; }
task2() { sleep 3; echo "Task 2 done"; }

task1 &
task2 &
wait        # wait for all background jobs

echo "All tasks completed"

# ---------------------------------------------------------------
# 4. Subshells
# ---------------------------------------------------------------
# Commands inside parentheses run in a subshell.

## Example:
(
    cd /tmp
    echo "Inside subshell: $(pwd)"
)

echo "Outside subshell: $(pwd)"

# ---------------------------------------------------------------
# 5. Command Substitution
# ---------------------------------------------------------------

## Capture output of a command
date_now=$(date)
echo "Current date: $date_now"

## Using backticks (older style)
files=`ls`
echo $files

# ---------------------------------------------------------------
# 6. Process Substitution
# ---------------------------------------------------------------
# Useful for comparing or combining streams.

## Compare two files
diff <(sort file1.txt) <(sort file2.txt)

## Combine outputs
paste <(ls /etc) <(ls /var)

# ---------------------------------------------------------------
# 7. Advanced Error Handling
# ---------------------------------------------------------------

## Exit on error
set -e

## Exit if any variable is undefined
set -u

## Exit if any command in a pipeline fails
set -o pipefail

## Example:
set -euo pipefail

# ---------------------------------------------------------------
# 8. Logging with Levels
# ---------------------------------------------------------------

log() {
    level=$1
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*"
}

log INFO "Script started"
log ERROR "Something went wrong"

# ---------------------------------------------------------------
# 9. Timing Commands
# ---------------------------------------------------------------

## Measure execution time
start=$(date +%s)

sleep 2

end=$(date +%s)
echo "Execution time: $((end - start)) seconds"

# ---------------------------------------------------------------
# 10. Reading Command Output Line-by-Line
# ---------------------------------------------------------------

## Example: parse system processes
ps aux | while read line; do
    echo "Process: $line"
done

# ---------------------------------------------------------------
# 11. Creating Temporary Files
# ---------------------------------------------------------------

## Create a secure temp file
tmpfile=$(mktemp)
echo "Temp file: $tmpfile"

## Write to it
echo "Hello" > "$tmpfile"

## Remove it
rm -f "$tmpfile"

# ---------------------------------------------------------------
# 12. Using getopts for Script Arguments
# ---------------------------------------------------------------

## Example script:
# #!/bin/bash
# while getopts "u:p:" opt; do
#     case $opt in
#         u) user=$OPTARG ;;
#         p) pass=$OPTARG ;;
#         *) echo "Invalid option" ;;
#     esac
# done
#
# echo "User: $user"
# echo "Pass: $pass"

## Run:
# ./script.sh -u worku -p secret

# ---------------------------------------------------------------
# 13. Advanced Menu System
# ---------------------------------------------------------------

select option in "Show Date" "Show Uptime" "Exit"; do
    case $option in
        "Show Date") date ;;
        "Show Uptime") uptime ;;
        "Exit") break ;;
        *) echo "