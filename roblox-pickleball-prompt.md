# Prompt: Roblox Pickleball Game (full version)

Copy everything below the line and give it to another AI.

---

# PROJECT: "Dink City" — a Roblox pickleball game

You are an expert Roblox game developer, 3D artist, sound designer, and game designer. Build a complete, polished, fun **pickleball game** in Roblox Studio using Luau. It should look and feel like a top Roblox sports game: responsive controls, satisfying hits, clean UI, and a reason to keep coming back.

Don't just describe things: **build them**. Use the MCP tools below to create scripts, models, sounds, and images, put them in the game, and playtest your own work. Everything you give me must be complete. No placeholders like "add logic here" or "TODO".

---

## 1. Tools (MCP servers) to use

You have MCP servers connected. Use them heavily. At the start, list which of these servers you can reach. If one is missing, tell me what it's for and how to connect it, then keep going with the ones you have. Never stop the whole project because one tool is missing.

### 1.1 Roblox Studio MCP (main tool, required)
Built into Roblox Studio: open Assistant → Manage MCP Servers → turn on "Enable Studio as MCP server". Use it to:
- Look through the open place (Workspace, ServerScriptService, ReplicatedStorage, StarterGui, and so on).
- Create and edit Instances, Parts, Models, Scripts, LocalScripts, ModuleScripts, and GUIs directly in Studio.
- Run Luau code in Studio to build the court and set properties.
- **Start playtests, read the Output window, find errors, and fix them.** Do this after every stage.

### 1.2 Blender MCP (3D models)
`ahujasid/blender-mcp` (now called `mcp-for-blender`). Use it to model and optimize:
- Paddle (a few shapes for the shop), pickleball ball (with the holes), net with posts, and the court surround.
- Stadium parts: bleachers, fences, a scoreboard tower, umbrellas, benches, water coolers, a ball hopper, and palm trees or other décor.
- A lobby area: a spawn plaza, shop stand, leaderboard board, and queue pads.
- Rules: at most 20k triangles per mesh, apply scale and rotation, set the origin sensibly, keep UVs clean, and export `.fbx` (or `.obj`) at Roblox-friendly scale. Use low-poly, stylized shapes with smooth shading where it helps.

### 1.3 Meshy MCP (AI 3D generation)
`meshy-dev/meshy-mcp-server`. Use it for:
- A mascot character (for example, a cartoon pickle with a paddle) for the lobby and thumbnails.
- Crowd NPC models, special cosmetic paddles (a "Lava Paddle", "Galaxy Paddle", "Golden Paddle"), and retextured shop versions.
- Auto-rigging and animating the NPC opponent or mascot (idle, cheer, swing).
- Always pass the result through Blender to cut down triangles and fix scale before importing.

### 1.4 Higgsfield MCP (images and video)
`https://mcp.higgsfield.ai/mcp`. Use it for:
- The game icon (512×512) and 3 thumbnails (1920×1080) with bright, clickable Roblox-style art.
- UI art: the logo, shop item icons, rank badges (Bronze → Champion), and the win and lose screen backgrounds.
- Textures: the court surface, stadium ad boards (made-up brands only), and a sky or backdrop.
- A 15–30 second promo trailer and a short gameplay-style teaser for social media.

### 1.5 ElevenLabs MCP (audio)
`elevenlabs/elevenlabs-mcp`. Use it for:
- Sound effects: a paddle "pop" (soft, normal, and power versions), ball bounce on court, net hit, ball on fence, footsteps on court, swing whoosh, a UI click, buying an item, a level-up, a whistle, crowd cheers (small, big, and a match-point roar), and crowd "ooh" for close calls.
- Music: an upbeat lobby loop, an intense match loop, and a victory sting.
- A referee voice that calls the score ("four, two, one"), faults ("Fault! Kitchen!"), "Out!", "Game point!", and "Match!".

### 1.6 Roblox Open Cloud (uploading assets)
Use an Open Cloud API key with asset upload permission, or an MCP that wraps it, to upload meshes, images, decals, and audio and get back `rbxassetid://` IDs. If that isn't available, give me a list of files to upload by hand and exactly where each asset ID goes.

