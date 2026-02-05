# Question 6 — Shell Script: metrics.sh

## Objective
Create a shell script `metrics.sh` that analyzes a text file named `input.txt`.

The script should display:

- Longest word  
- Shortest word  
- Average word length  
- Total number of unique words  

Use pipes and commands such as `tr`, `sort`, `uniq`, and `wc`.

---

## Script File
**Filename:** metrics.sh

---

## Script Code

```bash
#!/bin/bash

# Check if input.txt exists
if [ ! -f input.txt ]; then
    echo "Error: input.txt file not found."
    exit 1
fi

# Convert text to lowercase and extract words
words=$(tr -c 'A-Za-z0-9' '\n' < input.txt | tr 'A-Z' 'a-z' | grep -v '^$')

# Longest word
longest=$(echo "$words" | awk '{ if (length > max) { max = length; word = $0 } } END { print word }')

# Shortest word
shortest=$(echo "$words" | awk '{ if (NR == 1 || length < min) { min = length; word = $0 } } END { print word }')

# Average word length
avg=$(echo "$words" | awk '{ sum += length; count++ } END { if (count > 0) print sum / count; else print 0 }')

# Unique words count
unique=$(echo "$words" | sort | uniq | wc -l)

echo "Longest word: $longest"
echo "Shortest word: $shortest"
echo "Average word length: $avg"
echo "Total unique words: $unique"

