# Question 1 — Shell Script: analyze.sh

## Objective
Create a shell script `analyze.sh` that accepts exactly **one command-line argument** and performs the following tasks:

- If the argument is a **file**, display:
  - Number of lines
  - Number of words
  - Number of characters

- If the argument is a **directory**, display:
  - Total number of files
  - Number of `.txt` files

- If the argument count is invalid or the path does not exist, display an appropriate error message.

---

## Script File
**Filename:** analyze.sh

---

## Script Code

```bash
#!/bin/bash

# Check if exactly one argument is provided
if [ "$#" -ne 1 ]; then
    echo "Error: Please provide exactly one argument."
    exit 1
fi

# Check if path exists
if [ ! -e "$1" ]; then
    echo "Error: The given path does not exist."
    exit 1
fi

# If argument is a file
if [ -f "$1" ]; then
    echo "It is a file."

    lines=$(wc -l < "$1")
    words=$(wc -w < "$1")
    chars=$(wc -m < "$1")

    echo "Number of lines: $lines"
    echo "Number of words: $words"
    echo "Number of characters: $chars"

# If argument is a directory
elif [ -d "$1" ]; then
    echo "It is a directory."

    total_files=$(find "$1" -type f | wc -l)
    txt_files=$(find "$1" -type f -name "*.txt" | wc -l)

    echo "Total number of files: $total_files"
    echo "Number of .txt files: $txt_files"

else
    echo "Error: Invalid input."
fi

```

## Explanation of Execution Steps

### Step 1: Grant Execute Permission

The `chmod +x analyze.sh` command is used to give execute permission to the shell script.  
Without this permission, the script cannot be run directly as a program.

---

### Step 2: Execute Script with a File Argument

When the script is executed with a file name as an argument, it first verifies that the path exists and is a file.  
It then uses the `wc` command to calculate and display the number of lines, words, and characters in the file.

---

### Step 3: Execute Script with a Directory Argument

When a directory path is provided, the script confirms that the argument is a directory.  
It uses the `find` command to count the total number of files and the number of `.txt` files present in the directory.

---

### Step 4: Execute Script with Invalid Number of Arguments

If the script is executed without any arguments or with more than one argument, it checks the argument count.  
Since the requirement is exactly one argument, the script displays an appropriate error message and terminates.

---

### Step 5: Execute Script with a Non-Existing Path

Before performing any operation, the script checks whether the given path exists.  
If the path does not exist, the script displays an error message and stops execution to prevent incorrect processing.


