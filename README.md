# Helix Hunters 🧪

**Survive the regulatory landscape** — a phone-first top-down survival shooter built as a
conference marketing game for [Helix](https://helix.yordasgroup.com), the regulatory compliance
platform by Yordas-3E.

Fight off swarming chemicals in a laboratory maze. Every chemical is **colour-coded** — red is
hazardous, green is harmless, and later on purple and orange *unknowns* appear that could be either.
Grab power-ups for a few seconds of help (and **Insight** into which chemicals are really dangerous),
file your compliance documents before the deadline, brace for new regulations, and survive as long
as you can. When your **Compliance Meter** hits zero, you are consumed by ECHA.

The UI uses the **3E brand palette** (sampled from 3eco.com): deep navy `#133151` primary with
gold `#e8c76f`, terracotta `#c78d6e` and sky-blue `#6ab5d4` accents. It lives in one place — the
`:root` block at the top of the `<style>` in `index.html` (with a matching `C` object in the
`<script>` for the canvas) — so the whole game re-themes from there.

## Running the game

The whole game is one self-contained file: **`index.html`**. No build step, no server, no internet
connection required.

- **Phones (primary):** host the file on any static host (GitHub Pages, S3, …) and share the link
  or a QR code at the booth. It works in the phone browser as-is; "Add to Home Screen" gives a
  full-screen, app-like experience. Portrait and landscape both work.
- **Booth laptop / tablet:** double-click `index.html` (or drag it into Chrome/Edge/Firefox). Press
  `F11` for fullscreen kiosk mode on a laptop. Keyboard and mouse still work.

## How to play

| Action | Phone (touch) | Desktop |
| --- | --- | --- |
| Move | Touch and drag anywhere on the **left half** of the screen | `W` `A` `S` `D` |
| Aim | Touch and drag on the **right half** | Mouse |
| Shoot | Automatic while your right thumb is down | **Hold** left mouse button |
| Pause / exit & bank score | **⏸ button** (top-right) | `Esc` |
| Mute / unmute | Pause menu | `M` or pause menu |

Score = survival time (10 pts/second) + a bonus for every hazard neutralised. Waves escalate
forever — the run ends when you die or when you exit voluntarily and bank your score.

### Chemical colours

Chemicals carry no names — the colour is the only signal, so it reads on a small screen.

| Colour | Meaning |
| --- | --- |
| 🔴 **Red** | Hazardous — touching one drains the Compliance Meter. Shoot them. |
| 🟢 **Green** | Harmless — walk straight through, shots pass through. They disperse on their own after a while. |
| 🟣 🟠 **Purple / orange** | *Unknown* — each one is secretly hazardous or harmless (55 % hazardous by default). They appear from wave 3. |

Collecting **any** power-up grants **Insight** for 12 s: every unknown shows its true red or green
colour (gold bar under the meter, gold frame around the screen). When it runs out, they go back to
purple and orange.

### Power-ups (all temporary — 12 seconds, then gone)

Power-ups drop frequently (first one after ~6 s, then every 6–10 s, two at a time from wave 4). A
small chevron orbiting the player always points to the nearest one. Collecting one pauses the
action and shows what it does. Nothing is permanent any more: each effect lasts
`CONFIG.POWERUP_DURATION` seconds and shows a countdown ring in the HUD. Picking the same one up
again restarts its timer. Names are placeholders — the concept is what matters.

| Power-up | Effect |
| --- | --- |
| **Connect** | The nearest hazards (up to 6) are reclassified harmless and turn green; while it lasts, any hazard that comes within reach is talked down too. |
| **Knowledge Hub** | A buddy consultant orbits you and auto-fires at hazards (a second pickup adds a second buddy). |
| **Substance Inventory** | Fire rate doubled. |
| **Compliance Navigator** | Heals 50. Anything up to 100 % is permanent; anything above (up to 150 %) is *overheal* that reverts to 100 % when the timer ends. |
| **SDS Manager** | Damage doubled. |
| **AI Booster** | "God mode": invincible, walk through walls and chemicals; hazards you touch are neutralised. When it ends you are stepped out onto the nearest floor tile. |

### Regulatory update events

From ~45 s into the run (then every 40–60 s) **ECHA or the EPA publish new regulations**: the game
pauses with a pop-up, and either a batch of new chemicals enters the landscape or every purple and
orange unknown is reclassified **red**.

### Submission deadline mini game

From ~55 s into the run (then every 70–95 s) a **submission deadline** hits. The first time, a pop-up
explains it; after that it just happens. An **ECHA / EPA folder** appears and the player must drag
every compliance document into it (touch or mouse). They cannot move or shoot meanwhile, and the
hazards keep coming (at 70 % speed), so it is all about speed. From the second deadline onwards a
decoy document (lunch menu, cat memes…) is mixed in — filing it costs 50 points.

- **Filed in time (15 s):** +150 points, 1.5 s of invulnerability, and ~45 % of the hazards (at least
  four) are reclassified green for a breather.
- **Deadline missed:** a squad of red enforcement hazards spawns.

## Leaderboard & lead capture

After every run the player can save their score with a **username** (shown on the leaderboard),
plus an optional **email** and **company**. An email is only stored together with a ticked
GDPR-style consent checkbox. The leaderboard displays **usernames only** — contact details never
appear on screen.

All data lives in the browser's `localStorage` **on that device only** — nothing is transmitted
anywhere. On a single booth laptop or tablet that gives one shared leaderboard that survives
browser restarts. **If players use their own phones, each phone has its own private leaderboard
and sign-up list** — for a shared conference leaderboard or central lead capture you would need a
small backend, which this file does not include.

### Booth-staff shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl` + `Shift` + `E` | Download all captured sign-ups (username, email, company, consent, score…) as `helix-hunters-signups.csv` |
| `Ctrl` + `Shift` + `Backspace` | Erase all local scores and sign-ups (asks for confirmation) |
| Tap the menu footer ("Powered by Helix…") **7 times** quickly | Phone/tablet admin menu: type `export` or `wipe` |

Export the CSV at the end of each conference day. The file opens directly in Excel.

## Configuration

All the knobs live in the `CONFIG` object at the top of the `<script>` block in `index.html`:

- `HELIX_SIGNUP_URL` — where the end-screen "Sign up / Log in to Helix" button points.
- `CONSENT_TEXT`, `STORAGE_NOTE` — consent wording and privacy note.
- `BOARD_SHOW` — number of leaderboard rows displayed.
- `SCORE_PER_SEC`, `KILL_SCORE_BASE`, `KILL_SCORE_HP` — scoring balance.
- `POWERUP_DURATION`, `INSIGHT_DURATION` — how long power-ups and Insight last (seconds).
- `UNKNOWN_FROM_WAVE`, `UNKNOWN_HAZARD_CHANCE`, `GREEN_SHARE` — colour mix of the chemicals.
- `REG_FIRST`, `REG_EVERY` — timing of the "new regulations" events.
- `MINI_FIRST`, `MINI_EVERY`, `MINI_TIME_LIMIT` — timing of the document-filing mini game.
- `MOBILE_ZOOM` — camera zoom-out used on phone-sized screens.

The chemical colours are in the `KIND` table (mirrored as `--chem-*` CSS variables for the
tutorial legend). Power-up names, copy and effects are in `POWERUPS`. Game balance (wave sizes,
enemy stats, power-up drop timings) is in `spawnWave` / `spawnEnemy` / `spawnPickup` nearby.

## Credits

Based on the internal pygame prototype *Chem Survivors*; power-up concepts from the Helix Hunters
concept deck. All art (power-up icons, scientist, GHS hazard pictograms, documents and folder) is
drawn in code — the file has no external assets.
