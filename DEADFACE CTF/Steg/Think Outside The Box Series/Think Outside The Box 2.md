# CTF Write-up — "Think Outside the Box 2"

## Challenge Description

This challenge provided a **GIF** file with the hint to "think outside the box" again. The GIF appeared normal, but steganographic content was suspected.

---

## Tools Used

* `gift` — a command-line tool for GIF steganography (from [https://dtm.uk/gif-steganography/](https://dtm.uk/gif-steganography/)).

---

## Analysis & Steps

1. Download the `gift` tool from the linked page and install it according to the project's instructions.
2. Run the analyzer to inspect the GIF and extract frames:

```bash
gift analyze challenge.gif
```

3. The tool reports metadata about the GIF and dumps individual frames to disk.
4. Inspect the dumped frames (or use a quick image viewer) — pay attention to frame ranges where subtle differences may appear.

---

## Discovery

Frames **172** to **185** contain visible text when viewed sequentially. The extracted text reads:


<img width="523" height="527" alt="image" src="https://github.com/user-attachments/assets/6d317e53-be86-440c-b8a3-38b95abef67e" />

## Flag

```
deadface{cuT_th3_f33D!!}
```
