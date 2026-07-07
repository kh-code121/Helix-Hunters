# Helix Hunters 🧪

**Survive the regulatory landscape** — a browser-based top-down survival shooter built as a
conference marketing game for [Helix](https://helix.yordasgroup.com), the regulatory compliance
platform by Yordas Group.

Fight off swarming hazardous chemicals (SVHCs, PFAS, endocrine disruptors…) — each one wearing a
GHS-style pictogram and label so you know exactly what's attacking — collect Helix power-ups, and
survive as long as you can. When your **Compliance Meter** hits zero, you are consumed by ECHA.

The UI uses the IBM Carbon (dark theme) colour system to match Helix.

## Running the game

The whole game is one self-contained file: **`index.html`**. No build step, no server, no internet
connection required.

- **Booth laptop:** double-click `index.html` (or drag it into Chrome/Edge/Firefox) and press `F11`
  for fullscreen kiosk mode.
- **Hosted:** serve the file from any static host (GitHub Pages, S3, …) — it works identically.

Desktop with a mouse and keyboard is required (movement is WASD, aiming is mouse).

## How to play

| Action | Control |
| --- | --- |
| Move | `W` `A` `S` `D` |
| Aim | Mouse |
| Shoot | **Hold** left mouse button |
| Pause / exit & bank score | `Esc` |
| Mute / unmute | `M` |

Score = survival time (10 pts/second) + a bonus for every chemical neutralised. Waves escalate
forever — the run ends when you die or when you exit voluntarily and bank your score.

Power-ups drop frequently (first one after ~6 s, then every 6–10 s, two at a time from wave 4).
Collecting one pauses the action and shows what the app does, with a **Continue Hunting** button
(Space/Enter/Esc also continue).

### Power-ups (real Helix apps)

| App | Effect |
| --- | --- |
| **Helix Connect** | A buddy consultant orbits you and auto-fires at hazards |
| **Knowledge Hub** | 3-hit protective shield |
| **Substance Inventory** | Fire rate doubled |
| **Compliance Navigator** | Damage doubled |
| **SDS Manager** | Restores the Compliance Meter |

## Leaderboard & lead capture

After every run the player can save their score with a **username** (shown on the leaderboard),
plus an optional **email** and **company**. An email is only stored together with a ticked
GDPR-style consent checkbox. The leaderboard displays **usernames only** — contact details never
appear on screen.

All data lives in the browser's `localStorage` **on that device only** — nothing is transmitted
anywhere. This means the leaderboard is per-machine (ideal for a single booth laptop) and survives
browser restarts, but not a browser-profile wipe.

### Booth-staff shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl` + `Shift` + `E` | Download all captured sign-ups (username, email, company, consent, score…) as `helix-hunters-signups.csv` |
| `Ctrl` + `Shift` + `Backspace` | Erase all local scores and sign-ups (asks for confirmation) |

Export the CSV at the end of each conference day. The file opens directly in Excel.

## Configuration

All the knobs live in the `CONFIG` object at the top of the `<script>` block in `index.html`:

- `HELIX_SIGNUP_URL` — where the end-screen "Sign up / Log in to Helix" button points.
- `CONSENT_TEXT` — the consent checkbox wording.
- `STORAGE_NOTE` — the privacy note under the sign-up form.
- `BOARD_SHOW` — number of leaderboard rows displayed.
- `SCORE_PER_SEC`, `KILL_SCORE_BASE`, `KILL_SCORE_HP` — scoring balance.

Game balance (wave sizes, enemy stats, power-up timings) is in `spawnWave` / `spawnEnemy` /
`spawnPickup` nearby.

## Credits

Based on the internal pygame prototype *Chem Survivors*; power-up copy from the Helix Hunters
concept deck. All art (Helix app icons, scientist, chemicals) is drawn in code — the file has no
external assets.
