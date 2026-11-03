# 👻 The Haunting Coordinates — OSINT Writeup

**Category:** OSINT
**Flag:** `CYBTBR{408-247-2000}`

---

## Challenge

> Somewhere between history and haunting lies a trail waiting to be uncovered.
>
> *Was she instructed to build this home by a psychic? Was she haunted by the ghosts of those felled by the "Gun that Won the West"?*
>
> Strange stories surround this mysterious mansion, where history and supernatural whispers intertwine. But legends alone won't reveal the truth.
>
> Look beyond the legend. Follow the whispers. Find the house.
>
> Its name is not what we seek. Its location is only the beginning.
>
> Uncover the number that connects the living to this haunted mansion.
>
> Find its official telephone number.
>
> Flag Format: `CYBTBR{XXX-XXX-XXXX}`

No files are given. Everything comes from the text.

---

## Solution

### 1. Match the clues

| Clue | Meaning |
|------|---------|
| "the Gun that Won the West" | Nickname of the **Winchester** repeating rifle |
| "she" | **Sarah Winchester**, widow of William Wirt Winchester, heir to the rifle company |
| "instructed to build this home by a psychic" | The legend that a medium told her to keep building without stopping |
| "haunted by the ghosts of those felled by" the rifle | She was said to be hiding from the spirits of people killed by Winchester rifles |
| "this mysterious mansion" | The house, with staircases that lead nowhere and doors that open onto walls |

All of this points to the **Winchester Mystery House**.

### 2. Find the location

The house is at **525 S. Winchester Blvd, San Jose, California**. As the challenge says, the name and location are "only the beginning": the flag is the phone number.

### 3. Find the official phone number

The official website, [winchestermysteryhouse.com](https://winchestermysteryhouse.com), lists the main line as **(408) 247-2000**. The 408 area code matches San Jose.

### 4. Build the flag

The format is `XXX-XXX-XXXX`:

```
CYBTBR{408-247-2000}
```

---

## Takeaways

- Famous nicknames ("the Gun that Won the West") are strong pointers. Look them up first.
- Read what the flag actually asks for. Here the name and address were only steps toward the phone number.
- Take contact details from the place's **official** website, not from directories, which can be out of date.
