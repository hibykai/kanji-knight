# Kanji Arcade

A small library of browser games for drilling JLPT-level Japanese. No build step, no dependencies, no server required — open `index.html` and play.

## Play it

Live at **https://kanji-knight.vercel.app/** (the deployed URL is still the old project name; the game library opens there), or open `index.html` locally — double-click works.

## The games

### 🗡️ Kanji Knight — `kanji-knight.html`

A side-view arena where a knight stands on the left and a T-Rex slowly advances from the right. Read the JLPT word on the study card, then pick the matching answer (reading + English meaning shown on every card) before the T-Rex eats you. Correct answers buy you breathing room; wrong ones make him sprint.

- **Quiz pool:** ~500 JLPT-tagged words (N5–N1, cumulative) — kanji + kana mix.
- **Configurable:** study time per word (1–15s), T-Rex time-to-knight (5–120s), JLPT difficulty.
- **Hot-streak power-ups:** Blade Attack, Time Freeze, 50/50, Shield, Skip. Streak threshold scales with your difficulty settings (every 3 correct on hardest, every 8 on easiest).

### 🗼 Tower Rush — `tower-rush.html`

Top-down-ish tower defense. A tower at the right edge of the arena fires arrows automatically at monsters walking the path from the left. Wrong answers buff the monsters, right ones earn power-ups to fight back. Every 30 seconds the wave automatically gets tougher.

- **Quiz pool:** ~500 JLPT-tagged words (same shared dataset as Kanji Knight) — kanji + kana mix, with reading and English meaning on every answer card.
- **Configurable:** tower starting HP (10–50), JLPT difficulty, Hiragana / Romaji.
- **Power-ups earned every 3 correct:** 🏹 Arrows (volley everyone), 💣 Bomb (AoE), 🔥 Firewall (block + burn), ⚡ Lightning (instakill lead), 🩹 Repair (+5 tower HP).
- **Wrong answer = monster boost (auto-applied):** 🛡️ Lead-monster armor (+2 HP), 💨 Speed boost for 5s, 👹 Boss spawn, 📈 Two extra spawns.
- **Wave scaling:** every 30s monsters gain +1 HP and +10% speed; spawn rate accelerates. Bosses start showing up at wave 3+.

## Project layout

```
index.html         Library landing page (game card grid)
kanji-knight.html  Kanji Knight (T-Rex chase, words quiz)
tower-rush.html    Tower Rush (tower defense, words quiz)
words-data.js      Shared ~500-entry JLPT words dataset (active)
kanji-data.js      Shared ~2,000-entry single-kanji dataset (reference; not loaded by either game)
README.md
```

Both games `<script src="words-data.js">` the shared words file — single source of truth, fix a reading once and both games benefit. `kanji-data.js` is kept as a reference dataset (and as a fallback if you ever want to swap a game back to single-kanji drills). To add a new game, drop in another `.html` file, load the data file(s) you need, and link it from `index.html`.

## Datasets

### Words (active, shared) — `words-data.js`

Each entry: `{w: "食べる", h: "たべる", e: "to eat", j: 5}`.

- `w` — the written form (kanji + kana mix, kana only, or katakana)
- `h` — the full hiragana reading
- `e` — the English meaning
- `j` — JLPT level (5 easiest, 1 hardest)

Currently ~500 words across 5 JLPT levels. Real JLPT lists are 800+ (N5) to 8,500+ (N1) — easy to grow by appending to any `pushWords(level, ...)` block. Pushed easiest-first; runtime de-dupe keeps each word at its easiest level. Edits flow to both games automatically.

### Kanji (reference) — `kanji-data.js`

Each entry: `{k: "漢", h: "かん", j: 5}`. Same `pushChunk(level, ...entries)` pattern, ~2,000 entries from the Jōyō list. Not loaded by either game right now. Available if you ever want to swap a game back to single-kanji drills — change the `<script src>` and update `activePool()` to filter `KANJI_DATA` instead of `WORDS_DATA`.

## Hosting

Static files, so any static host works with zero config:

- **Vercel** — connect the repo, accept the defaults. Vercel serves `index.html` from `/` automatically. `kanji-data.js` is fetched as a normal sibling asset.
- **GitHub Pages** — enable Pages (Source: `main` / `(root)`). Live at `https://<your-user>.github.io/kanji-knight/`.
- **Netlify / Cloudflare Pages** — point at the repo, no build command, no publish directory override needed.

## License

MIT
