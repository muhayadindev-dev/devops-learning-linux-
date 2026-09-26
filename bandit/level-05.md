# Bandit Level 5 → Level 6

## Goal

Find a human-readable file inside `inhere` that is exactly 1,033 bytes and isn't executable.

## Concept

The `find` command searches directory trees and combines conditions such as file type, size and permissions to narrow down results.

## Commands Used

Starting from the home directory:

```bash
find . -type f -size 1033c ! -executable
file ./inhere/maybehere07/.file2
cat ./inhere/maybehere07/.file2
```

## What I Did

I used `find` to locate regular files that were exactly 1,033 bytes and weren't executable.

I then used `file` to confirm that the matching file contained readable text before displaying its contents with `cat`.

## What I Learned

This challenge demonstrated how multiple `find` conditions can narrow down a search without manually inspecting directories.

`-type f` selects regular files. `-size 1033c` specifies exactly 1,033 bytes. `! -executable` excludes files executable by the current user.

I also reinforced how `file` can be used to identify a file's data type.