### 1.7 Asset workflow (for every asset)
**Generate (Meshy / Higgsfield / ElevenLabs) → clean up and optimize (Blender) → upload (Open Cloud or by hand) → put it in the game and wire it up (Roblox Studio MCP) → playtest (Roblox Studio MCP).**

Keep an `ASSETS.md` file listing every asset: name, type, tool used, the prompt that made it, file name, asset ID, and where it's used.

### 1.8 Art direction (keep everything consistent)
- Style: bright, clean, slightly cartoony sports look, like a sunny outdoor resort court.
- Colors: teal court, lighter blue kitchen, white lines, orange and yellow accents, warm sunset lighting.
- No real brands, logos, or copyrighted music or characters. Everything must pass Roblox moderation.
- Limits: meshes ≤ 20k triangles, textures ≤ 1024×1024, and short, compressed audio.

---

## 2. Game overview

- **Genre:** Competitive and casual sports.
- **Players:** 1–16 per server, with several courts in one place.
- **Core loop:** Join the lobby → queue for a court (1v1, 2v2, or practice vs. NPC) → play a match → earn coins and XP → buy cosmetics, rank up → queue again.
- **Feel:** Easy to pick up in 30 seconds, with real depth (dinks, lobs, kitchen play, positioning).

---

## 3. The court and world

### 3.1 Court (build accurately)
- Real size: 20 ft × 44 ft. Use a clear scale (for example, 1 ft = 1.5 studs) and state it in a config ModuleScript so everything reads from it.
- Lines: baselines, sidelines, centerline, and the **non-volley zone ("kitchen")** 7 ft from the net on each side. Lines are about 2 inches wide, scaled.
- Net: 36 in high at the sidelines, 34 in at the center (a slight dip), with posts just outside the sidelines.
- Surrounding area outside the lines for players to run in, a fence around the whole court, and invisible walls so the ball can't leave the play area forever.
- Build the court from code (a `CourtBuilder` module) so its size is exact, then decorate it with the Blender and Meshy models.

### 3.2 World
- **Lobby:** A spawn plaza with the mascot, a shop stand, a leaderboard board (top wins), a daily reward chest, and queue pads for each mode.
- **Courts:** At least 4 courts (2 casual, 1 ranked, 1 practice vs. NPC), each with its own scoreboard.
- **Practice area:** A ball machine that feeds balls to practice drives, dinks, and lobs, plus target zones that award points.
- **Lighting:** Warm sunset (use Future lighting, a bit of Bloom, ColorCorrection, SunRays, and Atmosphere), and keep it fast on mobile.

---

## 4. Controls and shots

### 4.1 Paddle
- A Tool with the paddle model. Players spawn holding it in matches.
- Swing animation with a short wind-up and follow-through, blending cleanly with running.

### 4.2 Shot types (PC / mobile / console)
| Shot | PC | Mobile | Console | Behavior |
|---|---|---|---|---|
| Drive | Left click | Swing button | R2 | Fast, low arc |
| Power drive | Hold and release left click | Hold swing button | Hold R2 | Charge meter; faster, but more likely to go out |
| Lob | Right click | Lob button | L2 | High, deep arc |
| Dink | Shift + click | Dink button | R1 | Soft, short arc into the kitchen |
| Serve | Click when it's your serve | Swing button | R2 | Underhand, aimed diagonally |

- **Aim:** Aim with the camera direction or mouse, mixed with the direction the player is facing. Show a faint aim arc or landing marker for the shot about to be hit (turn it off in ranked).
- **Timing:** A "sweet spot" timing window gives a "Perfect!" hit with a bonus to speed and accuracy, a special sound, and a particle effect.
- **Spin (stretch goal):** Topspin and slice change the arc and bounce.

### 4.3 Movement
- Faster acceleration on the court, with a short cooldown dash or split-step.
- Automatically face the ball when it's coming at you (a setting to turn this off).

---

## 5. Ball physics and networking

