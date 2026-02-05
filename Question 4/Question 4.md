# Question 4 — Shell Script: emailcleaner.sh

## Objective
Create a shell script `emailcleaner.sh` that processes a file named `emails.txt`.  
The script should:

- Extract all **valid email addresses** and store them in `valid.txt`
- Extract **invalid email addresses** and store them in `invalid.txt`
- Remove **duplicate entries** from `valid.txt`

Valid email format:  
`<letters_and_digits>@<letters>.com`

The script uses `grep`, `sort`, `uniq`, and output redirection.

---

## Script File
**Filename:** emailcleaner.sh

---

## Script Code

```bash
#!/bin/bash

# Check if emails.txt exists
if [ ! -f emails.txt ]; then
    echo "Error: emails.txt file not found."
    exit 1
fi

# Extract valid email addresses
grep -E '^[A-Za-z0-9]+@[A-Za-z]+\.com$' emails.txt > temp_valid.txt

# Remove duplicates from valid emails
sort temp_valid.txt | uniq > valid.txt

# Extract invalid email addresses
grep -v -E '^[A-Za-z0-9]+@[A-Za-z]+\.com$' emails.txt > invalid.txt

# Remove temporary file
rm temp_valid.txt

echo "Email processing completed."
echo "Valid emails stored in valid.txt"
echo "Invalid emails stored in invalid.txt"

