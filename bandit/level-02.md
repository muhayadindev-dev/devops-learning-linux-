# Bandit Level 2 → Level 3

## Goal

Read a file whose name contains spaces and begins with hyphens to retrieve the password for the next level.

## Concept

Linux allows filenames to contain spaces and special characters, but these need to be handled correctly when passing them to commands.

Quotation marks prevent Bash from treating spaces within a filename as separators between arguments. An explicit relative path, such as `./`, ensures that filenames beginning with a hyphen aren't interpreted as command options.

## Commands Used

```bash
ls -la
cat ./'--spaces in this filename--'
```

## What I Did

I inspected the home directory to identify the exact filename.

I enclosed the filename in single quotation marks to preserve its spaces and prefixed it with `./` to reference the file in my current directory.

This allowed `cat` to read the file and display the password for the next level.

## What I Learned

This challenge built on the previous level by combining two techniques for handling unusual filenames.

Quotation marks prevent Bash from splitting a filename containing spaces into multiple arguments, while `./` provides an explicit relative path.

The main takeaway was understanding that these techniques solve different problems and can be used together when necessary.

## Alternative Solution

```bash
cat -- '--spaces in this filename--'
```

The `--` argument stops option parsing, while quotation marks preserve the spaces in the filename.