- **Server controls the ball.** Don't use default Roblox physics for the ball. Simulate it yourself on the server: position, velocity, gravity, air drag, bounce with energy loss, and friction.
- Check for collisions with the ground, net, fence, and walls along the path between frames so it never passes through (raycast or math along the path).
- **Clients** show a smoothed copy of the ball that follows the server's state, so it looks smooth even with lag. Send the ball's state with a timestamp at a fixed rate.
- Predict where the ball will land and show a marker under incoming balls.
- Unstick the ball automatically: if it doesn't move for a while or leaves the area, call a dead ball and replay the point.

---

## 6. Hit detection and anti-cheat

- The client says "I swung" (with shot type, aim direction, and charge amount). **The server decides whether it hit.**
- Server checks: the ball is within reach, in front of the player, inside the swing's time window, the player isn't on cooldown, and the aim direction is a real direction. Limit how fast players can send swing and move requests.
- Never trust anything the client sends about the ball's position, score, coins, or items.
- Kick or flag players who teleport or move impossibly fast.

---

## 7. Rules (enforce them all)

- **Serve:** Underhand, from behind the baseline, diagonally into the opposite service box. The serve can't land in the kitchen (a serve landing on the kitchen line is a fault). One serve attempt per server.
- **Two-bounce rule:** The serve has to bounce, and the return has to bounce, before anyone can volley.
- **Kitchen rule:** A player can't volley while standing in the kitchen or on its line, and can't step into it from momentum after a volley. Track foot position (the HumanoidRootPart plus a small margin).
- **Faults:** Out of bounds (a ball on the line is in), into the net, bouncing twice, volleying before the two-bounce rule allows, a kitchen violation, a serve into the wrong box, or hitting the ball twice on one side.
- **Scoring:**
  - Default: side-out scoring to 11, win by 2. Only the serving side scores.
  - Doubles: server 1 and server 2, start at "0-0-2", and call the score as "serving score – receiving score – server number".
  - Singles: serve from the right when your score is even, from the left when it's odd.
  - Setting: rally scoring (to 11 or 15, win by 2) for quick matches.
- Switch sides at 6 points in a game played to 11 (configurable).
- Show a big, clear reason for every fault, both on screen and from the referee voice.

---

## 8. Game modes

1. **Casual 1v1 / 2v2:** Queue pads. Start with a 3-second countdown when everyone's ready.
2. **Ranked:** Elo-style rating, ranks from Bronze → Silver → Gold → Platinum → Diamond → Champion, each with a badge image. Rating is saved.
3. **Practice vs. NPC:** Easy, Medium, Hard, and Pro. The NPC tracks the ball, moves to a good spot, picks shots (more dinks and lobs at higher levels), and makes realistic mistakes.
4. **Drills:** A ball machine with target zones for drives, dinks, and lobs, with a score to beat and a leaderboard.
5. **Stretch goal:** A tournament bracket event for 8 players.

---

## 9. UI / UX (StarterGui)

- **Scoreboard:** Team colors, player names and avatar headshots, score in the "4-2-1" format, a server indicator, and "GAME POINT" / "MATCH POINT" banners.
- **Match popups:** "Perfect!", "Kitchen fault!", "Out!", "Net!", "Point: Blue", with smooth tween animations.
- **Power meter** next to the crosshair while charging.
- **Win and lose screen:** The winner's avatar posed, stats (winners, errors, longest rally, perfect hits), coins and XP earned, and "Play again" / "Back to lobby" buttons.
- **Lobby HUD:** Coins, level and XP bar, rank badge, and buttons for the shop, inventory, settings, and daily reward.
- **Shop and inventory:** Categories for paddles, ball trails, hit effects, win celebrations, and titles. Preview items in 3D (ViewportFrame) and equip them.
- **Settings:** Aim assist, auto-face the ball, camera sensitivity, music and sound volume, graphics quality, and a colorblind-friendly line mode.
- **Tutorial:** A short interactive tutorial on first join (move, swing, dink, lob, the kitchen rule, scoring), which can be skipped.
- **Mobile:** Big, clear buttons that don't cover the court. Test on a phone-sized screen with the Device Emulator.
- Style: rounded corners (UICorner), outlines (UIStroke), gradients, the art style's colors, a consistent font, and layouts that scale with UIScale or UIAspectRatioConstraint.

