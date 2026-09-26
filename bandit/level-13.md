# Bandit Level 13 → Level 14

## Goal

Use the provided SSH private key to authenticate as `bandit14` and retrieve its password.

## Concept

SSH supports private-key authentication. The `-i` option selects the private key, while `chmod 600` restricts access to its owner.

`scp` securely transfers files between machines over SSH.

## Commands Used

From my local Linux terminal:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./bandit14.key

chmod 600 ./bandit14.key

ssh -i ./bandit14.key bandit14@bandit.labs.overthewire.org -p 2220
```

Once connected:

```bash
cat /etc/bandit_pass/bandit14
```

## What I Did

I transferred the private key to my Linux environment using `scp`, restricted its permissions with `chmod 600` and used it to authenticate as `bandit14`.

I then retrieved the password from the designated file.

## What I Learned

This challenge demonstrated how SSH private-key authentication works alongside file permissions and secure file transfers.

It also reinforced the distinction between `scp -P` for the transfer port, `ssh -p` for the connection port and `ssh -i` for selecting a private key.

