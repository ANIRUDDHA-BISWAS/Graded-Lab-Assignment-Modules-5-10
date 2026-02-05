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

