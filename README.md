# leaderboard 🏆

> **Public repository — `BreachToPatch` organization**
> The scoreboard. A single JSON file, automatically updated by GitHub Actions every time a player earns points.

---

## What this repo is

`leaderboard` is the **scoring engine** of BreachToPatch. It contains one file — `leaderboard.json` — which tracks every player's points, pwns, patches, and badges.

This file is updated **automatically** by a GitHub Actions workflow every time a PR is merged in `machines-public`. No human needs to touch it.

The website (`web-site`) reads this file directly via GitHub's raw content URL and displays the leaderboard in real time.

---

## What lives here

```
leaderboard/
├── README.md           ← You are here
└── leaderboard.json    ← The single source of truth for all scores
```

---

## The leaderboard.json format

```json
{
  "last_updated": "2026-04-10T12:00:00Z",
  "players": [
    {
      "github_username": "alice",
      "points": 175,
      "pwns": 2,
      "patches": 1,
      "machines_created": 0,
      "badges": ["First Blood", "Scribe"],
      "github_profile": "https://github.com/alice"
    }
  ]
}
```

| Field | Description |
|-------|------------|
| `github_username` | The player's GitHub username — their unique identifier |
| `points` | Total score (see scoring table below) |
| `pwns` | Number of validated exploit submissions |
| `patches` | Number of validated patch submissions |
| `machines_created` | Number of machines they authored |
| `badges` | List of earned badges |
| `github_profile` | Direct link to their GitHub profile |

---

## Scoring system

| Action | Points |
|--------|--------|
| First Blood (first pwn on a machine) | 100 pts |
| Subsequent pwn | 50 pts |
| Patch validated | 75 pts |
| Machine created and accepted | 150 pts |
| Writeup quality bonus (maintainer-awarded) | up to 25 pts |

---

## Badges

| Badge | How to earn it |
|-------|---------------|
| 🩸 **First Blood** | First player to pwn a machine |
| ✍️ **Scribe** | Submit 5 writeups |
| 🛡️ **Defender** | Get a patch validated |
| 🏰 **Fort Knox** | Your patch resists 3 consecutive re-pwn attempts |
| ⚙️ **Creator** | Get a machine you built accepted on the platform |
| 🐝 **Queen Bee** | Top of the leaderboard for 30 consecutive days |

---

## How the auto-update works

Every time a PR is **merged** in `machines-public`, a GitHub Actions workflow fires:

```
PR merged in machines-public
        │
        ▼
Workflow reads the PR labels
(e.g., "pwn:validated", "patch:validated", "first-blood")
        │
        ▼
Python script calculates new points and badges
        │
        ▼
leaderboard.json is updated and committed automatically
        │
        ▼
web-site picks up the new JSON within seconds (no deploy needed)
```

The workflow file lives at `.github/workflows/update-leaderboard.yml` in `machines-public`.

---

## How to read the leaderboard

**Directly (raw JSON):**
```
https://raw.githubusercontent.com/BreachToPatch/leaderboard/main/leaderboard.json
```

**On the website:**
The `web-site` repo reads this URL and renders it as a live leaderboard page.

---

## How this repo interacts with other repos

```
machines-public (BreachToPatch, public)
   └── Every merged PR triggers a GitHub Actions workflow
   └── That workflow commits an updated leaderboard.json here

leaderboard (this repo)
   └── leaderboard.json is the single source of truth

web-site (BreachToPatch, public)
   └── Fetches leaderboard.json via raw GitHub URL
   └── Renders it as a live scoreboard on the website
```
