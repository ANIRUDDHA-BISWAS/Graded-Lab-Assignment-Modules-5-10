# Question 3 — Shell Script: validate_results.sh

## Objective
Write a shell script `validate_results.sh` that reads student data from a file named `marks.txt`.  
Each line of the file contains:

RollNo, Name, Marks1, Marks2, Marks3

The script should:
- Print students who **failed in exactly ONE subject**
- Print students who **passed in ALL subjects**
- Print the **count of students** in each category  

Passing marks for each subject is **33**.  
The script uses **loops, conditionals, and arithmetic operations**.

---

## Script File
**Filename:** validate_results.sh

---

## Script Code

```bash
#!/bin/bash

# Check if marks.txt exists
if [ ! -f marks.txt ]; then
    echo "Error: marks.txt file not found."
    exit 1
fi

pass_all=0
fail_one=0

echo "Students who failed in exactly ONE subject:"
echo "------------------------------------------"

while IFS=',' read -r roll name m1 m2 m3
do
    fail_count=0

    if [ "$m1" -lt 33 ]; then
        fail_count=$((fail_count + 1))
    fi
    if [ "$m2" -lt 33 ]; then
        fail_count=$((fail_count + 1))
    fi
    if [ "$m3" -lt 33 ]; then
        fail_count=$((fail_count + 1))
    fi

    if [ "$fail_count" -eq 1 ]; then
        echo "$roll, $name"
        fail_one=$((fail_one + 1))
    fi

    if [ "$fail_count" -eq 0 ]; then
        pass_all=$((pass_all + 1))
    fi

done < marks.txt

echo
echo "Students who passed in ALL subjects:"
echo "-----------------------------------"

while IFS=',' read -r roll name m1 m2 m3
do
    if [ "$m1" -ge 33 ] && [ "$m2" -ge 33 ] && [ "$m3" -ge 33 ]; then
        echo "$roll, $name"
    fi
done < marks.txt

echo
echo "Summary:"
echo "--------"
echo "Students passed in all subjects: $pass_all"
echo "Students failed in exactly one subject: $fail_one"

