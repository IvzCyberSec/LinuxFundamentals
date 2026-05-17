# OverTheWire Bandit Levels 5–10

## Objective
Continue developing Linux command-line and cybersecurity investigation skills through the OverTheWire Bandit.

---

# Level 5

## Goal
Find the password file hidden within a large directory structure.

The target file was:
- human-readable
- exactly 1033 bytes in size

## Commands Used
```bash
find . -type f -size 1033c
```

## What I Learned
- Recursive file searching in Linux
- Filtering files by size using the `find` command
- Efficient file enumeration within nested directories

## Challenges
The `inhere` directory contained many similarly named subdirectories and files, making manual searching inefficient.

Using `find` allowed the correct file to be identified quickly.

## Cybersecurity Relevance
Recursive searching and file enumeration are important skills in:
- digital forensics
- SOC investigations
- Linux administration
- penetration testing

---

# Level 6

## Goal
Locate a file somewhere on the filesystem with the following properties:
- owned by user `bandit7`
- owned by group `bandit6`
- exactly 33 bytes in size

## Commands Used
```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

## Additional Command
```bash
cat $(find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null)
```

## What I Learned
- Searching the entire Linux filesystem recursively
- Filtering files by owner, group, and size
- Suppressing noisy terminal output using:
  
```bash
2>/dev/null
```

- Using command substitution with:
  
```bash
$()
```

## Challenges
Running the `find` command generated many `Permission denied` messages because the `bandit6` user lacked access to large parts of the filesystem.

Redirecting stderr into `/dev/null` removed unnecessary noise and displayed only useful results.

## Cybersecurity Relevance
These techniques are highly relevant to:
- Linux enumeration
- incident response
- digital forensics
- SOC investigations
- penetration testing

---

# Level 7

## Goal
Find the password stored in `data.txt` next to the word `millionth`.

## Commands Used
```bash
grep millionth data.txt
```

## What I Learned
- Using `grep` to search for specific text patterns
- Efficiently locating information within large datasets
- Basic text filtering in Linux

## Challenges
The file contained a large amount of text, making manual searching inefficient.

Using `grep` allowed the correct line to be located instantly.

## Cybersecurity Relevance
`grep` is widely used in:
- log analysis
- threat hunting
- searching system files
- investigating suspicious activity

---

# Level 8

## Goal
Find the only line in `data.txt` that appears exactly once.

## Commands Used
```bash
sort data.txt | uniq -u
```

## What I Learned
- Using Linux pipes (`|`) to chain commands together
- Sorting data before removing duplicates
- Using `uniq -u` to display unique lines only

## Challenges
Initially, using `uniq` alone did not work because duplicate lines were not adjacent.

The `sort` command was required first so identical entries were grouped together before processing with `uniq`.

## Key Linux Concepts
### Pipe Operator
```bash
|
```

The pipe operator sends the output of one command directly into another command.

### Why `sort` Was Necessary
The `uniq` command only detects adjacent duplicate lines, so sorting the file first ensures duplicate entries are grouped together.

## Cybersecurity Relevance
These techniques are important in:
- log analysis
- data filtering
- threat hunting
- SOC investigations

---

# Level 9

## Goal
Find the password hidden inside `data.txt`, which contained a large amount of non-human-readable data.

The challenge hinted that the password was stored within human-readable strings preceded by several `=` characters.

## Commands Used
```bash
strings data.txt | grep "="
```

## What I Learned
- Using the `strings` command to extract readable text from binary or non-readable files
- Filtering command output using `grep`
- Combining commands using Linux pipelines

## Challenges
The contents of `data.txt` were mostly unreadable.

Using:

```bash
strings data.txt
```

allowed readable ASCII text to be extracted before filtering the results using `grep`.

## Cybersecurity Relevance
These techniques are commonly used in:
- malware analysis
- digital forensics
- reverse engineering
- binary inspection

Security analysts frequently extract readable strings from suspicious files to identify:
- URLs
- credentials
- commands
- indicators of compromise

---

# Level 10

## Goal
Decode the Base64-encoded password stored inside `data.txt`.

## Commands Used
```bash
base64 -d data.txt
```

## What I Learned
- Understanding the difference between encoding and encryption
- Decoding Base64 data using Linux tools
- Reading Linux documentation using `man`

## Challenges
Initially, running:

```bash
base64 data.txt
```

encoded the already encoded data again.

After reading the manual page for `base64`, I learned to use the decode flag:

```bash
-d
```

to properly decode the file contents.

## Cybersecurity Relevance
Base64 encoding appears frequently in:
- malware payloads
- phishing attacks
- JWT tokens
- APIs
- PowerShell attacks
- HTTP authentication

Cybersecurity analysts regularly decode Base64 data during investigations and threat analysis.
