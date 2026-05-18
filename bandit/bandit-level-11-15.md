# OverTheWire Bandit Levels 11–15

## Objective
Continue developing Linux, networking, and cybersecurity investigation skills through the OverTheWire Bandit wargame.

---

# Level 11

## Goal
Decode the contents of `data.txt`, which had been encrypted using ROT13.

## Commands Used
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

## What I Learned
- Using the `tr` command for character substitution
- Understanding ROT13 encoding
- Applying text transformations through Linux pipelines
- Experimenting with Linux commands and adapting examples from documentation

## Challenges
Initially, I experimented with different methods of using `tr` and command substitution before successfully decoding the file contents using a pipe.

## Key Linux Concepts

### `tr`
Used to translate or substitute characters in text streams.

### ROT13
A simple letter substitution cipher that rotates alphabetical characters by 13 positions.

## Cybersecurity Relevance
ROT13 and character substitution techniques are commonly encountered in:
- CTF challenges
- malware obfuscation
- encoded payloads
- basic text transformations

Understanding text encoding and transformation is useful for cybersecurity investigations and scripting.

---

# Level 12

## Goal
Recover the password hidden inside `data.txt`, which contained a hexdump of a repeatedly compressed file.

## Commands Used
```bash
xxd -r data.txt > data
file data
mv data data.gz
gunzip data.gz
tar -xf data
bunzip2 data.bz2
```

## What I Learned
- Reversing hexdumps back into binary files using `xxd`
- Identifying unknown file types using the `file` command
- Working with multiple compression and archive formats
- Extracting archives using `tar`
- Decompressing files using:
  - `gunzip`
  - `bunzip2`

## Challenges
This level required repeatedly:
1. inspecting the current file,
2. identifying its format,
3. renaming it correctly,
4. extracting or decompressing it,
5. then inspecting the newly created file again.

The process involved handling several different file formats before eventually reaching the final readable password.

## Key Linux Concepts

### `xxd -r`
Reverses a hexdump back into binary format.

### `file`
Identifies the true type of a file regardless of its extension.

### Compression Formats
This challenge involved working with:
- gzip
- bzip2
- tar archives

## Cybersecurity Relevance
These techniques are highly relevant to:
- malware analysis
- digital forensics
- incident response
- binary inspection
- archive analysis

Security analysts often need to inspect unknown files, recover compressed payloads, and analyze suspicious archives.

---

# Level 13

## Goal
Use the provided SSH private key to authenticate as `bandit14`.

## Commands Used
```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

## What I Learned
- Using `scp` (Secure Copy Protocol) to transfer files securely over SSH
- Authenticating with SSH private keys instead of passwords
- Understanding local vs remote connections
- Using SSH identity files with the `-i` flag

## Challenges
Initially, I attempted to transfer the SSH key directly from the Bandit server to my Windows machine using a push-style `scp` command.

However, this approach failed because:
- my Windows machine was not running an SSH server,
- inbound SSH connections were not enabled,
- the Bandit server blocks localhost/self-connections.

Instead, I used a pull-based approach from Windows PowerShell to securely download the private key from the Bandit server.

## Key Linux Concepts

### `scp`
Transfers files securely over SSH.

### `ssh -i`
Uses a specific private key file for SSH authentication.

### SSH Key Authentication
SSH can authenticate users using private/public key pairs instead of passwords.

## Cybersecurity Relevance
SSH key authentication is heavily used in:
- Linux administration
- cloud environments
- DevOps
- penetration testing
- infrastructure security

---

# Level 14

## Goal
Retrieve the password for `bandit15` by sending the current password to a service listening on `localhost` port `30000`.

## Commands Used
```bash
cat /etc/bandit_pass/bandit14
```

```bash
nc localhost 30000
```

## What I Learned
- Using `netcat` (`nc`) to establish raw TCP connections
- Understanding how services listen on specific ports
- Sending manual input to a TCP service
- Basic client/server communication concepts

## Challenges
Initially, I explored whether SSH could be used to connect to the service. However, the challenge required interacting with a raw TCP service rather than an SSH server.

Using:

```bash
nc localhost 30000
```

opened a direct TCP connection to the listening service. After sending the current password, the service returned the password for the next level.

## Key Linux Concepts

### `nc` (Netcat)
A networking utility used to:
- open TCP/UDP connections
- inspect services
- test ports
- send raw data

### `localhost`
Refers to the current machine itself.

### Ports
A port identifies a specific service running on a system.

## Cybersecurity Relevance
Netcat is widely used in:
- penetration testing
- network troubleshooting
- banner grabbing
- service testing
- debugging network applications

---

# Level 15

## Goal
Retrieve the password for `bandit16` by sending the current password to a service running on `localhost` port `30001` using SSL/TLS encryption.

## Commands Used
```bash
openssl s_client -connect localhost:30001
```

## What I Learned
- Using `openssl s_client` to establish encrypted SSL/TLS connections
- Understanding the difference between plain TCP and encrypted communication
- Interacting with secure services over specific ports
- Basic SSL/TLS client-server communication

## Challenges
Unlike the previous level, the service on port `30001` required SSL/TLS encryption.

Using `nc` alone was insufficient because Netcat does not handle encrypted SSL/TLS handshakes.

Instead, I used:

```bash
openssl s_client -connect localhost:30001
```

to establish an encrypted client connection before sending the current password.

The service then returned the password for the next level.

## Key Linux Concepts

### `openssl s_client`
A tool used to:
- connect to SSL/TLS services
- inspect certificates
- debug encrypted connections
- test secure services

### SSL/TLS
Protocols used to encrypt network communications securely.

### Encrypted vs Plain TCP
- `nc` creates plain TCP connections
- `openssl s_client` supports encrypted SSL/TLS communication

## Cybersecurity Relevance
SSL/TLS knowledge is critical in:
- web security
- penetration testing
- secure communications
- certificate inspection
- network troubleshooting

Security professionals frequently use `openssl s_client` to investigate encrypted services and verify TLS configurations.
