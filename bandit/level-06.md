# Bandit Level 6 → Level 7

## Goal

Find a file anywhere on the server that is owned by `bandit7`, belongs to group `bandit6` and is exactly 33 bytes.

## Concept

`find` can search the entire filesystem and combine conditions such as file type, ownership, group and size.

## Commands Used

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

## What I Did

I combined the ownership, group and size conditions to locate the matching file.

I redirected permission errors to `/dev/null` and read the file using its absolute path.

## What I Learned

This challenge extended my use of `find` to searching by ownership and group across the filesystem.

It also demonstrated how `2>/dev/null` discards standard error without suppressing the search results on standard output.

