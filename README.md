# Kanji Knight

A single-file browser game that drills JLPT-level Japanese vocabulary. A knight stands on the left of a 2D arena while a hungry T-Rex slowly walks in from the right. Read the word before he eats you.

![Kanji Knight gameplay screenshot placeholder](docs/screenshot.png)

## Play it

Live at **https://kanji-knight.vercel.app/** (or your own Vercel domain), or open `index.html` locally — double-click works, no server needed.

## How to play

1. Open `index.html` in any modern browser (double-click works — no server needed).
2. On the start screen, configure:
   - **Display readings as:** Hiragana or Romaji.
   - **JLPT difficulty (cumulative):** N5 (easiest, ~80 kanji) through N1 (all ~2000). Selecting N3 includes everything from N5–N3, so higher levels are supersets of lower ones.
   - **Study time per kanji:** drag the slider, 1–15 seconds (default 5). How long the kanji card is shown before the answer options appear.
   - **T-Rex reaches knight in:** drag the slider, 5–120 seconds (default 30). Shorter = harder.
3. Each round:
   - You get your configured **study time** (default 5 seconds) to study a word (kanji + kana mix).
   - Then 3 cards appear, each showing a **reading** and **English meaning** — only one matches the word.
   - There's no time limit on your choice, but the T-Rex is walking toward the knight.
4. **Wrong answer:** a red ✕ appears on the card you picked, and the T-Rex speeds up to 1.5×.
5. **Right answer:** the T-Rex stops, score goes up, next word.
6. Crucially, the T-Rex **does not reset between rounds** — the position carries forward. At base speed he reaches the knight in your configured time (default 30s) total, so quick correct answers = higher score before he gets you.
7. Game over when he reaches the knight. Your score is the number of words you read correctly.

## Power-ups

Build a hot streak of correct answers and you'll earn a random power-up. The streak threshold scales with difficulty:

- **Hardest** (1s study + 5s T-Rex) → power-up every **3** correct
- **Easiest** (15s study + 120s T-Rex) → power-up every **8** correct
- A wrong answer or a granted power-up resets the streak

Click an earned power-up icon (top-right, below the score) to use it:

| Icon | Power-up | Effect |
|------|----------|--------|
| ⚔️ | Blade Attack | Knock the T-Rex back ~18% of the arena |
| ❄️ | Time Freeze | Stop the T-Rex in place for 5 seconds |
| ⚖️ | 50/50 | Mark one wrong card with an ✕ automatically |
| 🛡️ | Shield | Absorb the speed-up on your next wrong answer |
| ⏭️ | Skip | Skip the current word with no score penalty |

## What's inside

- Single self-contained `index.html` (~108 KB) — HTML, CSS, JS, SVG sprites, ~500 JLPT-tagged words, and ~2,000 Jōyō kanji entries all inline.
- No build step, no dependencies, no fetches — runs from `file://`.
- Hiragana → Romaji conversion done at runtime via a Hepburn-style lookup table, so the dataset only stores hiragana.
- Distractors are auto-picked from the rest of the dataset, preferring readings of the same length or that share characters with the correct answer. Distractor readings and meanings are always different from the correct one so there's exactly one right answer.
- The kanji-only quiz code path is preserved in the file (`KANJI_DATA`, `pushChunk`) but no longer queried by `activePool()`. To switch back to single-kanji drills, change `activePool()` to filter `KANJI_DATA` instead of `WORDS_DATA`.

## Editing the words dataset

Each entry in the file looks like:

```js
{w:"食べる", h:"たべる", e:"to eat"}
```

- `w` — the written form (kanji + kana mix, kana only, or katakana)
- `h` — the full hiragana reading
- `e` — the English meaning

One canonical reading per word. A runtime de-dupe keeps only the first occurrence of each `w`, so you can append corrections at the bottom of any `pushWords(...)` block without worrying about overwriting earlier entries.

The dataset is split into 5 JLPT-level chunks:

```js
pushWords(5, /* N5 → easiest */ {w:"私",h:"わたし",e:"I; me"}, ...);
pushWords(4, /* N4 */ ...);
pushWords(3, /* N3 */ ...);
pushWords(2, /* N2 */ ...);
pushWords(1, /* N1 → hardest */ ...);
```

Because chunks are pushed easiest-first and the dedupe keeps the first occurrence, every word ends up tagged with its **easiest** JLPT band, which is exactly what cumulative level filtering needs. To re-classify a word, move its entry into a different `pushWords(...)` block. To swap a meaning or reading, find the entry and edit the `e:` or `h:` value.

**Note on dataset size:** Currently ~500 words across 5 levels (~100 per level). Real JLPT word lists range from ~800 (N5) to 8,500+ (N1) words. The format is designed for easy expansion — drop additional entries into the appropriate `pushWords(...)` block.

## Hosting

This is a single static HTML file, so any static host works with zero config:

- **Vercel** — connect the repo, accept the defaults. Vercel serves `index.html` from `/` automatically.
- **GitHub Pages** — enable Pages in the repo settings (Source: `main` / `(root)`). The game will be live at `https://<your-user>.github.io/kanji-knight/`.
- **Netlify / Cloudflare Pages** — same drill, point at the repo, no build command, no publish directory override needed.

## License

MIT
