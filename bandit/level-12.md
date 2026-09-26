# Bandit Level 12 → Level 13

## Goal

Recover the password from a hexdump of a file that has been compressed and archived multiple times.

## Concept

`xxd -r` reconstructs binary data from a hexdump, while `file` identifies its format.

gzip and bzip2 decompress files, whereas tar extracts archives.

## Commands Used

I created a temporary directory and reconstructed the original data:

```bash
mktemp -d
cp data.txt /tmp/tmp.cPp1rTwXsn/
cd /tmp/tmp.cPp1rTwXsn

xxd -r data.txt > data.bin
file data.bin
```

I then used the appropriate commands for each identified format:

```bash
gzip -d file.gz
bzip2 -d file.bz2
tar -tf archive.tar
tar -xf archive.tar
```

*The temporary directory path is from my session; yours will differ. The extraction filenames are examples, not a fixed sequence.*

## What I Did

I reconstructed the hexdump and used `file` to identify each layer.

I then decompressed or extracted the data using the appropriate tool, repeating the process until I reached the readable file containing the password.

## What I Learned

This challenge brought together several file-processing tools and demonstrated the difference between compression and archiving.

The key was following a consistent process: **identify the format, decompress or extract the data, then inspect the result.**

It also highlighted how repetitive tasks could be automated using Bash.

