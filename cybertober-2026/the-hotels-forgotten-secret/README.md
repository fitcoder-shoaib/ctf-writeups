# 🦇 The Hotel's Forgotten Secret — OSINT Writeup

**Category:** OSINT / Web Archive
**Flag:** `cytbr{dracula_ate_the_last_garlic_bread}`

---

## Challenge

> Welcome to Hotel Transylvania, where monsters check in, but secrets never check out.
>
> Something has vanished from the hotel, and management claims nothing is missing. Yet the website may not tell the whole story. The hotel has changed over time, and somewhere in its digital past, a forgotten secret may still remain.
>
> Explore the Hotel Transylvania website, investigate its digital footprint, and follow the traces left behind. Look beyond what is visible today to uncover what the hotel once revealed.
> [link](https://hotel-transy1vania.netlify.app/)
>
> Submission: Submit the flag in the format `cytbr{...}`.
>
> *The present hides what the past remembers.*

Target: <https://hotel-transy1vania.netlify.app/>

---

## Solution

### 1. Look at the live site

The site is a Next.js app (hosted on Netlify) with three pages: **Home**, **Facilities** and **Locked**.

```bash
curl -sL https://hotel-transy1vania.netlify.app/
curl -sL https://hotel-transy1vania.netlify.app/locked/
```

The `/locked/` page has a "forbidden vault" with a **DISCOVER THE SECRETS** button, but the current version shows no flag. The page source and the current JavaScript chunks have nothing useful either.

### 2. Read the hint

| Clue | Meaning |
|------|---------|
| "The hotel has changed over time" | The site was redeployed, and an older version was different |
| "somewhere in its digital past" | Look at archived copies of the site |
| "The present hides what the past remembers" | The Wayback Machine (web.archive.org) |

### 3. Query the Wayback Machine

The Wayback CDX API lists every URL archived for a domain:

```bash
curl -s "https://web.archive.org/cdx/search/cdx?url=hotel-transy1vania.netlify.app&matchType=domain&output=txt"
```

Result: just one entry, an old JavaScript chunk that the live site no longer loads:

```
app,netlify,hotel-transy1vania)/_next/static/chunks/0n5t7kqi_ilks.js 20261031183810 https://hotel-transy1vania.netlify.app/_next/static/chunks/0n5t7kqi_ilks.js application/javascript 200 ...
```

### 4. Download the archived chunk

Adding `id_` after the timestamp returns the raw file, without the Wayback toolbar. The response is gzip-compressed, so decompress it first:

```bash
curl -sL "https://web.archive.org/web/20261031183810id_/https://hotel-transy1vania.netlify.app/_next/static/chunks/0n5t7kqi_ilks.js" -o chunk.gz
gunzip -c chunk.gz > chunk.js
grep -o 'cytbr{[^}]*}' chunk.js
```

### 5. Find the flag

The old chunk has the earlier version of the `LockedVault` component. In that version, clicking **DISCOVER THE SECRETS** opened a popup ("You broke into the vault.") with the flag in plain text:

```js
(0,t.jsxs)("div",{className:"flag-box",children:[
  (0,t.jsx)("span",{children:"RECOVERED CHALLENGE FLAG"}),
  (0,t.jsx)("code",{children:"cytbr{dracula_ate_the_last_garlic_bread}"})
]})
```

```
cytbr{dracula_ate_the_last_garlic_bread}
```

---

## Takeaways

- Removing a secret from a site doesn't remove it from the internet. Archives such as the Wayback Machine may still have older copies.
- The **CDX API** lists every archived URL for a domain, including JS, CSS and image files, not just HTML pages.
- On modern frameworks (Next.js, React) the content is often inside JavaScript chunks, so check those, not only the HTML.
- Use the `id_` suffix to get raw archived files, and remember they may be gzip-compressed.
