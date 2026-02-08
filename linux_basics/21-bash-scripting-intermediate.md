# Bash Scripting (Intermediate)
# -----------------------------
# This lesson expands your Bash scripting skills:
#   - arrays
#   - case statements
#   - error handling
#   - loops with files
#   - script structure
#   - colors and formatting
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Arrays
# ---------------------------------------------------------------

## Define an array
fruits=("apple" "banana" "orange")

## Access elements
echo ${fruits[0]}     # apple
echo ${fruits[2]}     # orange

## Add element
fruits+=("mango")

## Loop through array
for item in "${fruits[@]}"; do
    echo "Fruit: $item"
done

## Array length
echo ${#fruits[@]}

# ---------------------------------------------------------------
# 2. Associative Arrays (key-value)
# ---------------------------------------------------------------

## Enable associative arrays
declare -A user

## Add key-value pairs
user[name]="Worku"
user[city]="Copenhagen"
user[role]="Admin"

## Access values
echo ${user[name]}

## Loop through keys
for key in "${!user[@]}"; do
    echo "$key = ${user[$key]}"
done

# ---------------------------------------------------------------
# 3. case Statement
# ---------------------------------------------------------------

## Example menu
echo "Choose an option: start | stop | restart"
read action

case $action in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    *)
        echo "Invalid option"
        ;;
esac

# ---------------------------------------------------------------
# 4. Error Handling
# ---------------------------------------------------------------

## Exit on error
set -e

## Example:
cp file.txt /backup/ || echo "Copy failed"

## Custom error function
error() {
    echo "Error: $1"
    exit 1
}

## Use it
[ -f config.txt ] || error "config.txt not found"

# ---------------------------------------------------------------
# 5. Checking Exit Codes
# ---------------------------------------------------------------

## Run a command
ping -c 1 google.com

## Check exit code
if [ $? -eq 0 ]; then
    echo "Ping successful"
else
    echo "Ping failed"
fi

# ---------------------------------------------------------------
# 6. Reading Files Line by Line
# ---------------------------------------------------------------

## Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# ---------------------------------------------------------------
# 7. Functions with Return Values
# ---------------------------------------------------------------

add() {
    result=$(( $1 + $2 ))
    echo $result
}

sum=$(add 5 10)
echo "Sum: $sum"

# ---------------------------------------------------------------
# 8. Logging in Scripts
# ---------------------------------------------------------------

log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log "Backup started"
log "Backup completed"

# ---------------------------------------------------------------
# 9. Colors and Formatting
# ---------------------------------------------------------------

RED="\e[31m"
GREEN="\e[32m"
RESET="\e[0m"

echo -e "${GREEN}Success!${RESET}"
echo -e "${RED}Error!${RESET}"

# ---------------------------------------------------------------
# 10. Script Structure (Best Practice)
# ---------------------------------------------------------------

## Example template:
# #!/bin/bash
# set -e
#
# log() {
#     echo "$(date '+%Y-%m-%d %H:%M:%S') - $1"
# }
#
# main() {
#     log "Script started"
#     # your code here
#     log "Script finished"
# }
#
# main "$@"

# ---------------------------------------------------------------
# 11. Practical Examples
# ---------------------------------------------------------------

## 1. Check if a service is running
service="nginx"
if systemctl is-active --quiet $service; then
    echo "$service is running"
else
    echo "$service is NOT running"
fi

## 2. Loop through files
for file in *.log; do
    echo "Processing $file"
done

## 3. Simple menu script
while true; do
    echo "1) Show date"
    echo "2) Show uptime"
    echo "3) Exit"
    read choice

    case $choice in
        1) date ;;
        2) uptime ;;
        3) exit ;;
        *) echo "Invalid choice" ;;
    esac
done

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - Arrays → indexed and associative
# - case → cleaner than multiple if statements
# - set -e → exit on errors
# - Functions → reusable logic
# - Colors → improve script readability
# - Logging → track script actions
# - File loops → process logs, configs, etc.
#
# Next lesson:
# → Bash Scripting (Advanced): traps, signals, parallel jobs
