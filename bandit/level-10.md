# Bandit Level 10 → Level 11

## Goal

Decode the Base64-encoded data stored in `data.txt` to retrieve the password for the next level.

## Concept

Base64 converts data into a representation using printable characters. It is an encoding method, not encryption, and can be reversed using `base64 -d`.

## Commands Used

```bash
base64 -d data.txt
```

## What I Did

I checked `man base64` to identify the decoding option, then used `-d` to decode `data.txt` and display the password.

## What I Learned

This challenge reinforced the distinction between encoding and encryption.

It also demonstrated how manual pages can help me identify the correct options rather than memorising every command flag.

