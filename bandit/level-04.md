# Bandit Level 4 → Level 5

## Goal

Identify and read the only human-readable file inside the `inhere` directory.

## Concept

The `file` command identifies a file's data type by examining its contents rather than relying on its name or extension.

Wildcards allow multiple files to be inspected with a single command.

## Commands Used

```bash
cd inhere
file ./-file*
cat ./-file07
```

## What I Did

I used `file` with a wildcard to inspect all the files inside `inhere`.

The `./` prefix prevented filenames beginning with hyphens from being interpreted as options.

The results identified `-file07` as ASCII text, which I then read using `cat`.

## What I Learned

This challenge demonstrated how to identify a file's actual data type before reading it, while using wildcards to inspect multiple files efficiently.

