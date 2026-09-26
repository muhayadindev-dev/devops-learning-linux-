# Bandit Level 8 → Level 9

## Goal

Find the only line in `data.txt` that occurs exactly once.

## Concept

`uniq` compares adjacent lines. Sorting the file first groups identical lines together, allowing `uniq -u` to identify lines that occur only once.

## Commands Used

```bash
sort data.txt | uniq -u
```

## What I Did

I sorted `data.txt` and piped the output into `uniq -u`, which returned the unique line containing the password.

## What I Learned

This challenge demonstrated how pipelines combine commands, passing the output of one into the input of another.

It also reinforced why the order of commands matters when processing data.

