# Mastering `grep` Command (Basic to Advanced)

The `grep` command in Linux/Unix is a powerful text searching utility. It searches input files for lines that match a given pattern and outputs the matching lines.

---

## 1. Basic Syntax
```bash
grep [OPTIONS] PATTERN [FILE...]
```

- `PATTERN` → text, regex, or expression to search for
- `FILE` → one or more files to search in

---

## 2. Basic Usage

### Search for a word in a file
```bash
grep "hello" file.txt
```

### Search ignoring case
```bash
grep -i "hello" file.txt
```

### Display line numbers with matches
```bash
grep -n "hello" file.txt
```

### Count the number of matches
```bash
grep -c "hello" file.txt
```

### Show only the matching part of the line
```bash
grep -o "hello" file.txt
```

---

## 3. Searching in Multiple Files

### Search in all `.txt` files
```bash
grep "hello" *.txt
```

### Recursive search in directories
```bash
grep -r "hello" /path/to/dir
```

### Show filenames with matches only
```bash
grep -l "hello" *.txt
```

### Show filenames without matches
```bash
grep -L "hello" *.txt
```

---

## 4. Regular Expressions with `grep`

### Lines starting with a word
```bash
grep "^hello" file.txt
```

### Lines ending with a word
```bash
grep "hello$" file.txt
```

### Match any single character
```bash
grep "h.llo" file.txt
```

### Match zero or more occurrences
```bash
grep "hel*o" file.txt
```

### Extended regex (use `-E`)
```bash
grep -E "hello|world" file.txt
```

---

## 5. Useful Options

| Option | Description |
|--------|-------------|
| `-i`   | Ignore case |
| `-v`   | Invert match (show non-matching lines) |
| `-n`   | Show line numbers |
| `-c`   | Count matches |
| `-o`   | Show only the matching text |
| `-r`   | Recursive search |
| `-l`   | Show file names with matches |
| `-L`   | Show file names without matches |
| `--color` | Highlight matches |

Example (highlight matches):
```bash
grep --color "hello" file.txt
```

---

## 6. Advanced Examples

### Search multiple patterns
```bash
grep -E "error|warning|critical" logfile.log
```

### Invert search (show lines without "error")
```bash
grep -v "error" logfile.log
```

### Count occurrences of multiple patterns
```bash
grep -E -c "error|warning" logfile.log
```

### Case-insensitive recursive search for IP addresses
```bash
grep -riE "([0-9]{1,3}\.){3}[0-9]{1,3}" /var/log/
```

### Display 3 lines before and after a match (context)
```bash
grep -C 3 "error" logfile.log
```

### Display only lines that are exactly "hello"
```bash
grep -x "hello" file.txt
```

---

## 7. Combining with Other Commands

### Using `grep` with `ps`
```bash
ps aux | grep "nginx"
```

### Using `grep` with `dmesg`
```bash
dmesg | grep -i "usb"
```

### Using `grep` with `ls`
```bash
ls -l | grep "^d"
```

### Using `grep` with `tail`
```bash
tail -f logfile.log | grep "error"
```

---

## 8. Practical Use Cases

### Find failed login attempts
```bash
grep "Failed password" /var/log/auth.log
```

### Extract email addresses from a file
```bash
grep -E -o "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-z]{2,}" file.txt
```

### Search for phone numbers
```bash
grep -E -o "[0-9]{3}-[0-9]{3}-[0-9]{4}" file.txt
```

### Find all IP addresses in logs
```bash
grep -E -o "([0-9]{1,3}\.){3}[0-9]{1,3}" logfile.log
```

---

## 9. Grep Variants

- **egrep** → same as `grep -E` (extended regex)
- **fgrep** → same as `grep -F` (fixed strings, no regex)
- **zgrep** → search inside compressed `.gz` files

Example:
```bash
zgrep "error" logfile.gz
```

---

# Summary

- `grep` is essential for searching text and logs.
- Supports **basic & extended regex**.
- Can be combined with other commands in pipelines.
- Useful for **system admins, developers, and data analysts**.

