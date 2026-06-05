
# What's New in F1 Race Engineer — June 2026 Update

**Release date:** June 5, 2026
**Affected version:** v4.10+

This document summarizes the features and changes added for the F1 25 — 2026
Season Pack DLC released on June 3, 2026. The "classic" Setup Guide PDF does
not include these yet, so we've collected them here separately.

---

## 🆕 F1 25 — 2026 Season Pack DLC support

### Automatic UDP format detection

The program **supports both formats simultaneously**, no mode selection needed:

- **F1 25 base (UDP Format: 2025)** — 22 cars, classic behavior
- **2026 DLC (UDP Format: 2026)** — 24 cars (Audi + Cadillac), new MOM/Aero data

The program automatically detects the format from each packet's header
(`m_packetFormat`) and uses the matching parameters. **Nothing to configure**:
just set the UDP format in the game and everything works.

### New track: Madring

The new Madrid street circuit (2026) is supported (track_id=42). The estimated
100% lap time is 85 seconds — refined by your own measured lap time during a
session. The track map is learned on lap one via the Map feature, or loaded
from the backend's cached file if available.

---

## ⚡ Overtake / Boost (MOM) — new Dashboard visualization

Under the 2026 regulations, DRS has been replaced by **Overtake Mode (MOM)**.
New visual elements help race-day decisions.

### Top floating detection-line bar

A new floating bar appears at the top of the Dashboard — **only when it
matters**:

- **When does it appear?** When the next detection line is within 800 meters,
  AND at least one car is within 2 seconds of you (ahead or behind)
- **What does it look like?** Two-sided filling bars (left: behind you, right:
  ahead of you), with a purple "DETECTION POINT" pillar in the middle. As you
  approach the line, the two bars fill inwards and meet in the middle (= we're
  at the detection line)
- **Colors:**
  - **Pale orange**: that side's car is within 1-2 seconds
  - **Dark red pulsing** (left): behind you < 1 sec → they'll get Overtake
    next lap if we cross now
  - **Blue pulsing** (right): ahead of you < 1 sec → **you'll get** Overtake

### Yellow ⚡ bolt — acute MOM attack warning

A large yellow bolt pulses on the left side **only when** the car behind you
mounts a **real MOM attack**. Four conditions at once:

1. The car behind is within 1 second
2. They earned Overtake rights at the detection line (eligible for this lap)
3. They are **actively pressing** the MOM button
4. They are running in Boost ERS mode (`m_ersDeployMode == 3`)

The four-way filter matters because the AI presses ERS frequently, but a
**real MOM attack is rare** — the bolt only fires when there's actual danger.

### Left/right arcs during racing

Same arcs as the 2025 DRS — but in 2026 mode the right side is **blue
instead of green** (Overtake color convention):

- **Left red arc**: behind < 1 sec (classic "watch out, coming close")
- **Right blue arc**: ahead < 1 sec (classic "attack opportunity")
- **Thicker pulsing** variant: within 0.5 sec (critical)

---

## 🎙️ Hungarian TTS driver name pronunciation

The voice engineer pronounces driver names in **Hungarian phonetics**.
Examples:

| Original | Hungarian phonetic |
|---|---|
| Norris | **Norrisz** |
| Verstappen | **Versztappen** |
| Hamilton | **Hemilton** |
| Leclerc | **Lökler** |
| Sainz | **Szánc** |
| Russell | **Rasszel** |
| Piastri | **Piasztri** |
| Alonso | **Alonzó** |
| Tsunoda | **Cunoda** |
| Magnussen | **Magnüsszen** |
| Bearman | **Bermen** |
| Doohan | **Doán** |
| Lawson | **Lóvszon** |
| Hadjar | **Hadzsár** |

37 drivers are included (current grid + classic drivers for Career mode).
Unknown names (e.g. a league member's Steam username) are spoken as-is.

**Active only in Hungarian** — in English mode the TTS uses native English
pronunciation as before.

---

## 🏆 End-of-race summary

When you cross the finish line, the engineer announces your result and the
podium:

> *"Vége a versenynek. 7. lettél. A versenyt Norrisz nyerte, második Versztappen,
> harmadik Hemilton."*
>
> *("Race is over. You finished 7th. Norris won, second Verstappen, third
> Hamilton.")*

This only fires **in races** (sprint/race), not in time trial or qualifying.
The trigger is F1 25's `m_resultStatus = 3` (finished) — meaning the race
has physically ended for your own car.

---

## ⏱️ Time Trial cleanup

In Time Trial sessions, the engineer previously emitted race-specific
messages incorrectly (e.g. "Last lap, bring it home", "Tires overheating,
cool them down", "Automatic strategy mode"). All these have been removed.

In Time Trial, **only the intro plays** now:

> *"Radio clear. Welcome to Time Trial. Track: Madring."*

No further engineer messages — the driver can focus on hot laps.

---

## 🔧 Wheel-spin coaching refinement

The "Too aggressive on acceleration, the rear is spinning!" message:

- **New threshold**: 5.0 (was 2.5) — standing-start and normal corner-exit
  acceleration no longer trigger it
- **60-second cooldown** — at most once per minute
- **Race-only** — in time trial the driver consciously seeks the limit and
  doesn't want engineer chatter

Philosophical change: this is now an **aggregated** warning ("you've been
spinning a lot lately"), not a real-time alert. Instant wheel spin is felt
by the driver in the wheel — the engineer's voice arrives more gently.

---

## 🪟 Electron window starts maximized

When the program launches, the main window **automatically maximizes**
(fills to the Windows taskbar). The custom titlebar's minimize/maximize/close
buttons still work as before.

---

## ☕ "Buy Me a Coffee" button on the main menu

The support button is now also in the **main menu header**, between the
"📖 Manual" link and the clock. Same yellow button, same URL:
**buymeacoffee.com/zeropointrace**

All three places (main menu + dashboard + license page) now have the button
consistently.

---

## 🧹 Removed non-functional buttons

Two non-functional buttons removed from the Dashboard header:
- "Rádió Teszt" (Radio Test)
- "Időjárás Kérés" (Weather Request)

Remaining: "← MENU", "Logging: OFF", "🗺️ Map", "Help with Development",
"☕ Buy Me a Coffee".

---

## 🔍 Diagnostics — quick identification of new tracks

If an unknown `track_id` arrives from the game (e.g. a future F1 25 DLC track),
the program writes a WARNING **once per session** to the log file:

```
WARNING - Ismeretlen track_id=33 (packet_format=2026).
Bővítendő a config.F1_TRACKS mapping.
```

(translation: "Unknown track_id=N, needs to be added to config.F1_TRACKS.")

From this number we can quickly identify the new track and add it to the
next update.

---

## 🙏 Thanks

Thanks for the integrated features and the continuous feedback! If you spot
anything odd in the new features, please let us know — we'll fix it.

- **Feedback**: "Help with Development" button on the Dashboard
- **Email**: f1zeropointracing@gmail.com
- **Support**: [buymeacoffee.com/zeropointrace](https://buymeacoffee.com/zeropointrace)

---

**F1 Race Engineer © 2026 ZeroPoint Racing**
