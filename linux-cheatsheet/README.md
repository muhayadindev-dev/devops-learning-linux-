# Linux Cheat Sheet

**Section:** Linux Cheat Sheet · [Repository Home](../README.md) · [Bandit Walkthroughs](../bandit/README.md)

My quick reference for Linux command-line problems, built around my completed OverTheWire Bandit Levels 0–15 (16 challenges). The first part gives me reusable command patterns; the last part is a compact answer key for those challenges.

How to read it:

- `<value>` means replace the placeholder with the relevant value.
- `[options]` means an optional argument.
- Examples use practice filenames and hosts; no passwords or private-key contents are included.

## Contents

- [The Method](#the-method)
- [Which Tool for Which Problem](#which-tool-for-which-problem)
- [Moving Around and Reading](#moving-around-and-reading)
- [Files and Folders](#files-and-folders)
- [Searching with find](#searching-with-find)
- [Searching with grep](#searching-with-grep)
- [Pipes and Redirection](#pipes-and-redirection)
- [Decoding and Unpacking](#decoding-and-unpacking)
- [Remote and Network](#remote-and-network)
- [Shortcuts and Getting Unstuck](#shortcuts-and-getting-unstuck)
- [Bandit Answer Key: Levels 0–15](#bandit-answer-key-levels-015)

## The Method

1. Read the problem and note every clue: filename, path, type, size, owner, group, text, port or encryption.
2. Pick the tool that matches the problem. Am I finding a file, searching its contents, identifying its format or connecting to a service?
3. Fill in the pattern with the complete values: starting path, filters, filename, destination or hostname and port.
4. Check the output, then adjust and repeat. For an unfamiliar command: attempt → `man` / `--help` → this cheat sheet.

My rule: Don't stop at "use `find`". Write the whole command, then explain why every flag is there.

## Which Tool for Which Problem

| I want to... | Use |
|---|---|
| See hidden files | `ls -a` or `ls -la` |
| Find a file by name, size, owner, group or executable status | `find` |
| Find text inside a file | `grep` |
| Find the line that appears exactly once | `sort` then `uniq -u` |
| Pull readable text out of binary data | `strings` |
| Check a file's actual type | `file` |
| Decode Base64 | `base64 -d` |
| Swap characters or decode ROT13 | `tr` |
| Reverse a hexdump | `xxd -r` |
| Unpack compressed files and archives | `gzip -d`, `bzip2 -d`, `tar -xf` |
| Log in using an SSH key | `ssh -i` |
| Copy a key or file securely | `scp` |
| Talk to a TCP port | `nc` |
| Talk to a TLS-enabled service | `openssl s_client` |

## Moving Around and Reading

| I want to... | Pattern | Example |
|---|---|---|
| Go into a folder | `cd <folder>` | `cd inhere` |
| Go up one level | `cd ..` | `cd ..` |
| Go home | `cd ~` | `cd ~` |
| See where I am | `pwd` | `pwd` |
| List files | `ls [options] [folder]` | `ls -la inhere` |
| Show hidden files too | `ls -a` | `ls -la` |
| Read a file | `cat <file>` | `cat readme` |
| Read a file named `-` | `cat ./-` | `cat ./-` |
| Read a filename with spaces and leading hyphens | `cat ./'<name>'` | `cat ./'--spaces in this filename--'` |
| Read the first lines | `head -n <number> <file>` | `head -n 5 notes.txt` |
| Read the last lines | `tail -n <number> <file>` | `tail -n 10 notes.txt` |
| Check the file type | `file <file>` | `file ./-file07` |

Paths: `.` is the current directory; `..` is its parent; `~` is home; `/` starts an absolute path; `./name` explicitly refers to a name in the current directory.

Odd-filename trap: `cat -` reads standard input, not the file named `-`. Use `cat ./-` or `cat < ./-`. Quotes keep spaces together; `./` prevents leading hyphens from being parsed as options. `--` is another option terminator for commands that support it.

## Files and Folders

| I want to... | Pattern | Example |
|---|---|---|
| Make a folder | `mkdir <name>` | `mkdir backup` |
| Make nested folders | `mkdir -p <path>` | `mkdir -p labs/linux` |
| Make a private temporary folder | `mktemp -d` | `mktemp -d` |
| Copy a file | `cp <source> <destination>` | `cp data.txt backup/` |
| Rename or move a file | `mv <old> <new>` | `mv data.bin data.gz` |
| Change permissions | `chmod <mode> <file>` | `chmod 600 bandit14.key` |
| Check permissions and ownership | `ls -l <file>` | `ls -l bandit14.key` |
| See my user and groups | `id` | `id` |

Permissions I needed: `chmod 600` gives the owner read/write access and removes permissions for group and others. SSH may reject a private key if other users can access it.

`mktemp -d` trap: It prints a different directory each time. Use the path it actually returns; don't reuse a path from somebody else's terminal.

## Searching with find

Pattern:

```bash
find <where-to-start> [filters] [2>/dev/null]
```

| Where to start | Meaning |
|---|---|
| `.` | The current directory and everything beneath it |
| `/` | The entire filesystem accessible to my account |
| `<path>` | A particular directory tree |

| Filter | Pattern | Example |
|---|---|---|
| By name | `-name '<name>'` | `-name '*.log'` |
| Files only | `-type f` | `-type f` |
| Directories only | `-type d` | `-type d` |
| Exact size | `-size <number><unit>` | `-size 1033c` |
| Bigger than | `-size +<number><unit>` | `-size +5M` |
| Smaller than | `-size -<number><unit>` | `-size -5M` |
| By owner | `-user <user>` | `-user bandit7` |
| By group | `-group <group>` | `-group bandit6` |
| Not executable by my account | `! -executable` | `! -executable` |
| Match a permission mode | `-perm <mode>` | `-perm 644` |

Size units: `c` = bytes, `k` = 1,024-byte units, `M` = 1,048,576-byte units, `G` = 1,073,741,824-byte units. No space between the number and unit. Without `c`, `find` may round sizes to allocation units.

My Level 5 pattern - every condition included:

```bash
find . -type f -size 1033c ! -executable
file ./inhere/maybehere07/.file2
```

My Level 6 pattern - owner, group and exact bytes:

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Verify: `find` tells me where the file is. `file <path>` tells me what kind of file it is. `cat <path>` reads it if it's text.

Trap: `2>/dev/null` suppresses standard-error messages, such as expected permission-denied messages. It does not grant access; leave it off while investigating unexpected failures.

## Searching with grep

| I want to... | Pattern | Example |
|---|---|---|
| Show matching lines | `grep '<pattern>' <file>` | `grep 'millionth' data.txt` |
| Ignore case | `grep -i '<pattern>' <file>` | `grep -i 'permission denied' deployment-errors.log` |
| Show matching line numbers | `grep -n '<pattern>' <file>` | `grep -n 'error' app.log` |
| Show lines that do not match | `grep -v '<pattern>' <file>` | `grep -v 'error' app.log` |
| Count matching lines | `grep -c '<pattern>' <file>` | `grep -c 'error' app.log` |
| Show lines after a match | `grep -A <number> '<pattern>' <file>` | `grep -A 2 'error' app.log` |
| Search within a directory | `grep -r '<pattern>' <folder>` | `grep -r 'error' logs/` |
| Count matching output lines | `grep '<pattern>' <file> \| wc -l` | `grep -i 'permission denied' deployment-errors.log \| wc -l` |

My distinction: `find` locates files; `grep` searches inside them. `grep -c` counts matching lines, not every occurrence of a word within a line. `wc -l` counts newline characters in its input.

## Pipes and Redirection

```bash
<command-1> | <command-2>     # stdout from the first command becomes stdin to the second
<command> > <file>            # write stdout, overwriting the file
<command> >> <file>           # append stdout to the file
<command> < <file>            # take stdin from the file
<command> 2> <file>           # send stderr to the file
<command> 2>> <file>          # append stderr to the file
<command> 2>&1                # send stderr wherever stdout currently goes
<command> 2>/dev/null         # discard stderr
```

Common chains:

```bash
sort data.txt | uniq -u
sort data.txt | uniq -d
sort data.txt | uniq -c
strings data.txt | grep '='
grep -i 'permission denied' deployment-errors.log | wc -l
history | grep 'ssh'
```

What the flags mean: `uniq -u` prints lines occurring once; `uniq -d` prints duplicate lines; `uniq -c` displays counts. `sort` comes first because `uniq` only compares adjacent lines.

Trap: `2>` is for errors; `>` is for ordinary output. `2>&1` redirects stderr to stdout's current destination, so order matters.

## Decoding and Unpacking

| I want to... | Pattern | Example |
|---|---|---|
| Decode Base64 | `base64 -d <file>` | `base64 -d data.txt` |
| Translate characters | `tr '<from>' '<to>' < <file>` | `tr 'a-z' 'A-Z' < notes.txt` |
| Apply ROT13 | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < <file>` | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt` |
| Reverse a hexdump | `xxd -r <file> > <output>` | `xxd -r data.txt > data.bin` |
| Identify a file format | `file <file>` | `file data.bin` |
| Unpack gzip | `gzip -d <file>.gz` | `gzip -d data.gz` |
| Unpack bzip2 | `bzip2 -d <file>.bz2` | `bzip2 -d data.bz2` |
| List a tar archive | `tar -tf <file>.tar` | `tar -tf archive.tar` |
| Extract a tar archive | `tar -xf <file>.tar` | `tar -xf archive.tar` |

ROT13, exactly how I used it in Bandit:

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

`tr` takes two separate character sets. In `A-Z`, the hyphen describes a range; the first set is the characters to replace, and the second is their corresponding destination. ROT13 keeps uppercase and lowercase separate. Applying ROT13 twice restores the original.

My repeat-until-readable method for layered files:

1. `file <name>` - identify the actual format, not just the extension.
2. Use the correct decompressor (`gzip -d` or `bzip2 -d`) or archive extractor (`tar -xf`). Some decompression tools expect a matching extension; renaming does not change a file's actual format.
3. Run `file` on the result and repeat until the result is readable text.
4. Read the final text file with `cat`.

Don't mix these up: `gzip` and `bzip2` decompress data; `tar` packages or extracts files. Base64 and ROT13 are encoding, not encryption. `xxd -r` rebuilds bytes from their hexadecimal representation.

## Remote and Network

| I want to... | Pattern | Example |
|---|---|---|
| Log in to a server | `ssh <user>@<host> -p <port>` | `ssh bandit0@bandit.labs.overthewire.org -p 2220` |
| Log in with a key | `ssh -i <keyfile> <user>@<host> -p <port>` | `ssh -i ./bandit14.key bandit14@bandit.labs.overthewire.org -p 2220` |
| Copy a file from a server | `scp -P <port> <user>@<host>:<remote-file> <destination>` | `scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./bandit14.key` |
| Connect to a TCP service | `nc <host> <port>` | `nc localhost 30000` |
| Check whether a TCP port accepts connections | `nc -vz <host> <port>` | `nc -vz localhost 30000` |
| Connect to a TLS service | `openssl s_client -connect <host>:<port>` | `openssl s_client -connect localhost:30001 -quiet -nocommands` |
| Test basic ICMP reachability | `ping -c <count> <host>` | `ping -c 4 192.168.1.50` |
| Look up a DNS record | `dig +short <host>` | `dig +short example.com` |

Flags I need to distinguish: `ssh -p` uses a lowercase `p` for the port; `scp -P` uses an uppercase `P`. `ssh -i` specifies the identity (private-key) file. `chmod 600` restricts who can read it. In `openssl s_client`, `-connect` specifies `host:port`, `-quiet` reduces output, and `-nocommands` disables interactive command handling.

Network traps: `localhost` means the machine running the command. Inside the Bandit SSH session, it's the Bandit server, not my laptop. `nc` communicates with a service; it isn't a shell. A successful `ping` checks ICMP reachability, not whether a website or application works. TLS encryption alone doesn't prove the server's identity; certificate verification is a separate step.

## Shortcuts and Getting Unstuck

| I want to... | Do this |
|---|---|
| See past commands | `history` |
| Run a numbered history entry | `!<number>` |
| Repeat the previous command | `!!` |
| Search command history | `Ctrl+R` (press again for older matches) |
| Accept a history-search result for editing in Bash | `Ctrl+J` |
| Autocomplete a name | `Tab` |
| Cancel a running command | `Ctrl+C` |
| Search the manual | `man <command>`, then `/word` and `q` to quit |
| Get quick help | `<command> --help` |

My troubleshooting order: read the error → check `pwd` and `ls -la` → verify the exact filename/permissions → check `man` or `--help` → retry the complete command. Don't suppress a meaningful error just to make the output look clean.

## Bandit Answer Key: Levels 0–15

These are the 16 challenges I completed, Levels 0–15. Each heading shows the level solved and the next level unlocked. These are compact command reminders based on my approved walkthroughs, not copies of their full explanations. No passwords or private-key contents are published. Each entry links to my fuller walkthrough in the neighbouring `bandit` folder.

### Level 0 → 1 — SSH and read the file

[Detailed walkthrough](../bandit/level-00.md)

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```

Why: Connect to the remote shell, list the home directory and read the password file. `-p 2220` selects Bandit's SSH port.

### Level 1 → 2 — A file literally named `-`

[Detailed walkthrough](../bandit/level-01.md)

```bash
ls -la
cat ./-
```

Why: `./-` explicitly names the local file instead of telling `cat` to read standard input. Input redirection (`cat < ./-`) also works.

### Level 2 → 3 — Spaces and leading hyphens

[Detailed walkthrough](../bandit/level-02.md)

```bash
ls -la
cat ./'--spaces in this filename--'
```

Why: Quotes preserve spaces; `./` keeps leading hyphens from being treated as options.

### Level 3 → 4 — Hidden files

[Detailed walkthrough](../bandit/level-03.md)

```bash
cd inhere
ls -la
cat '...Hiding-From-You'
```

Why: `-a` reveals hidden entries. All three leading dots are part of this filename.

### Level 4 → 5 — Find the human-readable file

[Detailed walkthrough](../bandit/level-04.md)

```bash
cd inhere
file ./-file*
cat ./-file07
```

Why: `file` inspects the actual contents of all wildcard matches. The ASCII-text result identifies what to read.

### Level 5 → 6 — Exact size, regular file, not executable

[Detailed walkthrough](../bandit/level-05.md)

```bash
find . -type f -size 1033c ! -executable
file ./inhere/maybehere07/.file2
cat ./inhere/maybehere07/.file2
```

Why: Include all three conditions, then confirm the match is readable text. `1033c` means exactly 1,033 bytes.

### Level 6 → 7 — Owner, group and exact size

[Detailed walkthrough](../bandit/level-06.md)

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

Why: Search the whole filesystem with the exact requested properties; discard expected inaccessible-directory errors. The matching path is printed to stdout, while `2>/dev/null` discards stderr.

### Level 7 → 8 — Find text inside a file

[Detailed walkthrough](../bandit/level-07.md)

```bash
grep 'millionth' data.txt
```

Why: Search for the matching line rather than opening and scrolling through the entire file.

### Level 8 → 9 — The line occurring exactly once

[Detailed walkthrough](../bandit/level-08.md)

```bash
sort data.txt | uniq -u
```

Why: Sorting makes equal lines adjacent so `uniq -u` can isolate the single-occurrence line.

### Level 9 → 10 — Readable strings inside binary data

[Detailed walkthrough](../bandit/level-09.md)

```bash
strings data.txt | grep '='
```

Why: Extract human-readable fragments, then filter by the equals-sign clue. I used one equals sign, not `==`.

### Level 10 → 11 — Base64

[Detailed walkthrough](../bandit/level-10.md)

```bash
base64 -d data.txt
```

Why: `-d` decodes the Base64 text. Encoding is reversible and isn't encryption.

### Level 11 → 12 — ROT13

[Detailed walkthrough](../bandit/level-11.md)

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Why: The source and destination character sets map each letter 13 positions ahead, preserving case.

### Level 12 → 13 — Hexdump plus repeated compression

[Detailed walkthrough](../bandit/level-12.md)

```bash
mktemp -d
# Copy data.txt into the directory mktemp printed, then cd into it.
xxd -r data.txt > data.bin
file data.bin

# Apply the appropriate tool to each layer, then inspect again:
gzip -d file.gz
bzip2 -d file.bz2
tar -tf archive.tar
tar -xf archive.tar
```

Why: Reconstruct the bytes, identify each layer using `file`, decompress or extract it, then repeat. The `file.gz`, `file.bz2` and `archive.tar` names above are illustrative, not a literal transcript or a fixed command sequence. Use the real filenames returned at each step. `mktemp -d` also returns a session-specific path.

### Level 13 → 14 — Transfer and use the SSH key

[Detailed walkthrough](../bandit/level-13.md)

From my local Linux environment:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./bandit14.key
chmod 600 ./bandit14.key
ssh -i ./bandit14.key bandit14@bandit.labs.overthewire.org -p 2220
```

After authenticating as `bandit14`:

```bash
cat /etc/bandit_pass/bandit14
```

Why: My working route was to transfer the supplied key, lock down its permissions and authenticate with `ssh -i`. The final `cat` runs only after logging in as `bandit14`. Never commit the actual private key.

### Level 14 → 15 — Send data to a TCP service

[Detailed walkthrough](../bandit/level-14.md)

From inside the `bandit14` SSH session:

```bash
nc localhost 30000
```

Why: Connect to the local TCP service, then submit the current password interactively to receive the next. `localhost` is the Bandit server here.

### Level 15 → 16 — Submit data over TLS

[Detailed walkthrough](../bandit/level-15.md)

From inside the `bandit15` SSH session:

```bash
openssl s_client -connect localhost:30001 -quiet -nocommands
```

Why: The service requires TLS instead of an ordinary unencrypted TCP connection. Submit the current password interactively after connecting; an encrypted connection alone does not establish server identity.

Scope: This sheet covers my completed Bandit Levels 0–15. For the full reasoning, follow the linked Bandit walkthroughs.
