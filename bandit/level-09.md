# Bandit Level 9 → Level 10

## Goal

Find the password hidden among readable strings in `data.txt`, preceded by several equals signs.

## Concept

`strings` extracts readable text from binary data, while `grep` filters that output for a specified pattern.

## Commands Used

```bash
strings data.txt | grep "="
```

## What I Did

I used `strings` to extract readable text from `data.txt` and piped the output into `grep`, using a single equals sign to locate the relevant line.

## What I Learned

This challenge demonstrated how two simple commands can work together to extract and filter information from binary data.

It also reinforced that an effective solution doesn't need to be unnecessarily complicated.

