# DOM of the Haze

A co-op zombie-bunker escape game where **the browser's own code is the puzzle**. You win by opening DevTools and editing the page, while a partner on a second tab or laptop keeps the reactor alive.

**Play it live:** https://dom-of-the-haze.vercel.app/

---

## The story

Day 11, Quarantine Station 9. A purple haze has rewritten the bunker's mainframe and the dead are crawling through its markup. You are Mara Voss, the last engineer on shift. Clean the code and reach the helicopter.

## How to play

You need **two stations**: **Radar & Defense** and **Power & Scrubbers**. Open the game in two tabs, or use **MULTIPLAYER** to play on two laptops.

1. Press **F12** to open DevTools (Chrome calls the tab *Elements*, Firefox calls it *Inspector*).
2. **Radar player:** find the red sector in the code and change its `data-status` to `sterilized`, or delete its `<zombie-node>` children. Never delete `#main-power-grid`.
3. **Power player:** press **PUMP** to keep the reactor above 30%, watch the heat, and press **VENT COOLANT** when it gets high.
4. Follow the **Objective** panel through the 5 stages.
5. Keep **containment** above 0% or it's a meltdown.

The first sector you clean is a training step with no timer.

### The 5 stages

| # | Stage | What you do |
|---|-------|-------------|
| 1 | Armory | Find `#keycard` in the DOM, read its `data-code`, enter it on the keypad |
| 2 | Gate override | Run `bunker.help()` in the Console, find the token in the `gate-token` meta tag, run `bunker.override("TOKEN")` |
| 3 | Power the gate | Hold the reactor at 80% or more for 8 seconds (Power tab) |
| 4 | Clear the hall | Both players press **PURPLE PURGE** within 3 seconds of each other |
| 5 | Final stand | Hold the gate for 90 seconds with containment above 40% |

## Features

**C1: Plot Twist (DOM mutation puzzles)**
- The game reads the live DOM every tick, so DevTools edits change the game.
- Sectors, keycard and gate token all live in attributes and tags you have to find and edit.
- Locked sectors (set `data-locked` to `false`), trapped survivors (+12 containment if saved in time), and a `MutationObserver` guarding the main power grid.

**C2: Make It Come Alive (multi-tab and multi-device sync)**
- Two tabs sync over `BroadcastChannel`.
- **Multiplayer** links two laptops with a 4-letter room code over WebRTC (via PeerJS), with a manual copy/paste code as a backup.
- Reactor below 30% blacks out the Radar screen.
- Dragging the **Purifier Beacon** off the Power screen fires a laser into the Radar screen.
- **Purple Purge** needs both players within a 3-second window.
- Both laptops press **BEGIN MISSION** and start together.

**C3: Chaos Card (temporal logic)**
- **Entropy** climbs over 90 seconds and glitches the screen harder; at 100% the timeline collapses (3 outbreaks, history wiped).
- **Temporal Scrubber:** drag back through the last minute to preview past states, then **COMMIT REWIND** to restore that moment (+25% entropy, 30s cooldown).

**Extras**
- **Best escape time:** your fastest win is saved and shown on the main menu, the top bar and the briefing. Beat it next run.
- Procedural audio (Web Audio, no sound files), CRT grain, fog and glitch effects.

## Multiplayer (two laptops)

1. Both laptops open the game and wait for the intro to finish.
2. **Laptop 1:** press **MULTIPLAYER**, then **CREATE ROOM**. It shows a 4-letter code and plays Radar.
3. **Laptop 2:** press **MULTIPLAYER**, type the code, press **JOIN ROOM**. It plays Power.

Both laptops need internet. If the room code doesn't connect (some college Wi-Fi blocks it), put both laptops on the same phone hotspot, or use the **Manual link** option in the same panel.

## Run locally

No build step and no dependencies to install. Clone the repo and open `index.html` in a browser.

```bash
git clone <your-repo-url>
cd <your-repo-folder>
# open index.html in your browser
```

To deploy, any static host works. On Vercel, keep the file named `index.html` in the repo root and use the "Other" framework preset with no build command.

## Tech

- Plain HTML, CSS and JavaScript in a single file
- `MutationObserver`, `BroadcastChannel`, `localStorage`
- Web Audio API (procedural sound)
- WebRTC data channels via [PeerJS](https://peerjs.com/) for two-laptop play

## Notes

- The best time is stored in `localStorage`, so it is per browser. To reset it, run `localStorage.removeItem('haze_best_time')` in the Console.
- Best played on a desktop browser, since the game relies on DevTools.
