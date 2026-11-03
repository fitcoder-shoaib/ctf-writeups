# 👻 Mmm hidden flag — Writeup

**Category:** Misc / Esoteric language
**Flag:** `CYBTBR{Difficulty_level_extreme}`

---

## Challenge

The challenge description looks **empty**. It is made only of spaces, tabs and line breaks. The attached `flag.txt` has the same invisible content (856 bytes).

---

## Solution

### 1. Make the invisible visible

Showing the whitespace characters (here `S` = space, `T` = tab, `L` = line feed) gives a repeating pattern:

```
S S S T T T S T S S L      <- push a number
T L S S                    <- print it as a character
S S S T T S T S S S L
T L S S
...
```

That pattern is **Whitespace**, an esoteric programming language whose source code uses only space, tab and line feed. Every other character is ignored.

### 2. Read the instructions

| Whitespace | Meaning |
|------------|---------|
| `S S` | Push a number onto the stack |
| next `S` / `T` | Sign: `S` = positive, `T` = negative |
| following `S`/`T` up to `L` | The number in binary, `S` = 0, `T` = 1 |
| `T L S S` | Pop the top number and print it as an ASCII character |

Example: `S S` `S` `TTTSTSS` `L` pushes binary `1110100` = 116 = `t`, and `TLSS` prints it.

### 3. Decode

Run the file through any Whitespace interpreter (for example an online one), or decode it with a few lines of Python:

```python
import re

src = open("flag.txt", encoding="utf-8").read()
src = "".join(c for c in src if c in " \t\n")          # drop \r and anything else

# every "push" is: two spaces, sign, binary digits, line feed
nums = []
for sign, bits in re.findall(r"  ([ \t])([ \t]*)\n", src):
    n = int(bits.replace(" ", "0").replace("\t", "1") or "0", 2)
    nums.append(-n if sign == "\t" else n)

print("".join(map(chr, nums)))
```

Output:

```
this is the flag

CYBTBR{Difficulty_level_extreme}
```

---

## Takeaways

- A challenge whose text or file looks empty is often hiding data in **whitespace**. Open it in an editor that shows spaces and tabs, or dump it with `cat -A` or `xxd`.
- Only spaces, tabs and line feeds, with `S S` pushes and `T L S S` prints, means the **Whitespace** language. A tab-only or space-only pattern elsewhere might instead be binary or SNOW steganography.
- Windows line endings (`\r\n`) can break interpreters. Strip `\r` first.
