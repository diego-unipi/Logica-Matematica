---
description: "Use when correcting or rewriting Italian text in chat or files. Prevent apostrophe-style replacements of accented letters."
name: "Italian Accents Preservation"
applyTo: "**"
---
# Italian Accents Preservation

Hard rule for assistant-authored output.

## Absolute Prohibition
- Never replace accented Italian letters with apostrophe approximations.
- Forbidden substitutions include patterns like `e\`` , `a'`, `i'`, `o\`` , `u\`` when they stand in for accented letters.

## Correction Behavior
- During corrections, preserve user-authored accented letters exactly.
- In assistant-authored Italian prose, write proper accented letters and never apostrophe-based substitutes.
- This rule applies to chat responses and generated file edits.

## Safety Check
Before sending corrected text:
1. Scan for apostrophe-style accent surrogates in Italian words.
2. Replace them with proper accented letters.
3. Verify no surrogate accent pattern remains.
