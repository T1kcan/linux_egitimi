# Mastering Linux Text & String Commands

This guide covers commonly used Linux text and string processing commands:  
`cat`, `echo`, `sort`, `uniq`, `paste`, `join`, `split`, `tr`, `tee`, `wc`, `less`, `head`, `tail`, and `strings`.

---

## 1. `cat` – Concatenate and Display Files

### Display file content
```bash
cat file.txt
```

### Display multiple files
```bash
cat file1.txt file2.txt
```

### Number all output lines
```bash
cat -n file.txt
```

### Show non-printable characters
```bash
cat -v file.txt
```

---

## 2. `echo` – Print Text

### Print a simple string
```bash
echo "Hello World"
```

### Print variables
```bash
NAME="Linux"
echo "Welcome to $NAME"
```

### Print with escape characters
```bash
echo -e "Line1\nLine2\nLine3"
```

### Suppress newline at the end
```bash
echo -n "Hello"
```

---

## 3. `sort` – Sort Text

### Sort a file alphabetically
```bash
sort file.txt
```

### Sort numerically
```bash
sort -n numbers.txt
```

### Reverse sort
```bash
sort -r file.txt
```

### Unique sort
```bash
sort -u file.txt
```

### Sort by specific column (delimiter `,`)
```bash
sort -t, -k2 file.csv
```

---

## 4. `uniq` – Report or Omit Repeated Lines

### Remove duplicate lines (input must be sorted first)
```bash
sort file.txt | uniq
```

### Count duplicate occurrences
```bash
sort file.txt | uniq -c
```

### Show only duplicate lines
```bash
sort file.txt | uniq -d
```

### Show only unique lines
```bash
sort file.txt | uniq -u
```

---

## 5. `paste` – Merge Lines of Files

### Merge two files side by side
```bash
paste file1.txt file2.txt
```

### Use a custom delimiter
```bash
paste -d, file1.txt file2.txt
```

---

## 6. `join` – Join Lines of Two Files on a Common Field

### Join files on the first field
```bash
join file1.txt file2.txt
```

### Join on a different field
```bash
join -1 2 -2 1 file1.txt file2.txt
```

### Use a custom delimiter
```bash
join -t, file1.csv file2.csv
```

---

## 7. `split` – Split Files

### Split file into chunks of 1000 lines each
```bash
split -l 1000 file.txt part_
```

### Split by file size (1MB each)
```bash
split -b 1M file.txt chunk_
```

### Split into equal parts (4 parts)
```bash
split -n 4 file.txt part_
```

---

## 8. `tr` – Translate or Delete Characters

### Convert lowercase to uppercase
```bash
tr 'a-z' 'A-Z' < file.txt
```

### Remove digits
```bash
tr -d '0-9' < file.txt
```

### Replace spaces with underscores
```bash
tr ' ' '_' < file.txt
```

### Squeeze repeated characters
```bash
tr -s ' ' < file.txt
```

---

## 9. `tee` – Read and Write to Files Simultaneously

### Save command output to a file while displaying it
```bash
ls | tee output.txt
```

### Append output to a file
```bash
ls | tee -a output.txt
```

### Use with multiple files
```bash
echo "Hello" | tee file1.txt file2.txt
```

---

## 10. `wc` – Word, Line, Character Count

### Count lines, words, characters
```bash
wc file.txt
```

### Count only lines
```bash
wc -l file.txt
```

### Count only words
```bash
wc -w file.txt
```

### Count only characters
```bash
wc -m file.txt
```

---

## 11. `less` – View Large Files

### Open a file with less
```bash
less file.txt
```

### Search inside less
- Forward search: `/pattern`
- Backward search: `?pattern`
- Next match: `n`
- Quit: `q`

### View command output with less
```bash
dmesg | less
```

---

## 12. `head` – Show Beginning of File

### Show first 10 lines (default)
```bash
head file.txt
```

### Show first 20 lines
```bash
head -n 20 file.txt
```

### Show first 100 bytes
```bash
head -c 100 file.txt
```

---

## 13. `tail` – Show End of File

### Show last 10 lines (default)
```bash
tail file.txt
```

### Show last 20 lines
```bash
tail -n 20 file.txt
```

### Show last 100 bytes
```bash
tail -c 100 file.txt
```

### Follow file in real-time (log monitoring)
```bash
tail -f logfile.log
```

---

## 14. `strings` – Extract Printable Text from Binary Files

### Extract strings from a binary file
```bash
strings binaryfile
```

### Extract with minimum string length of 5
```bash
strings -n 5 binaryfile
```

### Extract and search for specific pattern
```bash
strings binaryfile | grep "password"
```

---

# Summary

- **Viewing content**: `cat`, `less`, `head`, `tail`, `strings`  
- **Printing text**: `echo`, `tee`  
- **Sorting & filtering**: `sort`, `uniq`, `tr`  
- **File merging & splitting**: `paste`, `join`, `split`  
- **Counting**: `wc`  

These commands form the foundation of **Linux text processing and string manipulation** for sysadmins, developers, and data engineers.

