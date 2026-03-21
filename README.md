# Studio Scheduler

![Studio Scheduler Dashboard](screenshot.png)

**A scheduling tool built for a real user, solving a real problem — in 24 hours.**

A single-file web app that eliminates weeks of manual scheduling for competitive dance studios. No backend, no install, no spreadsheet juggling.

---

## The Problem

Every season, the Ballet Director at a competitive dance studio faces the same impossible task: scheduling 200+ groups across multiple rooms and instructors, across a full week of rehearsals, classes, and performances — entirely by hand.

The constraints are brutal:
- No dancer can be in two places at once
- No instructor can teach two groups simultaneously
- No room can be double-booked
- Standing classes must be protected as hard blocks
- Competition groups need priority placement
- The whole thing has to fit within studio hours

Getting it right takes weeks of iteration. Getting it wrong means calling dancers, reshuffling instructors, and starting over.

---

## The Goal

Build something that:
1. Respects how the studio already works (CSV exports from existing tools)
2. Requires zero technical setup from the user
3. Generates a conflict-free schedule in seconds
4. Surfaces any problems it can't solve clearly, with enough detail to fix them manually

---

## Design Decisions

**Single HTML file, no install.** The user is a dance director, not a developer. Asking her to install Node, run a server, or manage dependencies is a non-starter. The entire app — UI, logic, solver — lives in one file she can open in any browser.

**CSV import, not manual entry.** She already has her roster data in spreadsheets. Rebuilding it in a new format would create friction and errors. The app parses her existing files directly, with flexible column name matching so it works even if the headers aren't exact.

**Conflict matrix before scheduling.** Before the solver runs, users see a visual map of which groups share dancers. This surfaces the hardest constraints upfront and sets accurate expectations — if 40 groups share dancers with 30 others, no scheduler will make everyone happy.

**Conflict report, not silent failure.** When conflicts can't be resolved, the app doesn't hide them or crash. It names the specific dancers, groups, times, and reasons — so the director knows exactly what needs a manual decision.

**Undo stack.** Scheduling is iterative. The app supports full history rollback so users can experiment, see what breaks, and step back without starting over.

---

## How the Solver Works

The scheduler runs a local constraint solver entirely in the browser — no server, no API calls. The core algorithm is a pure JavaScript bin-packing engine with backtracking:

- Builds a **conflict map** across all groups by identifying shared dancers
- Sorts groups by conflict density before placement (most constrained first)
- Assigns each group to a room/time slot while enforcing:
  - Dancer conflicts (no dancer in two places at once)
  - Instructor conflicts (no instructor double-booked)
  - Room conflicts (no room double-booked)
  - Standing class blocks (pre-existing classes as hard constraints)
  - Custom instructor-room bans (weekend constraints, etc.)
- Backtracks and retries on constraint violations
- Sanitizes output and surfaces unresolvable conflicts with full detail

---

## Features

- **CSV import** — dancers, groups, rooms, instructors, and classes from any spreadsheet
- **Conflict matrix** — visual map of shared-dancer conflicts across all groups
- **Free time analysis** — per-room and per-instructor availability breakdown
- **Instructor load balancing** — view and distribute teaching hours across the schedule
- **Student schedule view** — per-dancer lookup across the full week
- **Undo stack** — full snapshot-based history with rollback
- **Standing class protection** — pre-existing classes block scheduling slots
- **Conflict report** — violations surfaced with dancer names, times, and reason
- **Single file** — entire app in one `.html` file; open in any browser, no install

---

## Outcome

**Before:** Weeks of manual iteration per season, high error rate, constant reshuffling.  
**After:** Conflict-free draft schedule generated in under 60 seconds.

The app is currently in production use at a competitive dance studio, under active stress testing across a full season.

---

## Usage

1. Download `Studio_Scheduler.html`
2. Open in any modern browser (Chrome recommended)
3. Import studio data via CSV or enter manually
4. Set scheduling parameters (days, hours, session length)
5. Click **Generate Schedule**

No internet connection required after the page loads.

---

## Built With

- Vanilla JavaScript — no frameworks
- Pure CSS — no libraries
- Local constraint solver — bin-packing with backtracking
- CSV parsing — built-in, no dependencies

---

## Background

Built in collaboration with [Claude](https://claude.ai) over approximately 24 hours. The constraint logic and UX decisions were driven by direct domain knowledge of how competitive dance studios actually operate — the fast delivery was possible because the problem was already well-understood before a line of code was written.

---

*Part of [bmadero.github.io](https://bmadero.github.io)*
