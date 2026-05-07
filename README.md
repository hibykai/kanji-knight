# Kanji Knight

A single-file browser game that drills the most-common Jōyō kanji. A knight stands on the left of a 2D arena while a hungry T-Rex slowly walks in from the right. Read the kanji before he eats you.

![Kanji Knight gameplay screenshot placeholder](docs/screenshot.png)

## How to play

1. Open `kanji-game.html` in any modern browser (double-click works — no server needed).
2. On the start screen, choose whether to display readings as **Hiragana** or **Romaji**.
3. Each round:
   - You get **5 seconds** to study a kanji.
   - Then 3 cards appear with possible readings — only one is correct.
   - There's no time limit on your choice, but the T-Rex is walking toward the knight.
4. **Wrong answer:** a red ✕ appears on the card you picked, and the T-Rex speeds up to 1.5×.
5. **Right answer:** the T-Rex stops, score goes up, next kanji.
6. Crucially, the T-Rex **does not reset between rounds** — the position carries forward. At base speed he reaches the knight in 30 seconds total, so quick correct answers = higher score before he gets you.
7. Game over when he reaches the knight. Your score is the number of kanji you read correctly.

## What's inside

- Single self-contained `kanji-game.html` (~70 KB) — HTML, CSS, JS, SVG sprites, and ~2,000 Jōyō kanji entries all inline.
- No build step, no dependencies, no fetches — runs from `file://`.
- Hiragana → Romaji conversion done at runtime via a Hepburn-style lookup table, so the dataset only stores hiragana.
- Distractors are auto-picked from the rest of the dataset, preferring readings of the same length or that share characters with the correct answer.

## Editing the kanji dataset

Each entry in the file looks like:

```js
{k:"漢", h:"かん"}
```

One canonical reading per kanji (most-common kun-yomi where it exists, otherwise on-yomi). A runtime de-dupe keeps only the first occurrence of each kanji, so you can append corrections at the bottom of any `KANJI_DATA.push(...)` block without worrying about overwriting earlier entries.

To swap a reading, find the entry and edit the `h:` value. To add new kanji, append to one of the `KANJI_DATA.push(...)` blocks.

## Hosting on GitHub Pages

Rename `kanji-game.html` to `index.html`, push, then enable Pages in the repo settings (Source: `main` / `(root)`). The game will be live at `https://<your-user>.github.io/kanji-knight/`.

## License

MIT
