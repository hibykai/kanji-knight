# Kanji Knight

A single-file browser game that drills the most-common Jōyō kanji. A knight stands on the left of a 2D arena while a hungry T-Rex slowly walks in from the right. Read the kanji before he eats you.

![Kanji Knight gameplay screenshot placeholder](docs/screenshot.png)

## Play it

Live at **https://kanji-knight.vercel.app/** (or your own Vercel domain), or open `index.html` locally — double-click works, no server needed.

## How to play

1. Open `index.html` in any modern browser (double-click works — no server needed).
2. On the start screen, configure:
   - **Display readings as:** Hiragana or Romaji.
   - **JLPT difficulty (cumulative):** N5 (easiest, ~80 kanji) through N1 (all ~2000). Selecting N3 includes everything from N5–N3, so higher levels are supersets of lower ones.
   - **T-Rex reaches knight in:** drag the slider, 5–120 seconds (default 30). Shorter = harder.
3. Each round:
   - You get **5 seconds** to study a kanji.
   - Then 3 cards appear with possible readings — only one is correct.
   - There's no time limit on your choice, but the T-Rex is walking toward the knight.
4. **Wrong answer:** a red ✕ appears on the card you picked, and the T-Rex speeds up to 1.5×.
5. **Right answer:** the T-Rex stops, score goes up, next kanji.
6. Crucially, the T-Rex **does not reset between rounds** — the position carries forward. At base speed he reaches the knight in your configured time (default 30s) total, so quick correct answers = higher score before he gets you.
7. Game over when he reaches the knight. Your score is the number of kanji you read correctly.

## What's inside

- Single self-contained `index.html` (~70 KB) — HTML, CSS, JS, SVG sprites, and ~2,000 Jōyō kanji entries all inline.
- No build step, no dependencies, no fetches — runs from `file://`.
- Hiragana → Romaji conversion done at runtime via a Hepburn-style lookup table, so the dataset only stores hiragana.
- Distractors are auto-picked from the rest of the dataset, preferring readings of the same length or that share characters with the correct answer.

## Editing the kanji dataset

Each entry in the file looks like:

```js
{k:"漢", h:"かん"}
```

One canonical reading per kanji (most-common kun-yomi where it exists, otherwise on-yomi). A runtime de-dupe keeps only the first occurrence of each kanji, so you can append corrections at the bottom of any `pushChunk(...)` block without worrying about overwriting earlier entries.

The dataset is split into 11 chunks, each tagged with a JLPT level:

```js
pushChunk(5, /* Grade 1 → N5 */ {k:"一",h:"いち"}, ...);
pushChunk(4, /* Grade 2 → N4 */ ...);
pushChunk(3, /* Grades 3-4 → N3 */ ...);
pushChunk(2, /* Grades 5-6 → N2 */ ...);
pushChunk(1, /* Secondary-school → N1 */ ...);
```

Because chunks are pushed easiest-first and the dedupe keeps the first occurrence, every kanji ends up tagged with its **easiest** JLPT band, which is exactly what cumulative level filtering needs. To re-classify a kanji, move its entry into a different `pushChunk(...)` block. To swap a reading, find the entry and edit the `h:` value.

## Hosting

This is a single static HTML file, so any static host works with zero config:

- **Vercel** — connect the repo, accept the defaults. Vercel serves `index.html` from `/` automatically.
- **GitHub Pages** — enable Pages in the repo settings (Source: `main` / `(root)`). The game will be live at `https://<your-user>.github.io/kanji-knight/`.
- **Netlify / Cloudflare Pages** — same drill, point at the repo, no build command, no publish directory override needed.

## License

MIT
