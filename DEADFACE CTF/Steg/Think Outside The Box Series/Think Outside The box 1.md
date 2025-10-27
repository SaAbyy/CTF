# CTF Write-up — “Think Outside the Box 1"

## Challenge Description

We need, for this CTF, to think outside the box LITERALLY.

This implied something hidden outside the visible boundaries of the image — specifically in the **image dimensions**.

---

## Analysis

JPEG files contain structured markers. The **SOF (Start Of Frame)** marker declares the image **height** and **width**. For SOF, the relevant marker bytes are `FFC0` (baseline) or `FFC2` (progressive).

Open the file in a hex editor and locate the SOF segment. In this case we found:

```
ff c2 00 11 08 01 79 01 f4
```

Breakdown:

| Bytes   |                          Meaning | Value |
| ------- | -------------------------------: | ----: |
| `ff c2` |                      SOF2 marker |     — |
| `00 11` |              Segment length (17) |     — |
| `08`    |               Precision (8 bits) |     — |
| `01 79` | **Height** = 0x0179 = **377 px** |       |
| `01 f4` |  **Width** = 0x01F4 = **500 px** |       |

So the image is declared as **500×377**.

---

## The Trick

The JPEG file contains more scan data than what the declared height shows. By increasing the declared **height**, the viewer will render additional scanlines from the file as visible image pixels.

Modify the height bytes from `01 79` to `03 79`:

```
ff c2 00 11 08 01 79 01 f4

# becomes

ff c2 00 11 08 03 79 01 f4
```

Now the height becomes:

```
0x0379 = 889 pixels
```

Re-save the file and open it in an image viewer — the extra data below the original visible area becomes visible and reveals the hidden content/flag.

---

## Steps (practical)

1. Make a copy of the original JPG file.
2. Open the copy in a hex editor (e.g., `xxd`, `Bless`, `HxD`).
3. Search for the SOF marker `ff c2` (or `ff c0` in other files).
4. Locate the two bytes after the precision byte — they represent the height in big-endian.
5. Edit those two bytes to a larger value (for example `01 79` → `03 79`).
6. Save the file and open it with your image viewer.
7. Inspect the newly visible area for the flag or hidden message.

OR

Do theses steps with : https://cyberchef.io/

---

## Result

After editing the SOF height and reopening the image, the hidden part of the picture became visible — the flag (or hidden message) was revealed in the expanded area.

This technique works because the JPEG file contains extra scan data beyond the declared image height; viewers will render those extra scanlines when the SOF height is increased.

---

## Reference

Original write-up on the same technique (author => myself):

[https://medium.com/@sa4by/cacher-des-informations-grâce-aux-dimensions-dune-image-jpg-56ccd1622f68](https://medium.com/@sa4by/cacher-des-informations-grâce-aux-dimensions-dune-image-jpg-56ccd1622f68)

---

## Flag

**deadface{jp3g_alt3red_he1ght!}**

<img width="501" height="770" alt="image" src="https://github.com/user-attachments/assets/b9685289-bb76-4d8f-adb9-fac21b96a1cb" />

*(The flag is visible in the expanded region of the patched image)*
