# Leviathan Notes

Completed in June 2026.

Leviathan felt very different from Bandit. Instead of focusing on Linux commands and enumeration, it introduced me to binary analysis, tracing program execution, debugging, SetUID binaries, and common privilege escalation mistakes.

---

## leviathan0

The first level was simple enumeration.

I listed all files, including hidden ones, and found a `.backup` directory. Inside it was a `bookmarks.html` file.

Searching through the file revealed a comment containing the credentials for the next level.

Commands used:

```bash
ls -alh
cd .backup
grep leviathan bookmarks.html
```

Key lesson: always inspect hidden files, backups, and old exports.

---

## leviathan1

This level introduced `ltrace`.

Running the binary under `ltrace` showed several `strcmp()` calls. At first I focused on the wrong comparison, but eventually realised the important one was the comparison involving my own input.

Commands used:

```bash
ltrace ./check
```

The key idea was following my input through the program instead of assuming every hardcoded string was important.

Key lesson: identify where user input is used and ignore unrelated comparisons.

---

## leviathan2

This level involved a SetUID binary called `printfile`.

Using `ltrace` revealed that the program used `system()` internally to build and execute a command.

Commands used:

```bash
ltrace ./printfile
```

The program did not properly handle filenames containing spaces, which allowed command injection through a crafted filename.

I initially tried reusing techniques from previous levels, but the important clue was understanding how the shell interprets arguments.

Key lesson: whenever a privileged binary uses `system()`, inspect exactly how user input is passed into shell commands.

---

## leviathan3

This level continued using `ltrace`.

Again, there were multiple `strcmp()` calls, and the first comparison turned out to be a decoy.

Commands used:

```bash
ltrace ./level3
```

The important comparison was the one that changed depending on what I typed.

Instead of searching for suspicious strings, I focused on the question:

> Where does my input go?

Key lesson: track user-controlled data through the program.

---

## leviathan4

The binary output a sequence of binary values.

I recognised the output format immediately and decoded it manually to obtain the next password.

After decoding it, I used `ltrace` to confirm my understanding of how the program worked.

Commands used:

```bash
ltrace ./bin
```

The trace showed that the binary simply opened the next password file and printed its contents in binary format.

Key lesson: don't just decode output—understand how the output is generated.

---

## leviathan5

This level focused on insecure temporary file handling.

Using `ltrace` showed that the binary opened a predictable file in `/tmp`:

```c
fopen("/tmp/file.log", "r");
```

The program trusted whatever `/tmp/file.log` pointed to.

I created a symbolic link from `/tmp/file.log` to the next password file. Since the binary was running with elevated privileges, it followed the symlink and printed the contents.

Commands used:

```bash
ltrace ./leviathan5
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
```

Key lesson: privileged programs should never trust predictable files inside `/tmp`.

This was my first practical encounter with symlink attacks and insecure temporary file handling.

---

## leviathan6

This level required finding a four-digit PIN.

Using `strings` revealed a reference to `/bin/sh`, which suggested that entering the correct PIN would spawn a shell.

Commands used:

```bash
strings leviathan6
ltrace ./leviathan6 1234
```

`ltrace` showed that the program called `atoi()` on my input:

```c
atoi(argv[1]);
```

At this point I switched to `gdb` to understand the program internally instead of brute-forcing it immediately.

Commands used:

```bash
gdb ./leviathan6
break atoi
run 1234
finish
info registers
x/20i $eip
x/wd $ebp-0xc
```

After returning from `atoi()`, my input was stored in the `EAX` register.

I found the following comparison:

```asm
cmp %eax,-0xc(%ebp)
jne ...
```

This meant:

* my input was stored in `EAX`
* the correct PIN was stored at `EBP-0xc`

Inspecting the value on the stack revealed the correct PIN.

Key lesson: learn where user input is stored, how it moves through the program, and what controls execution flow.

This level was my introduction to reading assembly instructions and inspecting stack variables with GDB.

---

## leviathan7

The final level simply confirmed completion of the wargame.

After logging in, I found a file called:

```text
CONGRATULATIONS
```

Reading it confirmed that Leviathan was complete.

Commands used:

```bash
ls -alh
file CONGRATULATIONS
cat CONGRATULATIONS
```

---

## What Leviathan Taught Me

* Using `ltrace` to observe library calls
* Following user input through a program
* Understanding `strcmp()`
* Understanding `atoi()`
* Working with SetUID binaries
* Identifying command injection vulnerabilities
* Understanding symlink attacks
* Using `strings` effectively
* Basic reverse engineering with `gdb`
* Reading registers and stack variables
* Following execution flow in assembly

The biggest lesson from Leviathan was changing my mindset from:

> "What command solves this?"

to:

> "What is the program actually doing?"
