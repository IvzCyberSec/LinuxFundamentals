# Bandit 27

## Goal

Clone a remote Git repository and retrieve the password for the next level.

## Commands Used

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

## What I Learned

* Cloning repositories over SSH
* Git URL syntax
* Remote repository authentication

## Challenges

This level required me to clone a repository hosted on the OverTheWire server using Git. Initially, I attempted to use SSH-style syntax for specifying the port, but I learned that Git requires the port to be included directly inside the SSH URL.

Using:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

I successfully cloned the repository and found the password for the next level.

## Cybersecurity Relevance

This level introduced:

* Git over SSH
* Repository authentication
* Remote source-code retrieval
* Secure development workflows

---

# Bandit 28

## Goal

Retrieve the password for the next level by investigating Git commit history.

## Commands Used

```bash
git log
```

```bash
git show
```

## What I Learned

* Git history analysis
* Recovering deleted information
* Understanding commit history

## Challenges

This level required cloning another repository and investigating its commit history. The current version of the README contained a redacted password, but previous commits still contained the original value.

Using:

```bash
git show
```

I inspected previous commits and recovered the hidden password from the repository history.

## Cybersecurity Relevance

This level demonstrated:

* Git forensics
* Secret leakage through version control
* Source-code auditing
* Historical data recovery

---

# Bandit 29

## Goal

Retrieve the password for the next level by investigating Git branches.

## Commands Used

```bash
git branch -a
```

```bash
git checkout dev
```

```bash
git show
```

## What I Learned

* Git branch enumeration
* Switching between branches
* Investigating development environments

## Challenges

The main branch contained no useful password information, so I enumerated all available branches using:

```bash
git branch -a
```

After discovering a development branch, I switched to it using:

```bash
git checkout dev
```

Inspecting the commit history revealed credentials that had been left inside the development environment.

## Cybersecurity Relevance

This level demonstrated:

* Development environment exposure
* Source-code auditing
* Git branch analysis
* Secret management failures

---

# Bandit 30

## Goal

Retrieve the password for the next level by investigating Git tags.

## Commands Used

```bash
git show-ref
```

```bash
git show secret
```

## What I Learned

* Git tags
* Git references
* Basic Git object inspection

## Challenges

This level required inspecting repository references rather than commits or branches.

Using:

```bash
git show-ref
```

I discovered a tag named:

```text
secret
```

Inspecting the tag with:

```bash
git show secret
```

revealed the password for the next level.

## Cybersecurity Relevance

This level introduced:

* Git metadata analysis
* Hidden references
* Repository forensics
* Secret discovery techniques

---

# Bandit 31

## Goal

Push a required file to the remote repository.

## Commands Used

```bash
git add -f key.txt
```

```bash
git commit -m "add key"
```

```bash
git push origin master
```

## What I Learned

* Working with .gitignore
* Force-adding ignored files
* Git commits and pushes
* Remote repository interaction

## Challenges

This level required creating a file named `key.txt` containing a specific string and pushing it to the remote repository.

Because the file was ignored by `.gitignore`, I had to force Git to add it using:

```bash
git add -f key.txt
```

After committing and pushing the changes, the server returned the password for the next level.

## Cybersecurity Relevance

This level demonstrated:

* Git repository management
* Source control workflows
* File tracking controls
* Remote repository interaction

---

# Bandit 32

## Goal

Escape the restricted uppercase shell and retrieve the password for the final level.

## Commands Used

```bash
$0
```

```bash
cd /etc/bandit_pass
```

```bash
cat bandit33
```

## What I Learned

* Restricted shell bypass techniques
* Shell variable expansion
* Linux shell behavior
* Environment-based escapes

## Challenges

This level presented a custom shell that automatically converted commands to uppercase, preventing normal Linux commands from working.

After experimenting with shell variables, I discovered that:

```bash
$0
```

spawned the current shell executable before the uppercase transformation occurred.

This provided access to a normal shell session, allowing me to navigate to:

```text
/ etc / bandit_pass
```

and retrieve the final password.

## Cybersecurity Relevance

This level demonstrated:

* Restricted shell bypassing
* Environment variable abuse
* Command sanitization weaknesses
* Shell escape techniques

These concepts are directly applicable to:

* Linux privilege escalation
* Restricted environments
* Security research
* Penetration testing
