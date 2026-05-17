# OverTheWire Bandit Levels 0–4

## Objective
Practice Linux fundamentals and command line skills through hands on exercises using the OverTheWire Bandit.

---

# Level 0

## Goal
Connect to the Bandit server using SSH.

## Commands Used
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

## What I Learned
- How SSH remote connections work
- Connecting to a remote Linux machine
- Using a specific port with SSH
- Basic terminal interaction

---

# Level 1

## Goal
Retrieve the password stored inside the `readme` file.

## Commands Used
```bash
ls
cat readme
```

## What I Learned
- Listing files using `ls`
- Reading file contents using `cat`
- Basic Linux file interaction

---

# Level 2

## Goal
Open a file named `-`.

## Commands Used
```bash
cat ./-
```

## What I Learned
- Handling special characters in filenames
- Understanding file paths
- Why Linux sometimes requires explicit paths

---

# Level 3

## Goal
Find a hidden file inside the `inhere` directory.

## Commands Used
```bash
cd inhere
find
cat ...Hiding-From-You
```

## What I Learned
- Hidden files in Linux
- File enumeration techniques
- Navigating directories using the command line

---

# Level 4

## Goal
Identify the only human-readable file in a directory containing multiple files.

## Commands Used
```bash
cd inhere
file ./*
cat ./-file07
```

## What I Learned
- Using the `file` command to identify file types
- Difference between binary and ASCII text files
- Basic Linux investigation techniques

## Cybersecurity Relevance
Skills practiced in these levels are useful for:
- Linux system administration
- Security Operations Centre (SOC) work
- Digital forensics
- Malware analysis
- Linux enumeration and investigation

---

# Commands Learned So Far

```bash
ssh
ls
cd
cat
find
file
```

# Key Linux Concepts Learned
- Remote access using SSH
- Linux file navigation
- Hidden files
- Relative paths
- File type identification
- Basic Linux investigation workflow
