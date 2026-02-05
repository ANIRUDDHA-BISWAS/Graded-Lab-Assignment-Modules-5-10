# Question 8 — Shell Script: bg_move.sh

## Objective
Create a shell script `bg_move.sh` that accepts a directory path and performs background file operations.

The script should:
- Move each file in the given directory into a subdirectory named `backup/`
- Perform each move operation **in the background**
- Display the **PID** of each background process
- **Wait** for all background processes to finish

Use `&`, `wait`, `$$`, and `$!` variables.

---

## Script File
**Filename:** bg_move.sh

---

## Script Code

```bash
#!/bin/bash

# Check if exactly one argument is provided
if [ "$#" -ne 1 ]; then
    echo "Error: Please provide a directory path."
    exit 1
fi

dir="$1"

# Check if directory exists
if [ ! -d "$dir" ]; then
    echo "Error: Directory does not exist."
    exit 1
fi

# Create backup directory if not exists
backup_dir="$dir/backup"
mkdir -p "$backup_dir"

echo "Parent Script PID: $$"
echo "Moving files in background..."

# Loop through files in directory
for file in "$dir"/*
do
    # Skip backup directory itself
    if [ "$file" != "$backup_dir" ] && [ -f "$file" ]; then
        mv "$file" "$backup_dir/" &
        pid=$!
        echo "Moved $(basename "$file") | Background PID: $pid"
    fi
done

# Wait for all background processes
wait

echo "All background move operations completed."

