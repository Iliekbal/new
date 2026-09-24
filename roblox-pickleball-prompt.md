# Prompt: Roblox Pickleball Game

Copy everything below the line and give it to another AI.

---

You are an expert Roblox developer. Build a complete, playable **pickleball game** in Roblox Studio using Luau. Give me every script, where it goes in the Explorer (ServerScriptService, ReplicatedStorage, StarterPlayerScripts, StarterGui, Workspace), and step-by-step setup instructions a beginner can follow.

## Core gameplay
- **Court:** Regulation-proportioned pickleball court (20 × 44 ft, scaled to studs, e.g. 1 ft ≈ 1 stud × 1.5). Include baselines, sidelines, centerline, the 7-ft **non-volley zone ("kitchen")** on both sides, and a net 36 in high at the sidelines and 34 in at the center. Build it from Parts in code or give exact Part sizes/positions.
- **Modes:** 1v1 (singles) and 2v2 (doubles). Players join a match by stepping on a pad next to the court; the match starts when enough players are ready.
- **Paddle:** A Tool the player holds. Clicking/tapping swings it. The shot type depends on input:
  - Normal click = drive (fast, low arc)
  - Hold + release = power drive (charge meter shown in the UI)
  - Right-click / second button = lob (high arc)
  - Shift + click near the kitchen = dink (soft, short arc)
- **Ball physics:** Use a custom, server-authoritative ball (no default Roblox physics) that moves along a computed arc with gravity, simple air drag, and a bounce off the ground. The ball should be predictable and fair, and never tunnel through the net or paddle. Aim the shot using the player's facing direction plus the mouse/camera direction.
- **Hit detection:** A hit counts only if the ball is inside a small hitbox in front of the player during the swing window (~0.25 s). Validate hits on the server with distance and timing checks to prevent exploits.

## Rules to enforce
- Underhand serve from behind the baseline, diagonally to the opposite service box.
- **Two-bounce rule:** the serve must bounce once, and the return must bounce once, before anyone can volley.
- **Kitchen rule:** a player may not volley while standing in the non-volley zone. Detect this and award a fault.
- Faults: ball out of bounds, into the net, bouncing twice, a kitchen violation, or a serve landing in the wrong box.
- **Scoring:** Traditional side-out scoring to 11, win by 2. Only the serving side scores. In doubles, track server 1 and server 2 and use the "0-0-2" start. Add a setting to switch to rally scoring.
- Switch serving sides correctly based on score (even = right, odd = left).

## UI (StarterGui)
- A scoreboard showing team names, score, and server number, formatted like the callout "4-2-1".
- A shot-power meter, a message when there's a fault or point ("Kitchen fault!", "Out!", "Point: Blue"), and a win screen.
- A short tutorial popup with the controls and rules, shown on first join.
- Mobile support: on-screen buttons for swing, lob, and dink.

## Polish
- Swing animation (a simple keyframe animation or a CFrame tween on the paddle), "pop" hit sounds, bounce sounds, and a small particle burst on hit.
- A ball trail and a landing-spot indicator for the incoming ball.
- A camera that follows behind the player but keeps the court readable.
- Idle NPC opponent (a simple AI that tracks the ball and returns it with adjustable difficulty) so one player can practice solo.

## Progression (optional, but include it)
- Save wins, losses, and coins with DataStoreService (use pcall and retry logic).
- A simple shop to buy paddle colors and ball trails with coins.
- A leaderboard (leaderstats) showing Wins.

## Technical requirements
- Use a clean ModuleScript architecture: `BallController`, `MatchManager`, `RulesEngine`, `ScoreManager`, `CourtBuilder`, `NPCController`, `DataManager`.
- Use RemoteEvents in a `Remotes` folder in ReplicatedStorage. **Never trust the client**: rate-limit swings and validate everything on the server.
- Replicate the ball smoothly: the server simulates it, and clients interpolate or predict a visual copy so it doesn't look laggy.
- Comment the code, avoid deprecated APIs (use `task.wait`, `task.spawn`, not `wait` or `spawn`), and use strict typing (`--!strict`) where practical.
- Handle edge cases: a player leaving mid-match, the ball getting stuck, and restarting a match.

## Deliverables
1. The full Explorer tree layout.
2. Every script, complete (no placeholders like "add logic here").
3. Setup steps and a testing checklist (e.g. "serve into the wrong box → fault is called").
4. A list of ideas for future features (tournaments, ranked matchmaking, spin shots).

Build it in stages: first the court and ball physics, then hitting, rules, scoring, UI, NPC, and finally progression. At each stage, explain how to test it in Studio before moving on.
