# Studio Scheduler

A single-file web app that solves competitive dance studio scheduling in seconds — no backend, no install, no spreadsheet.

---

## The Problem

Competitive dance studios routinely manage 200+ groups, dozens of instructors, and multiple rooms across a full week of rehearsals, classes, and performances. Scheduling all of this by hand — while avoiding dancer conflicts, instructor double-bookings, room collisions, and standing class overlaps — can take weeks of iteration per season.

This app was built for exactly that situation. A working schedule for a full studio week can be generated in under a minute.

---

## How It Works

The scheduler runs a **local constraint solver** entirely in the browser — no server, no API calls. The core algorithm is a pure JavaScript bin-packing engine with backtracking that:

- Builds a **conflict map** across all groups by identifying shared dancers
- Sorts groups by conflict density before placement (most constrained first)
- Assigns each group to a room/time slot while checking:
  - Dancer conflicts (no dancer in two places at once)
  - Instructor conflicts (no instructor double-booked)
  - Room conflicts (no room double-booked)
  - Standing class blocks (pre-existing classes are treated as hard constraints)
  - Custom instructor-room bans (weekend constraints, etc.)
- Backtracks and retries on constraint violations
- Sanitizes the final output and surfaces any unresolvable conflicts explicitly

---

## Features

- **CSV import** — load dancers, groups, rooms, instructors, and classes from spreadsheets exported from any studio management tool
- **Conflict matrix** — visual map of which groups share dancers, used to prioritize placement order
- **Free time analysis** — per-room and per-instructor availability breakdown
- **Instructor load balancing** — view and distribute teaching load across the schedule
- **Student schedule view** — per-dancer schedule lookup
- **Undo stack** — full history with snapshot-based rollback
- **Standing class protection** — pre-existing classes treated as hard blocked times
- **Conflict report** — any violations surfaced with dancer names, times, and reason
- **Single file** — the entire app is one `.html` file; open it in any browser, no install needed

---

## Status

**v1.0.0 — Active stress testing**

The app is currently in production use at a competitive dance studio. It is being evaluated under real scheduling load across a full season. Core scheduling logic is stable; UI refinements and edge case handling are ongoing.

---

## Usage

1. Download `Studio_Scheduler.html`
2. Open it in any modern browser (Chrome recommended)
3. Import your studio data via CSV or enter it manually
4. Set scheduling parameters (days, hours, session length)
5. Click **Generate Schedule**

No internet connection required after the page loads.

---

## Built With

- Vanilla JavaScript (no frameworks)
- Pure CSS (no libraries)
- Local constraint solver — bin-packing with backtracking
- CSV parsing — built-in, no dependencies

---

## Background

Built in collaboration with [Claude](https://claude.ai) over approximately 24 hours to solve a real scheduling problem. The goal was to deliver a working tool fast — and to demonstrate that logic-heavy domain problems (scheduling, constraint satisfaction) are well-suited to rapid AI-assisted development when you understand the underlying problem structure.

---

*Part of [bmadero.github.io](https://bmadero.github.io)*
