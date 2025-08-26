# Linux File Compression Lab Guide

This guide will help you learn and practice **file compression and decompression** in Linux.

---

## 1. Compressing Files with `gzip`

- **Compress a single file**:

```bash
gzip filename.txt
```

This will replace `filename.txt` with `filename.txt.gz`.

- **Decompress**:

```bash
gunzip filename.txt.gz
```

---

## 2. Using `bzip2`

- **Compress**:

```bash
bzip2 filename.txt
```

This creates `filename.txt.bz2`.

- **Decompress**:

```bash
bunzip2 filename.txt.bz2
```

---

## 3. Using `xz`

- **Compress**:

```bash
xz filename.txt
```

This creates `filename.txt.xz`.

- **Decompress**:

```bash
unxz filename.txt.xz
```

---

## 4. Creating Archives with `tar`

- **Create a tar archive**:

```bash
tar -cvf archive.tar file1 file2 file3
```

- **Extract a tar archive**:

```bash
tar -xvf archive.tar
```

- **Create a tar.gz (compressed archive)**:

```bash
tar -czvf archive.tar.gz file1 file2 file3
```

- **Extract tar.gz**:

```bash
tar -xzvf archive.tar.gz
```

- **Create a tar.bz2 archive**:

```bash
tar -cjvf archive.tar.bz2 file1 file2 file3
```

- **Extract tar.bz2**:

```bash
tar -xjvf archive.tar.bz2
```

- **Create a tar.xz archive**:

```bash
tar -cJvf archive.tar.xz file1 file2 file3
```

- **Extract tar.xz**:

```bash
tar -xJvf archive.tar.xz
```

---

## 5. Comparing Compression Methods

| Tool   | Extension   | Speed      | Compression Ratio |
|--------|------------|------------|-------------------|
| gzip   | .gz        | Fast       | Medium            |
| bzip2  | .bz2       | Slower     | Better than gzip  |
| xz     | .xz        | Slowest    | Best compression  |

---

## 6. Practical Example

1. Create some files:

```bash
echo "This is file one" > file1.txt
echo "This is file two" > file2.txt
echo "This is file three" > file3.txt
```

2. Create a tar.gz archive:

```bash
tar -czvf myfiles.tar.gz file1.txt file2.txt file3.txt
```

3. Extract it:

```bash
tar -xzvf myfiles.tar.gz
```

---

✅ You now know how to use **gzip, bzip2, xz, and tar** for file compression in Linux.
