# AWK Complete Hands-on Lab Guide

This lab will take you from **basic to advanced AWK usage** step by step.  
You can run all commands on any Linux machine.  

---

## Objectives
- Learn `awk` syntax
- Work with fields (`$1`, `$2`, etc.)
- Perform filtering, calculations, and text processing
- Use `awk` with system commands and logs
- Build real-world monitoring examples

---

## 1 Introduction to AWK

AWK processes text **line by line** and splits each line into **fields** (default separator: whitespace).

- `$0` → entire line
- `$1`, `$2`, … → fields
- `NF` → number of fields
- `NR` → line number

---

## 2 Create a Test File

```bash
cat > data.txt <<EOF
Ali Veli 25
Ayse Yilmaz 30
Mehmet Kaya 40
Fatma Demir 35
EOF
```

---

## 3 Printing Fields

```bash
awk '{print $1}' data.txt
```
 Prints the first column (names).

```bash
awk '{print $1, $3}' data.txt
```
 Prints the first and third columns (name + age).

---

## 4 NR, NF, and $0

```bash
awk '{print NR, NF, $0}' data.txt
```
- `NR` = line number
- `NF` = number of fields
- `$0` = entire line

---

## 5 Filtering with Conditions

```bash
awk '$3 > 30 {print $1, $2, $3}' data.txt
```
 Prints people older than 30.

```bash
awk '$3 <= 30 {print $0}' data.txt
```
 Prints people age 30 or less.

---

## 6 Mathematical Operations

### Sum of Ages
```bash
awk '{sum += $3} END {print "Total Age:", sum}' data.txt
```

### Average Age
```bash
awk '{sum += $3} END {print "Average Age:", sum/NR}' data.txt
```

### Maximum Age
```bash
awk 'max<$3 {max=$3; name=$1} END {print "Oldest:", name, max}' data.txt
```

---

## 7 Using Different Field Separators

If fields are separated by `:` (e.g., `/etc/passwd`):

```bash
awk -F: '{print $1, $3}' /etc/passwd
```
 Prints username and UID.

---

## 8 System Monitoring Examples

### Disk Usage Alert
```bash
df -h | awk 'NR==1 || $5+0 > 70 {print $0}'
```
 Show header and partitions with >70% usage.

### RAM Usage
```bash
free -m | awk 'NR==2 {printf "RAM Usage: %.2f%%\n", $3*100/$2}'
```

### CPU Usage
```bash
top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}'
```
 Shows CPU usage (user + system).

---

## 9 Log Analysis Examples

### Count Errors in Logs
```bash
grep "ERROR" /var/log/syslog | awk '{c++} END {print "Total errors:", c}'
```

### Top IP Addresses from Access Log
```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head
```

---

## 10 Advanced AWK Usage

### Format Output with printf
```bash
awk '{printf "%-10s %-10s %d\n", $1, $2, $3}' data.txt
```
 Nicely formatted table.

### Multiple Actions
```bash
awk '$3 > 30 {print $1 " is older than 30"} $3 <= 30 {print $1 " is 30 or younger"}' data.txt
```

### Custom Output Separator
```bash
awk 'BEGIN {OFS=";"} {print $1, $2, $3}' data.txt
```
 Fields separated by `;`.

---

## Exercises

1. Add more entries into `data.txt` with different ages.  
2. Print only names of people younger than 28.  
3. Find the youngest person.  
4. Print only even ages.  
5. In `/etc/passwd`, print usernames with UID > 1000.  

---

## Conclusion
- Use `awk` for parsing text, system monitoring, and log analysis.  
- Combine `awk` with other tools (`grep`, `sort`, `uniq`) for powerful pipelines.  
- For large automation scripts, `awk` can replace small Python/Perl programs.  

---
