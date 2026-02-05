# Question 7 — Shell Script: patterns.sh

## Objective
Write a shell script `patterns.sh` that reads a text file and classifies words based on patterns.

The script should:

- Write words containing **ONLY vowels** into `vowels.txt`
- Write words containing **ONLY consonants** into `consonants.txt`
- Write words containing **both vowels and consonants but starting with a consonant** into `mixed.txt`

Case should be **ignored** while checking patterns.

---

## Script File
**Filename:** patterns.sh

---

## Script Code

```bash
#!/bin/bash

# Check if input file is provided
if [ "$#" -ne 1 ]; then
    echo "Error: Please provide a text file as argument."
    exit 1
fi

file="$1"

# Check if file exists
if [ ! -f "$file" ]; then
    echo "Error: File not found."
    exit 1
fi

# Clear output files
> vowels.txt
> consonants.txt
> mixed.txt

# Extract words (case-insensitive)
words=$(tr -c 'A-Za-z' '\n' < "$file" | tr 'A-Z' 'a-z' | grep -v '^$')

for word in $words
do
    # Only vowels
    if echo "$word" | grep -Eq '^[aeiou]+$'; then
        echo "$word" >> vowels.txt

    # Only consonants
    elif echo "$word" | grep -Eq '^[bcdfghjklmnpqrstvwxyz]+$'; then
        echo "$word" >> consonants.txt

    # Mixed (both vowels and consonants, starting with consonant)
    elif echo "$word" | grep -Eq '^[bcdfghjklmnpqrstvwxyz][a-z]*$' && \
         echo "$word" | grep -Eq '[aeiou]' && \
         echo "$word" | grep -Eq '[bcdfghjklmnpqrstvwxyz]'; then
        echo "$word" >> mixed.txt
    fi
done

echo "Processing completed."
echo "Vowel-only words stored in vowels.txt"
echo "Consonant-only words stored in consonants.txt"
echo "Mixed pattern words stored in mixed.txt"

