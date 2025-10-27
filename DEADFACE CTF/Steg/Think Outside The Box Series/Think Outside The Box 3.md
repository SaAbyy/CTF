# CTF Write-up — "Think Outside the Box 3"

## Challenge Description

A suspicious image was provided with the hint to "think outside the box". Visual steganography was suspected, information hidden across color channels or composite layers rather than in file metadata.

---

## Tool

Use **Aperisolve** ([https://www.aperisolve.com/](https://www.aperisolve.com/)).

## Analysis & Steps

1. Open the provided image in Aperisolve: `https://www.aperisolve.com/` → **Load image** (drag & drop or paste URL).
2. Look at the **Decomposer** view (in Aperisolve this may appear under decomposition filters or layer analysis).
3. Inspect each decomposed frame carefully.

## Flag

**deadface{Th3_b0X_d0esnT_eX1st}**
