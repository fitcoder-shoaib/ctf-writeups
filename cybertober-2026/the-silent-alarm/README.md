# 📻 The Silent Alarm — OSINT Writeup

**Category:** OSINT / Audio
**Flag:** `CYBTBR{26_04_1986}`

---

## Challenge

> A series of radio transmissions and emergency calls were recorded on a night that changed history forever.
>
> Panic, confusion, unanswered questions — voices captured in the middle of an unfolding catastrophe.
>
> Listen carefully. Every transmission holds a fragment of the truth.
>
> Piece together the conversations, follow the timeline, and uncover the day when everything changed.
>
> **The voices remember. Do you?**
>
> Flag Format: `CYBTBR{DD_MM_YYYY}`

We are given one file, `Audio.ogg` (about 70 seconds).

---

## Solution

### 1. Read the file's tags

Before listening, check the metadata. Ogg Vorbis files keep their tags as plain text near the start of the file, so `strings` is enough:

```bash
strings -n 5 Audio.ogg | head -20
```

```
encoder=Lavc58.34.100 libvorbis
DESCRIPTION=
genre=Speech
artist=
title=
date=1986
```

`exiftool Audio.ogg` or `ffprobe Audio.ogg` show the same tags.

The genre is **Speech** and the date is **1986**.

### 2. Match the clues

| Clue | Meaning |
|------|---------|
| `date=1986` | The recording is from 1986 |
| "radio transmissions and emergency calls" | Recorded dispatcher and responder communications |
| "on a **night** that changed history" | The disaster happened at night |
| "an unfolding catastrophe" | A disaster that kept getting worse over hours and days |

The most famous disaster of 1986 that happened at night is **Chernobyl**. Reactor 4 exploded at **01:23 on 26 April 1986**, and recordings of the firefighters' radio traffic and emergency calls from that night are well known. The other big 1986 disaster, the Challenger launch, happened in daylight, so it doesn't fit "night".

### 3. Build the flag

Format `DD_MM_YYYY`:

```
CYBTBR{26_04_1986}
```

---

## Takeaways

- Check the file's metadata before spending time on the content. Here a single `date=` tag narrowed the search to one year.
- Use every word of the story. "Night" ruled out the other major disaster of the same year.
- `strings` works well on audio containers like Ogg and MP3, whose tags are stored as plain text.