---

## 10. Game feel and polish

- Hit feel: a tiny pause on "Perfect!" hits, a small camera shake on power shots, and a quick color flash on the paddle.
- Effects: a ball trail (cosmetic trails from the shop), particles on hit, dust on bounce, and confetti and fireworks on match win.
- Sound: the ElevenLabs sounds above, with slightly varied pitch so repeated hits don't sound the same; a crowd that reacts to long rallies and close points; and music that switches between the lobby and match.
- Camera: follows behind the player and keeps the court and ball readable. Switch to a wide broadcast camera for serves and replays.
- **Replay:** An instant replay of the match-winning point (record the ball and player positions and play them back).
- Celebrations: emotes after winning a point or match (cosmetic, bought in the shop).

---

## 11. Progression and economy

- **Data:** Use DataStoreService with a session-lock pattern (or ProfileStore-style logic you write yourself). Save coins, XP, level, rating, wins, losses, owned and equipped items, settings, and whether the tutorial is done. Wrap calls in pcall, retry with increasing waits, save on leave and when the server shuts down (BindToClose), and autosave every few minutes.
- **Rewards:** Coins and XP per match (more for winning, a bonus for long rallies and perfect hits), and a daily login reward that grows with a streak.
- **Leaderboards:** leaderstats show Wins. An OrderedDataStore feeds global leaderboard boards in the lobby (Top Wins, Top Rating).
- **Monetization (optional, fair, not pay-to-win):** A Game Pass for VIP (a chat tag, a special trail, 1.5× coins) and Developer Products for coin packs. Cosmetic only. Set up MarketplaceService correctly with ProcessReceipt.
- **Badges:** First win, 10 wins, first "Perfect!", beat Pro NPC, and reach each rank.

---

## 12. Code architecture

Use ModuleScripts with clear names and one job each. Use this structure (change it only if you have a good reason, and explain why):

```
ReplicatedStorage
├── Shared
│   ├── Config            (court size, physics constants, scoring settings, shot tuning)
│   ├── Types             (shared type definitions)
│   ├── BallMath          (arc, collision, and landing prediction math shared by server and client)
│   └── Items             (shop catalog: id, name, price, asset IDs, rarity)
├── Remotes               (RemoteEvents / RemoteFunctions folder)
└── Assets                (meshes, sounds, particles, UI images)

ServerScriptService
├── Main.server.lua       (starts everything)
└── Services
    ├── CourtBuilder
    ├── MatchManager      (queues, match lifecycle, teams)
    ├── BallController    (server ball simulation)
    ├── HitValidator      (anti-cheat hit checks)
    ├── RulesEngine       (faults, two-bounce, kitchen, serve checks)
    ├── ScoreManager      (side-out and rally scoring, callouts)
    ├── NPCController     (practice opponent AI)
    ├── RankedService     (Elo, ranks)
    ├── DataManager       (saving, session locking)
    ├── EconomyService    (coins, XP, rewards, shop purchases)
    ├── MonetizationService
    └── ReplayService

StarterPlayer/StarterPlayerScripts
├── ClientMain.client.lua
└── Controllers
    ├── InputController   (PC / mobile / console)
    ├── BallRenderer      (smoothed ball, trail, landing marker)
    ├── CameraController
    ├── EffectsController (sounds, particles, camera shake)
    └── UIController      (scoreboard, popups, shop, settings, tutorial)

StarterGui
└── (ScreenGuis made in code or built in Studio via the MCP)
```

Coding rules:
- `--!strict` where you can, type annotations on public functions, and clear comments explaining *why*.
- Modern APIs only: `task.wait`, `task.spawn`, `task.delay`, not `wait`, `spawn`, or `delay`. Use `GetAttribute`/`SetAttribute` and `CollectionService` tags where they help.
- Clean up connections when matches end or players leave (no memory leaks).
- Performance: one physics step loop per active ball, not per player. Avoid creating Instances every frame. Reuse effect objects instead of creating new ones.
- All tuning numbers go in `Config`. None hardcoded inside logic.

