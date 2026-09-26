# Bandit Level 3 → Level 4

## Goal

Locate and read a hidden file inside the `inhere` directory to retrieve the password for the next level.

## Concept

In Linux, files beginning with a dot (`.`) are hidden from ordinary directory listings.

`ls -a` displays hidden files, while `ls -la` also shows details such as permissions, ownership and file size.

## Commands Used

```bash
cd inhere
ls -la
cat '...Hiding-From-You'
```

## What I Did

I navigated into `inhere` and used `ls -la` to display all files, including hidden ones.

I identified `...Hiding-From-You` and used `cat` to read its contents.

## What I Learned

This challenge reinforced the importance of checking hidden files when inspecting a directory.

It also clarified the distinction between `.` (current directory), `..` (parent directory) and filenames beginning with dots.

In this case, all three leading dots were part of the filename.

