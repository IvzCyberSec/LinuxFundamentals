# OverTheWire Bandit Levels 16–20

## Objective
Continue developing Linux, networking, and cybersecurity investigation skills through the OverTheWire Bandit wargame.

---

# Level 16

## Goal
Identify the correct SSL/TLS-enabled service between ports `31000-32000` and retrieve the credentials for the next level.

## Commands Used
```bash
nmap -sV localhost -p31000-32000
```

```bash
echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31790 -quiet -tls1_2
```

## What I Learned
- Using `nmap` for service enumeration
- Identifying SSL/TLS-enabled services
- Interacting with encrypted network services
- Troubleshooting TLS connection behavior
- Retrieving credentials from network services

## Challenges
The challenge required identifying which service between ports `31000-32000` was running SSL/TLS.

Using:

```bash
nmap -sV localhost -p31000-32000
```

allowed me to detect which ports were running encrypted services.

Initially, both SSL-enabled ports returned unexpected `KEYUPDATE` messages during communication. After troubleshooting, I determined this behavior was related to TLS negotiation and solved the issue by forcing a TLS 1.2 connection using OpenSSL.

After sending the current password to the correct service on port `31790`, the server returned an SSH private key for the next level.

## Key Linux & Networking Concepts

### `nmap -sV`
Performs service and version detection on open ports.

### `openssl s_client`
Used to establish encrypted SSL/TLS client connections.

### TLS/SSL Enumeration
Identifying which services use encrypted communication protocols.

### TLS Version Negotiation
Different TLS versions may behave differently during encrypted communication.

## Cybersecurity Relevance
These techniques are highly relevant to:
- penetration testing
- service enumeration
- TLS investigation
- encrypted service analysis
- network reconnaissance

Security professionals frequently:
- scan for services,
- identify encrypted ports,
- investigate TLS behavior,
- manually interact with network services,
- retrieve and analyze credentials securely.

---

# Level 17

## Goal
Find the password for `bandit18` by comparing the files `passwords.old` and `passwords.new`.

## Commands Used
```bash
diff passwords.old passwords.new
```

## What I Learned
- Using `diff` to compare files
- Identifying changed lines between datasets
- Understanding how Linux tools can detect differences efficiently

## Challenges
The challenge required identifying the single line that differed between:
- `passwords.old`
- `passwords.new`

Using:

```bash
diff passwords.old passwords.new
```

allowed me to quickly locate the changed line, which contained the password for the next level.

## Key Linux Concepts

### `diff`
Compares files line-by-line and displays differences between them.

## Cybersecurity Relevance
File comparison is commonly used in:
- incident response
- log analysis
- configuration auditing
- malware analysis
- forensic investigations

Security analysts frequently compare:
- logs,
- configurations,
- file versions,
- system outputs,
to identify suspicious changes.

---

# Level 18

## Goal
Retrieve the password stored inside the `readme` file despite being immediately logged out after login.

## Commands Used
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

## What I Learned
- Executing remote commands over SSH
- Understanding restricted shell environments
- Bypassing interactive shell limitations
- Using SSH non-interactively

## Challenges
Attempting to log into the `bandit18` shell normally caused an immediate logout due to the `.bashrc` configuration.

Since interactive shell access was restricted, I instead executed a remote command directly through SSH:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

This allowed the contents of the `readme` file to be displayed before the logout process could terminate the session.

## Key Linux Concepts

### Remote Command Execution
SSH can execute commands directly on remote systems without opening an interactive shell.

### Restricted Shell Environments
Some systems intentionally restrict user interaction through shell configuration files such as `.bashrc`.

## Cybersecurity Relevance
These concepts are important in:
- penetration testing
- Linux administration
- automation
- restricted shell bypassing
- remote system management

Security professionals often interact with restricted environments and use non-interactive methods to retrieve information or execute commands remotely.

---

# Level 19

## Goal
Retrieve the password for `bandit20` using the SUID binary provided in the home directory.

## Commands Used
```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

## What I Learned
- Understanding SUID (Set User ID) binaries
- Executing commands with elevated permissions
- Basic Linux privilege escalation concepts
- Access control and permission delegation in Linux

## Challenges
The challenge provided a SUID binary called:

```text
bandit20-do
```

This binary executed commands with the permissions of its owner rather than the current user.

Normally, the file:

```text
/etc/bandit_pass/bandit20
```

could not be accessed directly by the current user.

However, by using the SUID binary to execute:

```bash
cat /etc/bandit_pass/bandit20
```

I was able to read the password with elevated permissions.

## Key Linux Concepts

### SUID (Set User ID)
A special Linux permission that allows executables to run with the permissions of the file owner.

### Privilege Escalation
Using elevated permissions to access resources or execute commands that would normally be restricted.

## Cybersecurity Relevance
SUID binaries are highly important in:
- Linux security
- privilege escalation
- penetration testing
- system hardening

Security professionals frequently audit systems for misconfigured SUID binaries because vulnerable or improperly configured SUID executables can lead to:
- unauthorized access,
- privilege escalation,
- full system compromise.

---

# Level 20

## Goal
Retrieve the password for `bandit21` by setting up a listening server and communicating with the `suconnect` program.

## Commands Used
```bash
nc -l 12345
```

```bash
./suconnect 12345
```

## What I Learned
- Setting up a listening TCP server using Netcat
- Understanding client/server networking roles
- Establishing local socket connections
- Sending and receiving data between processes

## Challenges
Unlike previous levels where I connected to existing services, this challenge required me to become the listening server myself.

Using:

```bash
nc -l 12345
```

I created a Netcat listener on port `12345`.

In a second terminal, I executed:

```bash
./suconnect 12345
```

which connected back to my Netcat listener.

After sending the current level password through the listener, the client returned the password for the next level.

## Key Linux & Networking Concepts

### `nc -l`
Starts Netcat in listening mode, allowing it to act as a TCP server.

### Client/Server Communication
This level demonstrated how:
- one process listens for connections,
- another process connects as a client.

### Local Socket Communication
Processes on the same machine can communicate over TCP sockets using localhost and ports.

## Cybersecurity Relevance
These concepts are highly relevant to:
- penetration testing
- reverse shells
- bind shells
- network services
- command-and-control (C2) communication
- socket programming

Security professionals frequently work with:
- listeners,
- inbound connections,
- callback behavior,
- local TCP communication,
during offensive security operations and network investigations.
