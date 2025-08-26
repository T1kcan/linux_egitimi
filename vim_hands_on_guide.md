# Vi/Vim Hands-On Guide

## Introduction
`vi` is a powerful text editor available on almost all Unix/Linux systems. `vim` (Vi IMproved) is an enhanced version of `vi` with more features. This guide will teach you basic and advanced commands with hands-on exercises, including challenge labs for practice.

---

## Table of Contents
1. Starting Vi/Vim
2. Modes in Vi
3. Basic Navigation
4. Editing Text
5. Saving and Exiting
6. Searching and Replacing
7. Advanced Editing
8. Practice Exercises
9. Challenge Lab

---

## 1. Starting Vi/Vim

Open a terminal and type:

```bash
vi filename.txt
```

- If `filename.txt` exists, it opens the file.
- If not, it creates a new file.

---

## 2. Modes in Vi

Vi has **three main modes**:

1. **Normal Mode**: Default mode. Used for navigation and commands.
2. **Insert Mode**: Used to insert text. Press `i` to enter.
3. **Command Mode**: Entered by pressing `:` in Normal mode. Used for saving, exiting, and more.

> **Tip:** Press `Esc` anytime to return to Normal mode.

---

## 3. Basic Navigation

| Command | Description |
|---------|-------------|
| h       | Move left |
| j       | Move down |
| k       | Move up |
| l       | Move right |
| w       | Jump to next word |
| b       | Jump to beginning of word |
| 0       | Go to beginning of line |
| $       | Go to end of line |

---

## 4. Editing Text

### Insert Text
- `i` → Insert before cursor
- `I` → Insert at beginning of line
- `a` → Append after cursor
- `A` → Append at end of line
- `o` → Open a new line below
- `O` → Open a new line above

### Delete Text
- `x` → Delete character under cursor
- `dd` → Delete current line
- `d$` → Delete to end of line
- `dw` → Delete word

### Copy/Paste
- `yy` → Yank (copy) current line
- `p` → Paste after cursor
- `P` → Paste before cursor

---

## 5. Saving and Exiting

- `:w` → Save
- `:q` → Quit
- `:wq` or `ZZ` → Save and quit
- `:q!` → Quit without saving

---

## 6. Searching and Replacing

- `/word` → Search forward for "word"
- `?word` → Search backward for "word"
- `n` → Next match
- `N` → Previous match

Replace examples:

```vim
:%s/old/new/g  " Replace all occurrences in file
:1,10s/old/new/g  " Replace in lines 1 to 10
```

---

## 7. Advanced Editing

- `u` → Undo
- `Ctrl + r` → Redo
- `.` → Repeat last command
- `>>` → Indent line
- `<<` → Un-indent line
- Visual mode: `v` → Select text, then `d` to delete or `y` to yank

---

## 8. Practice Exercises

1. Create a file `practice.txt` and insert the following lines:

```
Linux is powerful.
Vim is awesome.
Practice makes perfect.
```

2. Navigate to the second line and change `awesome` to `amazing`.
3. Copy the first line and paste it at the end.
4. Delete the third line.
5. Save and exit the file.

> Verify the file using `cat practice.txt`.

---

## 9. Challenge Lab

Try these tasks to test your Vim skills:

1. Open a new file `challenge.txt` and insert the following:
```
Vim is efficient.
Editing files is fun.
Learn Vim step by step.
Practice daily.
```

2. Move the last line to the top.
3. Replace the word `fun` with `exciting`.
4. Delete the second line using a single command.
5. Copy the first line and paste it after the last line.
6. Undo the last change, then redo it.
7. Save and exit.

> Use `cat challenge.txt` to check your results.

---

## References

- `:help` → Opens Vim help
- [Vim Cheat Sheet](https://vim.rtorr.com/)

---

## Tips

- Use `Esc` to return to normal mode whenever confused.
- Learn 2–3 new commands per day and practice.
- The Challenge Lab helps solidify advanced editing skills.

