# STATLINE

One NBA player a day. Three guesses. Identify them from their career stat line.

A daily-puzzle game in the spirit of Wordle / Weddle, built around the
Basketball Reference per-game stat table. 40 players in the rotation —
roughly six weeks of unique daily play before any repeat.

---

## Three ways to ship this

### Path A — Fastest (no install, ~5 min)

Best for: just send the URL to the group chat tonight.

1. Go to **https://stackblitz.com/fork/vite-react** (forks a Vite + React starter you can edit in the browser).
2. In `package.json`, add `"lucide-react": "^0.383.0"` to `dependencies`. StackBlitz will auto-install.
3. Replace the contents of `src/App.jsx` with the contents of `src/StatLine.jsx` from this folder.
4. Open `index.html` and add `<script src="https://cdn.tailwindcss.com"></script>` inside the `<head>`.
5. Copy the URL StackBlitz gives you (top of the editor) and share it.

Caveat: StackBlitz URLs work but look like `stackblitz.com/edit/xyz`. Fine for the group chat, less great for bragging. Persistence works (uses `localStorage`).

### Path B — Vercel CLI (~15 min, real URL)

Best for: an actual `statline-yourname.vercel.app` link.

```bash
# 1. Install Node 18+ if you don't have it: https://nodejs.org

# 2. From this folder:
npm install

# 3. Test locally:
npm run dev
# → opens http://localhost:5173

# 4. Install Vercel CLI (one time):
npm install -g vercel

# 5. Deploy:
vercel
# → follow prompts. Defaults work. First deploy gives a preview URL;
#   running `vercel --prod` afterward gives you the real one.
```

### Path C — Vercel via GitHub (most "real")

Best for: future tweaks, easy redeploys, sharing the source.

1. Create a GitHub repo, push this folder up.
2. Go to **vercel.com** → "Add New Project" → import the repo.
3. Vercel detects Vite and deploys with default settings.
4. Custom domain available in the project settings if you want
   `statline.example.com` instead of the `.vercel.app` URL.

Pushes to `main` auto-redeploy.

---

## Configuration

### Day numbering

Open `src/StatLine.jsx`, find this line near the top:

```js
const STATLINE_EPOCH = '2026-05-09';
```

That's "Day 1." Change it to your actual launch date so the day counter
in the share string matches when your group started playing.

### Editing player stats

If a friend catches a bad stat line in the chat, find the player in the
`RAW_PLAYERS` array near the top of `src/StatLine.jsx`. Each season is a
tuple in this order:

```
[year, age, team, games, MPG, PPG, RPG, APG, FG%, 3P%, FT%]
```

Use `null` for 3P% in pre-1979 seasons. Edit, redeploy.

### Adding more players

Same `RAW_PLAYERS` array, same shape. Each entry needs:

- `id` (unique short string)
- `name`
- `position` (`'PG'` / `'SG'` / `'SF'` / `'PF'` / `'C'`)
- `era` (string like `'1996–2016'`)
- `seasons` (array of tuples)
- `clues` (array of 3 strings — revealed in order on wrong guesses)

---

## How it works

- **Same player for everyone, every day.** A hash of the calendar date
  picks the player from the roster. No server needed; everyone running
  the same code on the same day gets the same dossier.
- **3 guesses, progressive clue reveals.** Each wrong guess unlocks one
  more "intel" line. After all 3 attempts (or a correct guess), the game
  is over for the day.
- **Wordle-style sharing.** The share button generates `STATLINE Day N — X/3`
  with an emoji grid (🟩 correct, 🟥 wrong, ⬛ unused). Uses the native
  share sheet on mobile, falls back to clipboard on desktop.
- **Persistence.** Daily progress + lifetime score are saved to
  `localStorage`. Refreshing the page after winning or losing won't let
  you replay today's puzzle.

---

## Stack

- React 18
- Vite
- Tailwind CSS (via Play CDN — fine for a side project; convert to a
  build-time install if you ever want to optimize bundle size)
- lucide-react (icons)

No backend. No analytics. No tracking.
