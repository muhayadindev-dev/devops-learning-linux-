# Bandit Level 11 → Level 12

## Goal

Decode the ROT13-encoded text in `data.txt` to retrieve the password.

## Concept

ROT13 shifts each letter 13 positions through the alphabet, preserving its case.

The `tr` command translates characters from one set into their corresponding characters in another.

## Commands Used

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

## What I Did

I piped the contents of `data.txt` into `tr`, mapping uppercase and lowercase letters to their ROT13 equivalents to decode the password.

## What I Learned

This challenge demonstrated how `tr` performs character substitutions.

Each character in the first set maps to the corresponding position in the second. Applying ROT13 twice restores the original text.

