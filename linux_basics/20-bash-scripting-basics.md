# Bash Scripting Basics
# ---------------------
# This lesson introduces Bash scripting fundamentals:
#   - variables
#   - conditions
#   - loops
#   - functions
#   - script execution
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is a Bash Script?
# ---------------------------------------------------------------
# A Bash script is a text file containing commands that run in sequence.
# Scripts automate tasks such as:
#   - backups
#   - system checks
#   - deployments
#   - monitoring
#
# Bash scripts use the .sh extension (optional but recommended).

## Example script:
# #!/bin/bash
# echo "Hello, Worku!"

# ---------------------------------------------------------------
# 2. Creating and Running a Script
# ---------------------------------------------------------------

## Create a script
nano hello.sh

## Add:
# #!/bin/bash
# echo "Hello, Worku!"

## Make it executable
chmod +x hello.sh

## Run it
./hello.sh

# ---------------------------------------------------------------
# 3. Shebang Line
# ---------------------------------------------------------------
# The first line of a script is the "shebang":
# #!/bin/bash
#
# It tells Linux which interpreter to use.

# ---------------------------------------------------------------
# 4. Variables
# ---------------------------------------------------------------

## Define a variable
name="Worku"

## Use a variable
echo "Hello, $name"

## Read user input
echo "Enter your name:"
read username
echo "Welcome, $username"

# ---------------------------------------------------------------
# 5. Environment Variables
# ---------------------------------------------------------------

## Show all environment variables
printenv

## Access environment variable
echo $HOME

## Set environment variable (temporary)
export CITY="Copenhagen"

# ---------------------------------------------------------------
# 6. Arithmetic Operations
# ---------------------------------------------------------------

a=10
b=5

echo $((a + b))
echo $((a * b))
echo $((a / b))

# ---------------------------------------------------------------
# 7. Conditions (if / else)
# ---------------------------------------------------------------

## Basic if statement
if [ $a -gt $b ]; then
    echo "a is greater than b"
else
    echo "a is not greater than b"
fi

## Check if file exists
if [ -f /etc/passwd ]; then
    echo "File exists"
fi

## Check if directory exists
if [ -d /var/log ]; then
    echo "Directory exists"
fi

# ---------------------------------------------------------------
# 8. Comparison Operators
# ---------------------------------------------------------------

## Numbers:
# -eq  equal
# -ne  not equal
# -gt  greater than
# -lt  less than

## Strings:
# =    equal
# !=   not equal
# -z   empty string
# -n   not empty

# ---------------------------------------------------------------
# 9. Loops
# ---------------------------------------------------------------

## For loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

## For loop with range
for i in {1..5}; do
    echo "Count: $i"
done

## While loop
count=1
while [ $count -le 5 ]; do
    echo "Loop: $count"
    count=$((count + 1))
done

# ---------------------------------------------------------------
# 10. Functions
# ---------------------------------------------------------------

## Define a function
greet() {
    echo "Hello, $1"
}

## Call function
greet "Worku"

# ---------------------------------------------------------------
# 11. Script Arguments
# ---------------------------------------------------------------

## $1 = first argument
## $2 = second argument
## $@ = all arguments

## Example script:
# #!/bin/bash
# echo "First argument: $1"
# echo "All arguments: $@"

## Run:
# ./script.sh hello world

# ---------------------------------------------------------------
# 12. Exit Codes
# ---------------------------------------------------------------
# Exit codes indicate success or failure.
# 0 = success
# non‑zero = error

## Example:
if [ -f file.txt ]; then
    exit 0
else
    exit 1
fi

# ---------------------------------------------------------------
# 13. Practical Examples
# ---------------------------------------------------------------

## Backup script
# #!/bin/bash
# tar -czf /backup/home.tar.gz /home/workua

## Check disk usage
# #!/bin/bash
# df -h | grep /dev/sda1

## Ping test
# #!/bin/bash
# ping -c 3 google.com

# ---------------------------------------------------------------
# 14. Debugging Scripts
# ---------------------------------------------------------------

## Run script in debug mode
bash -x script.sh

## Add debug inside script
set -x     # enable debug
set +x     # disable debug

# ---------------------------------------------------------------
# 15. Summary
# ---------------------------------------------------------------
# - #!/bin/bash → shebang
# - chmod +x → make script executable
# - Variables → name="value"
# - Conditions → if / else
# - Loops → for, while
# - Functions → reusable code
# - Arguments → $1, $@
# - Debugging → bash -x
#
# Next lesson:
# → Bash Scripting (Intermediate): arrays, case, error handling
