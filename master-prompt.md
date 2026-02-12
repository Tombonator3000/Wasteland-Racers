# Master System Prompt - Wasteland Racers

Use this prompt with Claude Code, Lovable, Cursor, or other AI development tools.
Copy the section below into your system prompt or project instructions.

---

## SYSTEM PROMPT START

You are working on "Wasteland Racers", a browser-based top-down arcade combat racing game. The game is inspired by Badlands (1990, Atari) and Super Off Road (1989, Leland Corporation).

### Core Concept
- Single-screen top-down racing on a fixed circuit
- 4 cars racing simultaneously (1 player + 3 AI)
- Combat mechanics: cannons, missiles, mines
- Post-apocalyptic theme and visual style
- Upgrade shop between races (wrenches as currency)
- Custom tracks via background images + zone data

### Technical Stack
- Pure HTML5 Canvas + vanilla JavaScript
- Single-file architecture (one .html file per tool)
- No build tools, no npm, no frameworks
- Target: 900x700 canvas at 60fps
- Track data stored as JSON files

### The Project Has Two Main Components

**1. The Game (`wasteland-racers.html`)**
A self-contained HTML file with the racing game engine. Features:
- Spline-based or grid-based track rendering
- Player controls: Arrow keys/WASD (steer/accelerate), SPACE (shoot), N (nitro)
- AI opponents that follow waypoints
- Collision detection against track zones (grid-based, O(1) lookup)
- Combat: bullets, missiles, mines, health, stun, respawn
- Lap counting via start/finish line intersection
- HUD: position, lap, speed, ammo, health, nitro, wrenches, time
- Game states: Menu, Countdown, Racing, Results, Shop
- Upgrade system: speed, acceleration, tires, ammo, nitro, missiles
- Particle effects: dust trails, explosions
- Background image support for custom track art

**2. The Zone Editor (`zone-editor.html`)**
A standalone track editor that produces JSON files the game can load. Features:
- Load a background image (user's Photoshop art) as the track visual
- Grid overlay with adjustable resolution (default 60x46)
- Paint zones with brush tool: Track, Wall, Hazard, Boost, Pit, Eraser
- Place AI waypoints (ordered path for AI to follow)
- Place start positions (up to 4 cars)
- Draw start/finish line
- Export/Import JSON track files
- Can embed background image as base64 in the JSON

### Track JSON Format (v1)
```json
{
  "version": 1,
  "name": "string",
  "laps": 3,
  "canvas": { "width": 900, "height": 700 },
  "grid": {
    "cols": 60,
    "rows": 46,
    "data": [["row of zone values..."]]
  },
  "waypoints": [{ "x": number, "y": number }],
  "startPositions": [{ "x": number, "y": number, "angle": number }],
  "finishLine": { "x1": number, "y1": number, "x2": number, "y2": number },
  "backgroundImage": "data:image/png;base64,..." 
}
```

Zone values in grid.data: `0` (empty), `"track"`, `"wall"`, `"hazard"`, `"boost"`, `"pit"`

### Zone Behavior in Game
- `track`: Normal driving surface, full speed and grip
- `wall`: Solid barrier. Cars bounce off, lose speed
- `hazard`: Slow zone. Reduces speed, may cause damage over time
- `boost`: Speed boost. Temporarily increases max speed
- `pit`: Repair zone. Gradually restores health
- `0` (empty): Off-track. Heavy friction, very slow

### Game Physics Model
- Cars have: position (x,y), angle, speed, maxSpeed, acceleration, friction
- Steering is angle-based, turn rate scales with speed
- On-track friction: 0.985, off-track: 0.94
- Grid collision: convert car position to grid cell, check zone type
- Car-car collision: circle-based push apart
- Bullets: linear projectiles with owner tracking, 15 damage + stun

### AI System
- AI cars follow a chain of waypoints placed in the editor
- Look-ahead: 10-20 waypoints forward for steering target
- Steering: angle difference to target, with random noise for variety
- AI shoots when an opponent is within 150px and within 0.5 rad of forward angle
- AI uses nitro randomly (low probability per frame)

### Development Workflow
1. User draws track art in Photoshop (900x700 PNG)
2. User loads the image into the Zone Editor
3. User paints zones, places waypoints, sets start positions and finish line
4. User exports track as JSON
5. Game loads the JSON and uses it for racing

### File Structure
```
wasteland-racers/
  wasteland-racers.html    # Main game
  zone-editor.html         # Track editor
  agents.md                # Instructions for AI tools
  memory.md                # Project state tracking
  todo.md                  # Task list
  log.md                   # Development log
  tracks/                  # Exported track JSON files
  assets/                  # Background images, sprites
```

### Rules
- Always update log.md with timestamps when making changes
- Always update todo.md when completing or starting tasks
- Keep everything in single HTML files, no build step
- Maintain the JSON track format for compatibility between editor and game
- Performance target: 60fps with 4 cars, particles, bullets on screen
- The game must work offline (no CDN dependencies for core functionality)

## SYSTEM PROMPT END

---

## How to Use This Prompt

### With Claude Code
1. Create a git repo with the project files
2. Add this prompt content to `.claude/instructions.md` or paste when starting a session
3. Also keep `agents.md` in the repo root (Claude Code reads this automatically)
4. Run `claude` in the project directory

### With Lovable
1. Create a new Lovable project
2. Paste the SYSTEM PROMPT section into the project instructions
3. Note: Lovable works best with React/component apps. For a Canvas game, you may need to wrap it in a React component that renders a canvas
4. Connect to GitHub for version control

### With Cursor
1. Add the SYSTEM PROMPT section to `.cursorrules` in the project root
2. Keep `agents.md` in the repo root
3. Open the project folder in Cursor

### With Any AI Tool
1. Paste the SYSTEM PROMPT section as context/instructions
2. Provide the current state of the HTML files as needed
3. Ask for specific features or changes
