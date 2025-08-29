
# Bash Scripting Guide – Annotated Examples

This guide explains every example from the Bash scripting complete guide with detailed commentary.

---

# Part 1: BASIC

## 0) Quick-start checklist
```bash
printf '%s\n' '#!/usr/bin/env bash' 'echo "Hello"' > hello.sh
chmod +x hello.sh
./hello.sh
bash --version
```
**Explanation:**
- Creates a new script `hello.sh` with a shebang and a simple echo.
- `chmod +x` makes it executable.
- `./hello.sh` runs the script.
- `bash --version` checks the Bash version.

## 1) Script structure & execution
```bash
#!/usr/bin/env bash
set -Eeuo pipefail
main() {
  echo "Script name: $0"
}
main "$@"
```
**Explanation:**
- `set -Eeuo pipefail` ensures the script exits on error, unset variables, or failed pipes.
- `main "$@"` passes all command-line arguments to the function safely.
- `$0` prints the script’s name.

## 2) Variables & parameter expansion
```bash
name="Ada"
greet="Hello, $name"
echo "$greet"
echo "${name} Lovelace"
```
**Explanation:**
- `$name` is expanded in double quotes.
- `${name}` avoids ambiguity when concatenating with other strings.
- You can provide default values `${VAR:-default}` or mandatory checks `${VAR:?error}`.

## 3) Quoting & escaping
```bash
today="$(date +%F)"; echo "Today: $today"
```
- `$(...)` executes a command and substitutes output.
- `"..."` preserves spaces.
- `'...'` prevents expansion.

## 4) Globbing (wildcards)
```bash
echo *.txt
echo /var/log/*.{log,txt}
```
- `*` matches any characters.
- `{log,txt}` matches multiple patterns.

## 5) Exit status & tests
```bash
f="notes.txt"
if [[ -f $f && -s $f ]]; then
  echo "File exists and has data"
fi

x=10 y=20
if (( x < y )); then echo "x<y"; fi
```
- `-f` → checks file existence.  
- `-s` → non-empty.  
- `(( ... ))` → arithmetic evaluation, returns 0 if true.

## 6) Conditionals
```bash
if [[ $USER == "root" ]]; then
  echo "Hi root"
else
  echo "Regular user"
fi
```
- `[[ ... ]]` safer than `[ ... ]`, allows pattern matching.

## 7) Loops
```bash
for i in 1 2 3; do echo "$i"; done
count=0; while (( count < 3 )); do echo "$count"; ((count++)); done
num=1; until [ $num -gt 3 ]; do echo "$num"; ((num++)); done
```
- `for` iterates over list.
- `while` runs while condition true.
- `until` runs until condition true.

## 8) Functions
```bash
greet() { local who=${1:-world}; echo "Hello, $who"; }
greet "Ada"
```
- `local` limits variable scope to the function.
- `${1:-world}` provides default if argument missing.

## 10) I/O redirection
```bash
# cmd > out.txt 2> err.log
# cmd | other pipeline
cat <<'EOF' > config.ini
[user]
name = "$USER"
EOF
```
- `>` redirect stdout, `2>` redirect stderr.  
- `|` pipe stdout to another command.  
- `'EOF'` disables variable expansion in here-doc.

## 11) Safe-mode basics
```bash
set -Eeuo pipefail
trap 'echo "Error on line $LINENO"; exit 1' ERR
```
- `trap` executes code on error.  
- `$LINENO` shows the line number of error.

---

# Part 2: INTERMEDIATE

## Conditional Statements
```bash
read num
if [ $num -gt 10 ]; then echo "Number >10"; fi
```
- `[ $num -gt 10 ]` → numeric comparison.

## Case Statements
```bash
case $char in
 [a-z]) echo "Lower";;
 [A-Z]) echo "Upper";;
 [0-9]) echo "Digit";;
 *) echo "Special";;
esac
```
- Matches patterns to execute specific code.

## Arrays
```bash
fruits=("Apple" "Banana")
echo "${fruits[@]}"
```
- `"${fruits[@]}"` expands all elements individually.

## Input / Output Redirection
```bash
ls > file.txt
echo "Append" >> file.txt
ls /nonexistent 2> error.log
ls /etc /nonexistent &> all.log
```
- `>>` append, `&>` redirect both stdout and stderr.

---

# Part 3: ADVANCED

## Functions (advanced usage)
```bash
greet_user() { echo "Hello, $1!"; }
greet_user "Alice"
```
- Functions reusable, `$1` positional argument.

## Associative Arrays
```bash
declare -A capitals; capitals[France]="Paris"
echo "${capitals[France]}"
```
- Keys can be strings. Retrieve value with `${array[key]}`.

## Error Handling
```bash
set -euo pipefail
```
- Stops script on any error, unset variable, or pipeline failure.

## Trap Signals
```bash
trap "echo 'Interrupted'; exit" SIGINT SIGTERM
```
- Handles Ctrl+C or termination signals gracefully.

## Parallel Execution
```bash
./task1.sh & ./task2.sh & wait
```
- `&` runs tasks in background.  
- `wait` waits for all background processes.

## Using getopts for Flags
```bash
while getopts "u:p:" opt; do
  case $opt in
    u) user=$OPTARG;;
    p) pass=$OPTARG;;
  esac
done
echo "Kullanıcı: $user"
echo "Şifre: $pass"
```
- Parses command-line options `-u` and `-p`. `$OPTARG` stores the value.