---

## 13. Edge cases to handle

- A player leaves mid-match (the other side wins by forfeit in ranked; in casual, an NPC fills in or the match is cancelled).
- The ball gets stuck or leaves the map → dead ball, replay the point.
- Both players hit at the same moment → the server decides fairly (closest to the ball wins).
- Laggy players (high ping) → the timing window adjusts a bit for ping, within safe limits.
- The server shuts down mid-match → save everyone's data.
- A player respawns or resets mid-match → put them back in their position.
- Too few players → suggest practice vs. NPC.

---

## 14. How to build it (stages)

Work in these stages. **After each stage: playtest with the Roblox Studio MCP, read the Output window, fix every error, then give me a short report** (what you built, what you tested, what's left).

1. **Setup:** Check which MCP servers are connected. Create the folder structure, Config, and Remotes.
2. **Court:** CourtBuilder makes the exact court. Test by checking sizes and positions.
3. **Ball physics:** Server simulation plus smooth client display. Test by launching balls with a debug command and checking bounces, the net, and walls.
4. **Hitting:** Paddle tool, input for all devices, server hit checks, and all shot types. Test every shot type.
5. **Rules and scoring:** RulesEngine and ScoreManager. Test every fault on purpose (see the checklist).
6. **Match flow:** Queues, countdown, teams, sides, end of match, return to lobby.
7. **NPC opponent:** All 4 difficulty levels.
8. **UI:** Everything in section 9, including mobile.
9. **Art pass:** Blender/Meshy models, Higgsfield textures and UI art, upload, and replace placeholder parts.
10. **Audio pass:** ElevenLabs sounds, music, and referee voice, wired in.
11. **Progression:** Saving, economy, shop, ranked, leaderboards, badges, monetization.
12. **Polish:** Game feel, replays, celebrations, lighting, performance pass (check the MicroProfiler, test on mobile).
13. **Launch kit:** Icon, thumbnails, trailer, game description, and tags.

---

## 15. Testing checklist (run these with the Studio MCP)

- [ ] Serve into the correct box → rally continues. Serve into the wrong box → fault.
- [ ] Serve lands in the kitchen → fault. Serve lands on the baseline or sideline → in.
- [ ] Volley the return of serve → fault (two-bounce rule).
- [ ] Volley while standing in the kitchen → kitchen fault.
- [ ] Ball bounces twice → point to the hitter.
- [ ] Ball hits the net and doesn't cross → fault.
- [ ] Side-out scoring: the receiving side winning the rally doesn't score. Server 1 → server 2 → side out in doubles, with "0-0-2" at the start.
- [ ] 10-10 → game continues until someone leads by 2.
- [ ] Rally scoring setting works.
- [ ] Players switch sides at 6.
- [ ] Player leaves mid-match → handled properly.
- [ ] Data saves on leave and reloads on rejoin, including in Studio with API access turned on.
- [ ] Buying an item removes coins, adds the item, and saves it. It can't be bought twice or with too few coins.
- [ ] Mobile buttons work and don't cover the court.
- [ ] No errors in Output during a full 2v2 match with NPCs filling slots.
- [ ] Runs smoothly (60 FPS on PC, stable on a mid-range phone).

---

## 16. What to give me at the end

1. The full Explorer tree.
2. Every script, complete, with where it goes. (If you built it in Studio via the MCP, also paste the source so I have a backup.)
3. `ASSETS.md` with every generated asset, the prompt that made it, and its asset ID.
4. Setup instructions a beginner can follow, including how to turn on API access for DataStores, set up Game Passes and Developer Products, and upload assets by hand if needed.
5. The completed testing checklist with results.
6. The game's store page description, genre, tags, icon, and thumbnails ready to publish.
7. A list of known problems, and ideas for future updates (seasons, clans, tournaments, new courts, spin shots, voice chat reactions, cross-server matchmaking with MemoryStoreService).

Start with **Stage 1** now. Tell me which MCP tools you can reach, then keep going through the stages until the game is done, checking in with a short report after each one.
