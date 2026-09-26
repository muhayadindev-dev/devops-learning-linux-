# Bandit Level 1 → Level 2

## Goal

Read the contents of a file named `-` in the home directory to retrieve the password for the next level.

## Concept

Some filenames have special meanings when passed to Linux commands. In this case, `cat` interprets a single hyphen (`-`) as standard input rather than a filename.

Using an explicit relative path removes that ambiguity. The `./` prefix refers to the current directory, so `./-` identifies the file named `-` rather than asking `cat` to read from standard input.

## Commands Used

```bash
ls -la
cat ./-
```

## What I Did

I inspected the home directory using `ls -la` and identified the file named `-`.

I then used `./-` to reference the file directly. This allowed `cat` to read its contents and retrieve the password for the next level.

## What I Learned

This challenge demonstrated how Linux commands can interpret unusual filenames differently.

Using `./` makes the filename an explicit relative path, allowing me to work with files that would otherwise have special meanings.

It also introduced an alternative approach using input redirection, where the shell opens the file and passes its contents to `cat` through standard input.

## Alternative Solution

```bash
cat < ./-
```

Here, `<` tells the shell to open the file and pass its contents to `cat` through standard input.

