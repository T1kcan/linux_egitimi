# 🧑‍💻 Regular Expressions (Regex) Complete Hands-on Lab Guide

This lab will take you from **basic to advanced regex usage** step by step.  
You can run all examples using `grep`, `egrep`, or within programming languages like Python/Perl.  

---

## Objectives
- Understand regex basics
- Learn character classes, anchors, quantifiers
- Perform grouping and backreferences
- Use regex in real-world Linux commands
- Practice advanced patterns

---

## 1️ Introduction to Regex

A **regular expression (regex)** is a pattern used to match text.  

Basic tools in Linux:
```bash
grep 'pattern' file
egrep 'pattern' file    # or grep -E
```

---

## 2️ Create a Test File

```bash
cat > data.txt <<EOF
Ali Veli 25
Ayse Yilmaz 30
Mehmet Kaya 40
Fatma Demir 35
user1@test.com
user.name@company.org
123-456-7890
+90-555-123-4567
http://example.com
https://secure.example.org
EOF
```

---

## 3️ Basic Matching

```bash
grep 'Ali' data.txt
```
👉 Matches lines containing `Ali`.

```bash
grep '^A' data.txt
```
👉 Matches lines starting with `A`.

```bash
grep '0$' data.txt
```
👉 Matches lines ending with `0`.

---

## 4️ Character Classes

```bash
grep '[0-9]' data.txt
```
👉 Matches any digit.

```bash
grep '^[A-Z]' data.txt
```
👉 Matches lines starting with uppercase.

```bash
grep '[aeiou]' data.txt
```
👉 Matches lines containing vowels.

---

## 5️ Predefined Classes (POSIX)

- `[[:digit:]]` → digits
- `[[:alpha:]]` → letters
- `[[:alnum:]]` → letters + digits
- `[[:space:]]` → whitespace

Example:
```bash
grep '[[:digit:]]' data.txt
```

---

## 6️ Quantifiers

```bash
grep -E '[0-9]{2}' data.txt
```
👉 Matches numbers with exactly 2 digits.

```bash
grep -E '[0-9]{2,}' data.txt
```
👉 Matches numbers with 2 or more digits.

```bash
grep -E '[0-9]{2,3}' data.txt
```
👉 Matches numbers with 2 to 3 digits.

---

## 7️ Wildcard and Repetition

```bash
grep '.*@.*' data.txt
```
👉 Matches email-like strings.

```bash
grep -E 'a+' data.txt
```
👉 Matches lines with one or more `a`.

```bash
grep -E 'a?' data.txt
```
👉 Matches lines with zero or one `a`.

---

## 8️ Grouping and Alternation

```bash
grep -E '(Ali|Ayse)' data.txt
```
👉 Matches either `Ali` or `Ayse`.

```bash
grep -E '(com|org)$' data.txt
```
👉 Matches domains ending in `.com` or `.org`.

---

## 9️ Email Pattern Example

```bash
grep -E '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$' data.txt
```
👉 Matches email addresses.

---

## 10 Phone Number Example

```bash
grep -E '^[0-9]{3}-[0-9]{3}-[0-9]{4}$' data.txt
```
👉 Matches U.S. style phone numbers.

```bash
grep -E '^\+?[0-9-]+$' data.txt
```
👉 Matches international phone numbers.

---

## 11 URL Example

```bash
grep -E 'https?://[A-Za-z0-9./]+' data.txt
```
👉 Matches `http://` and `https://` URLs.

---

## 12 Backreferences

Example file:
```bash
echo -e "foo foo
bar baz
hello hello" > repeat.txt
```

```bash
grep -E '([a-z]+) ' repeat.txt
```
👉 Matches repeated words.

---

## 13 Negative Matching

```bash
grep -v 'Ali' data.txt
```
👉 Prints all lines not containing `Ali`.

---

## 14 Advanced Examples

### Extract IP Addresses
```bash
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log
```

### Extract Dates (YYYY-MM-DD)
```bash
grep -E '[0-9]{4}-[0-9]{2}-[0-9]{2}' logfile.txt
```

### Validate Strong Password (at least one digit, one uppercase, 8+ chars)
```bash
grep -E '^(?=.*[0-9])(?=.*[A-Z]).{8,}$' passwords.txt
```

---

## Exercises

1. Match all lines starting with `Mehmet`.  
2. Match emails from `company.org`.  
3. Extract only `.org` domains.  
4. Find lines containing a number greater than 30.  
5. Validate Turkish phone numbers like `+90-555-123-4567`.  

---

## 🚀 Conclusion
- Regex is universal across tools like `grep`, `sed`, `awk`, Python, Java, etc.  
- Mastering regex saves time in text parsing and automation.  
- Always test patterns carefully to avoid false matches.  

---
