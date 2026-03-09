# JS‑Puzzle‑RPG

A **single‑page HTML5 puzzle game** where the player writes JavaScript code to control a hero on a top‑down RPG‑style grid. Each level (mission) introduces a new programming concept – variables, loops, functions, conditionals, arrays, etc.  The game is built with plain `<canvas>` and runs entirely in the browser; the player code is executed inside a secure sandboxed `<iframe>`.

## How it works
* **Map** – JSON files in `levels/` describe a tile map (0 = wall, 1 = floor), player start, enemies and items.
* **Engine** – `index.html` contains all game logic (render loop, entity system, turn scheduler).
* **Sandbox** – The player’s script is sent to an invisible iframe with `sandbox="allow-scripts"`. Only a tiny whitelisted API is exposed:
  ```js
  API.move('up'|'down'|'left'|'right');
  API.attack();
  API.pick('itemType');
  API.log('msg');
  API.getEnemies();   // [{x, y}, …]
  API.getPlayer();    // {x, y}
  ```
  The sandbox can only communicate via `postMessage`; infinite loops are prevented by a step counter.
* **Progression** – Levels 1‑10 unlock increasingly advanced concepts.  The objective for each level is shown above the editor.
* **UI** – A canvas on the left, a code editor (`<textarea>`) on the right, a **Run** button, a **Reset** button, an objective display, and a console pane for `log` output.

## Adding new levels
1. Create a new JSON file `levels/levelN.json` following the existing schema.
2. Increment the `id` field in the file (or rely on the filename).  The game will automatically load the next level after a win, up to level 10.

## Running locally
```bash
# from the project root
python -m http.server 8000  # any static web server works
open http://localhost:8000/index.html
```

## Deploying to GitHub Pages
1. Commit the repository.
2. In GitHub, enable **Pages** on the `main` (or `gh-pages`) branch, selecting the root folder.
3. GitHub will serve `https://<your‑username>.github.io/<repo>/index.html`.

## License
Free assets are used from the **Kenney** public domain asset packs (CC0).  The source code in this repository is released under the MIT License.
