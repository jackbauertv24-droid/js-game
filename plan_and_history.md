# JS‑Puzzle‑RPG Project Plan & Session History

---

## 📋 Project Overview
You want a **single‑page HTML5 puzzle game** where the player writes JavaScript code to control the avatar. The game shows a top‑down grid map (RPG style) with enemies and items. Each level is a “mission” and the player must script the character to clear the map. The game is educational, gradually increasing programming difficulty.

---

## 🗂️ Session History

**User (initial)**
> I want to create a html5 web based game, the main game play is showing a bird eye view of grid, with a RPG style map, main character and various enemy and items on it. the key for the game is you can NOT control the character action by traditional control like arrow keys, instead, you have to write javascript to control the movement and action of the character. each round, is like a mission, in a forest, or a dungeon, the main character must be controlled by a series of javascript command, so that it can eliminate enemy, take items and clear the map. it is kind of like a puzzle game. the objective of this game is to allow the player to learn programming concept.

**Assistant (plan)**
> Provided a high‑level architecture, milestones, tasks, resource list, and a set of clarification questions (A‑G).

**User (clarifications)**
> 1) plain canvas. 2) the complexity of the programming syntax should go up as level goes up. 3) single page, single html, single js if possible. reduce the number of files to minimum. 4) json map 5) sandbox is needed. in addition, please generate relevant game assets.

**Assistant (detailed plan)**
> Delivered a concrete, step‑by‑step plan respecting the constraints:
> - Plain `<canvas>` rendering, no external engine.
> - All logic lives in a single `index.html` script block.
> - Levels described by JSON files.
> - Secure iframe sandbox exposing a whitelist API.
> - Progressive unlocking of API commands per level.
> - Asset strategy (placeholder data‑URL PNGs or free Kenney assets).
> - Timeline split into ~10 days of work.
> - Detailed task list, JSON schema example, testing strategy, security considerations.
> - Requested additional clarification (tile size, art style, number of levels, concepts per level, asset generation preference, hosting context, extra UI). 

**User (final request)**
> write the plan and all session history to a MD

---

## 📚 Full Plan (as previously given)

### 1️⃣ Clarifying Questions (still pending)
| # | Question |
|---|----------|
| **A** | Tile size & map dimensions – e.g., 32 px tiles, 20 × 15 grid? |
| **B** | Art style – pixel‑art (8‑bit), low‑poly, or simple colored squares? |
| **C** | Number of built‑in levels you’d like to start with (e.g., 3–5)? |
| **D** | Desired programming concepts per level (variables, loops, functions, etc.) |
| **E** | Asset generation preference – ready‑made free sprite sheets or programmatic placeholders? |
| **F** | Hosting context – static site (GitHub Pages) or local file? |
| **G** | Optional extra UI – side console for `log()` output? |

### 2️⃣ Repository Layout
```
/ (root)
│
├─ index.html                # single HTML file (canvas + UI + script)
├─ levels/
│   ├─ level1.json
│   ├─ level2.json
│   └─ …
└─ assets/
    ├─ tiles.png
    ├─ player.png
    ├─ enemy.png
    └─ item.png
```
*Only `index.html` contains JavaScript code.*

### 3️⃣ Core Components (inside the inline script)
- **Game** – canvas init, main loop, level loader.
- **TileMap** – draws background tiles.
- **Entity** (base) → `Player`, `Enemy`, `Item`.
- **TurnScheduler** – processes queued commands, runs enemy AI, checks win/lose.
- **Sandbox** – iframe with `srcdoc`, whitelist API (`move`, `attack`, `pick`, `log`, `getEnemies`).
- **Message Bridge** – `postMessage` communication.
- **UI** – canvas, code editor (plain `<textarea>` or Monaco), Run/Reset buttons, optional side panel.
- **Level Loader** – fetches JSON, builds map, registers allowed API list.
- **Progressive API Unlock** – each level’s JSON defines which commands are exposed.

### 4️⃣ Detailed Development Timeline (≈ 2 weeks)
| Week | Milestone | Tasks |
|------|-----------|-------|
| 0 | Finalize specs (answer A‑G) | – |
| 1 | Scaffold | Create repo, `index.html` skeleton, placeholder assets, sample level JSON. |
| 2 | Canvas & TileMap | Init canvas, draw tiles from JSON. |
| 3 | Entities & Turn System | Implement movement, collision, command queue. |
| 4 | Sandbox & API | Build iframe sandbox, expose whitelist, step‑limit, error handling. |
| 5 | Level JSON & Progression | Define schema, author 3‑5 levels, progressive API unlock. |
| 6 | UI Polish | Side panel with objectives, console output, Run/Reset wiring. |
| 7 | Testing & Debugging | Jest unit tests, Cypress e2e, manual play‑testing. |
| 8 | Asset Integration | Replace placeholders with free CC0 sprites (Kenney). |
| 9 | Deployment | Build script, publish to GitHub Pages / Netlify, write README. |
|10 | Future‑proofing | Documentation for adding new levels/APIs, roadmap. |

### 5️⃣ Asset Generation Strategy
- **Zero‑dependency placeholders** – generate 32 × 32 PNG data‑URLs for player, enemy, item, and a minimal tileset.
- **Free assets** – download Kenney’s *RPG Tilepack* (CC0) and use its sprites.
- **Procedural generation (optional)** – simple canvas draw that outputs a data‑URL.

### 6️⃣ Security & Sandbox Details
- `<iframe sandbox="allow-scripts">` isolates the player code.
- Only a **whitelisted API object** is passed to the user script.
- **Step counter** (`MAX_STEPS = 500`) aborts infinite loops.
- Script size limited to 5 KB before sending to sandbox.
- Errors and console logs are sent back via `postMessage` and displayed.

### 7️⃣ Example Level JSON (template)
```json
{
  "name": "Tutorial – Move",
  "tiles": [
    [0,0,0,0,0],
    [0,1,1,1,0],
    [0,1,2,1,0],
    [0,1,1,1,0],
    [0,0,0,0,0]
  ],
  "tileSize": 32,
  "playerStart": { "x": 2, "y": 2 },
  "enemies": [{ "x": 3, "y": 2, "hp": 1 }],
  "items": [{ "x": 1, "y": 2, "type": "gold" }],
  "objective": "Move to the right and attack the goblin.",
  "api": ["move", "attack", "log"],
  "solution": "move('right'); attack();"
}
```

### 8️⃣ Testing Strategy
- **Unit** – Jest tests for movement, collision, scheduler.
- **Integration** – Verify sandbox receives correct command batches.
- **E2E** – Cypress loads a level, injects the author’s solution script, asserts mission success.
- **Manual** – Test error handling (syntax errors, infinite loops).

---

## 🚀 Next Steps
1. **Provide answers to the pending clarification questions (A‑G).**
2. With those answers locked in, we will:
   - Create the repo and the initial scaffold.
   - Implement the canvas, tilemap, and basic entity system.
   - Continue through the milestones as listed.

Feel free to add any additional constraints or preferences, and we’ll incorporate them into the implementation plan.

---

*This file captures the entire conversation up to this point and the concrete project plan.*
