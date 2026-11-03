# 🦇 Wing In The Dark — OSINT Writeup

**Category:** OSINT / Social Media
**Flag:** `CYBTBR{b4ts_0wn_th3_n1ght}`

---

## Challenge

> Something has taken flight in the digital shadows.
>
> Strange sightings have been reported across the network. An unfamiliar symbol keeps appearing, accompanied by cryptic references to creatures that thrive in the darkness.
>
> VEIL seems to have left traces across multiple digital identities. No single account holds the complete truth. Three fragments remain scattered across the shadows, waiting to be discovered.
>
> Your investigation will take you across different social media profiles. Observe carefully, connect the clues, and recover all three fragments to reveal the hidden message.
>
> The night belongs to those who know where to look. Can you piece together what is hiding in the dark?

No files are given.

---

## Solution

### 1. Fragment 1: µLearn Cyber IG's Instagram story

The organisers' Instagram account, **@mulearn.cyberig**, posted a 24-hour story containing only one line of text:

```
Q1lCVEJSe2I0dHNf
```

It's Base64 (`Q1lCVEJS` always decodes to `CYBTBR`). The font makes `I`/`l` and `0`/`O` look alike. The reading that decodes to readable text is:

```bash
echo -n Q1lCVEJSe2I0dHNf | base64 -d
# CYBTBR{b4ts_
```

"b4ts" (bats) matches the bat theme of the challenge. Misreading the characters gives `CYBTBR{iNts_`, which is wrong.

### 2. Fragment 3: Ekholon's Instagram story

The co-organiser **@ekholon** posted a story showing a tombstone under a full moon with bats. Under "EST. 2024 · OCTOBER 31" it reads:

```
th3_n1ght}
```

The closing `}` shows this is the last fragment.

### 3. Fragment 2: the missing middle

The challenge's own last line fills the gap: "*The night belongs to those who…*". Bats **own** the night:

```
b4ts_ + 0wn_ + th3_n1ght}
```

### 4. Build the flag

```
CYBTBR{b4ts_0wn_th3_n1ght}
```

---

## Takeaways

- Instagram stories disappear after 24 hours, so check the organisers' stories during the event.
- In screenshots of Base64, try the look-alike characters (`I`/`l`, `0`/`O`). Only one combination gives readable text.
- Use the challenge text itself. Its theme (bats, "the night belongs to…") helps confirm and complete the fragments.
