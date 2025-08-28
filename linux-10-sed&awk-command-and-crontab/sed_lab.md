# SED Complete Hands-on Lab Guide

This lab will take you from **basic to advanced `sed` usage** step by step.  
You can run all commands on any Linux machine.  

---

## Objectives
- Learn `sed` syntax
- Perform text substitutions
- Insert, delete, and transform lines
- Use regex with `sed`
- Build real-world text processing examples

---

## 1 Introduction to `sed`

`sed` (Stream EDitor) is a non-interactive text editor.  
It reads input line by line, applies editing commands, and outputs the result.

**Basic syntax:**
```bash
sed 'command' filename
```

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

## 3 Basic Substitution

### Replace a Word
```bash
sed 's/Ali/Ahmet/' data.txt
```
👉 Replaces first occurrence of `Ali` with `Ahmet`.

### Replace All Occurrences
```bash
sed 's/a/A/g' data.txt
```
👉 Replaces all `a` with `A`.

---

## 4 In-place Editing

To modify the file directly:
```bash
sed -i 's/Mehmet/Murat/' data.txt
```

---

## 5 Deleting Lines

### Delete Specific Line
```bash
sed '2d' data.txt
```
👉 Deletes line 2.

### Delete Line Range
```bash
sed '2,3d' data.txt
```
👉 Deletes lines 2 to 3.

### Delete Matching Pattern
```bash
sed '/Ayse/d' data.txt
```
👉 Deletes lines containing `Ayse`.

---

## 6 Insert and Append

### Insert Before Line
```bash
sed '2i\New Person 50' data.txt
```

### Append After Line
```bash
sed '2a\Another Person 60' data.txt
```

---

## 7 Print Specific Lines

```bash
sed -n '1,2p' data.txt
```
👉 Prints only lines 1 and 2.

```bash
sed -n '/Fatma/p' data.txt
```
👉 Prints only lines with `Fatma`.

---

## 8 Using Regular Expressions

### Replace Only at Line End
```bash
sed 's/[0-9]$/AGE/' data.txt
```

### Remove Digits
```bash
sed 's/[0-9]//g' data.txt
```

---

## 9 Advanced Transformations

### Change Delimiter
```bash
sed 's|/usr/bin|/bin|g' file.txt
```
👉 Useful when `/` is inside the pattern.

### Multiple Commands
```bash
sed -e 's/Ali/Ahmet/' -e 's/25/26/' data.txt
```

### Using Script File
Create a file `script.sed`:
```sed
s/Ali/Ahmet/
s/25/26/
```
Run it:
```bash
sed -f script.sed data.txt
```

---

## 10 Real-world Examples

### 1. Remove Comments from Config
```bash
sed '/^#/d;/^$/d' /etc/ssh/sshd_config
```
👉 Removes comment and empty lines.

### 2. Replace in Config File
```bash
sed -i 's/^Port 22/Port 2222/' /etc/ssh/sshd_config
```

### 3. Batch Rename Inside Files
```bash
sed -i 's/oldcompany/newcompany/g' *.html
```

### 4. Extract IP Addresses
```bash
sed -n 's/.*\([0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\).*/\1/p' access.log
```

---

## Exercises

1. Replace `Veli` with `Can` in `data.txt`.  
2. Delete the last line in `data.txt`.  
3. Insert `Zeynep Gunes 28` at the top of `data.txt`.  
4. Remove all numbers from `data.txt`.  
5. In `/etc/passwd`, print only lines with `/bin/bash`.  

---

## 🚀 Conclusion
- `sed` is powerful for text editing in pipelines.  
- Use `sed -i` carefully (it modifies files in place).  
- Combine with `grep`, `awk`, and regex for advanced text processing.  

---
