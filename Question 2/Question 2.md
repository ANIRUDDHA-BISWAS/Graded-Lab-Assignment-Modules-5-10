# Question 2 — Shell Script: log_analyzer.sh

## Objective
Create a shell script `log_analyzer.sh` that processes a log file with entries in the format:

YYYY-MM-DD HH:MM:SS LEVEL MESSAGE

The script should:
- Accept the log file name as a command-line argument
- Validate that the file exists and is readable
- Count total log entries
- Count number of INFO, WARNING, and ERROR messages
- Display the most recent ERROR message
- Generate a report file named `logsummary_<date>.txt`
- Handle errors gracefully with meaningful messages

---

## Script File
**Filename:** log_analyzer.sh

---

## Script Code

```bash
#!/bin/bash

# Check if exactly one argument is provided
if [ "$#" -ne 1 ]; then
    echo "Error: Please provide exactly one log file."
    exit 1
fi

logfile="$1"

# Check if file exists
if [ ! -f "$logfile" ]; then
    echo "Error: File does not exist."
    exit 1
fi

# Check if file is readable
if [ ! -r "$logfile" ]; then
    echo "Error: File is not readable."
    exit 1
fi

# Count total log entries
total=$(wc -l < "$logfile")

# Count INFO, WARNING, ERROR
info_count=$(grep -c " INFO " "$logfile")
warning_count=$(grep -c " WARNING " "$logfile")
error_count=$(grep -c " ERROR " "$logfile")

# Get most recent ERROR message
recent_error=$(grep " ERROR " "$logfile" | tail -n 1)

# Display results
echo "Total log entries: $total"
echo "INFO messages: $info_count"
echo "WARNING messages: $warning_count"
echo "ERROR messages: $error_count"

if [ -n "$recent_error" ]; then
    echo "Most recent ERROR: $recent_error"
else
    echo "No ERROR messages found."
fi

# Generate report file
date_today=$(date +%Y-%m-%d)
report_file="logsummary_${date_today}.txt"

echo "Log Summary Report - $date_today" > "$report_file"
echo "--------------------------------" >> "$report_file"
echo "Total log entries: $total" >> "$report_file"
echo "INFO messages: $info_count" >> "$report_file"
echo "WARNING messages: $warning_count" >> "$report_file"
echo "ERROR messages: $error_count" >> "$report_file"

if [ -n "$recent_error" ]; then
    echo "Most recent ERROR: $recent_error" >> "$report_file"
else
    echo "No ERROR messages found." >> "$report_file"
fi

echo "Report generated: $report_file"
```
