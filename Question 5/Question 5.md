# Question 5 — Shell Script: sync.sh

## Objective
Create a shell script `sync.sh` to compare two directories `dirA` and `dirB`.

The script should:

- List files present **only in dirA**
- List files present **only in dirB**
- For files with the **same name in both directories**, check whether their contents match  

The script must **NOT copy or modify files**.  
Use `diff`, `cmp`, or checksum techniques.

---

## Script File
**Filename:** sync.sh

---

## Script Code

```bash
#!/bin/bash

# Check if both directories exist
if [ ! -d dirA ] || [ ! -d dirB ]; then
    echo "Error: One or both directories (dirA, dirB) do not exist."
    exit 1
fi

echo "Files only in dirA:"
echo "-------------------"
for file in dirA/*
do
    fname=$(basename "$file")
    if [ ! -e "dirB/$fname" ]; then
        echo "$fname"
    fi
done

echo
echo "Files only in dirB:"
echo "-------------------"
for file in dirB/*
do
    fname=$(basename "$file")
    if [ ! -e "dirA/$fname" ]; then
        echo "$fname"
    fi
done

echo
echo "Comparing common files:"
echo "----------------------"

for file in dirA/*
do
    fname=$(basename "$file")

    if [ -e "dirB/$fname" ]; then
        if cmp -s "dirA/$fname" "dirB/$fname"; then
            echo "$fname : Contents match"
        else
            echo "$fname : Contents differ"
        fi
    fi
done

