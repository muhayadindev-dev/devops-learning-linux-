# Bandit Level 0 → Level 1

## Goal

Connect to the Bandit server using SSH and read the `readme` file containing the password for the next level.

## Concept

SSH establishes an encrypted connection to a remote machine and allows me to interact with its shell. The Bandit server uses port `2220`, which is specified using the `-p` option.

Once connected, I can navigate the remote filesystem and execute Linux commands as I would in my own terminal.

## Commands Used

First, I connected to the Bandit server from my local terminal:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Once logged in, I ran:

```bash
ls
cat readme
```

## What I Did

I connected to the Bandit server using SSH, specifying the username, hostname and port.

Once logged in, I used `ls` to inspect the home directory and identify the `readme` file. I then used `cat` to display its contents and retrieve the password for the next level.

## What I Learned

This challenge gave me practical experience connecting to a remote Linux machine using SSH.

It reinforced how the username, hostname and port work together to establish a connection. Once connected, I could navigate the remote filesystem and use familiar commands to inspect and read files.

