# AGENTS.md - Wasteland Racers

## Project Overview
Wasteland Racers is a top-down, single-screen arcade combat racing game inspired by Badlands (1990) and Super Off Road (1989). It runs entirely in the browser using HTML5 Canvas and vanilla JavaScript.

## Architecture

### Files
- `wasteland-racers.html` - Main game (single-file, self-contained)
- `zone-editor.html` - Track zone editor (standalone tool)
- `tracks/` - Directory for exported track JSON files
- `assets/` - Directory for background images (PNGs from Photoshop)
- `memory.md` - Project state and decisions
- `todo.md` - Task tracking
- `log.md` - Development log with timestamps

### Track Data Format (JSON)
Tracks are defined as JSON with this structure:
```json
{
  "version": 1,
  "name": "Track Name",
  "laps": 3,
  "canvas": { "width": 900, "height": 700 },
  "grid": {
    "cols": 60,
    "rows": 46,
    "data": [["track", 0, "wall", ...], ...]
  },
  "waypoints": [{ "x": 100, "y": 200 }, ...],
  "startPositions": [{ "x": 100, "y": 200, "angle": 0 }, ...],
  "finishLine": { "x1": 0, "y1": 0, "x2": 0, "y2": 0 },
  "backgroundImage": "data:image/png;base64,..." 
}
```

### Zone Types
- `track` - Driveable surface (green overlay)
- `wall` - Solid barrier, cars bounce off (red overlay)
- `hazard` - Slows cars, causes damage over time (yellow overlay)
- `boost` - Speed boost zone (blue overlay)
- `pit` - Repair zone, restores health (purple overlay)
- `0` - Empty/off-track, cars slow down significantly

### Game Engine Details
- Canvas size: 900x700 pixels
- Rendering: requestAnimationFrame loop
- Physics: Simple 2D with angle-based steering, friction, collision
- AI: Follows waypoint chain with steering noise
- Combat: Cannons (bullets), missiles, mines
- Upgrade system: Wrenches as currency, speed/accel/tire/ammo/nitro/missile upgrades

## Development Rules

### Always Do
- Log all changes to `log.md` with timestamps
- Update `todo.md` when tasks are started or completed
- Update `memory.md` with key decisions
- Keep the game as a single HTML file (no build step)
- Keep the zone editor as a separate single HTML file
- Test that exported JSON from editor loads correctly in game
- Maintain 60fps performance

### Never Do
- Don't introduce npm/webpack/build tools
- Don't split into multiple JS files (keep single-file architecture)
- Don't use external CDN dependencies for the core game
- Don't break the JSON track format without updating version number
- Don't remove existing features without explicit user request

### Code Style
- Vanilla JavaScript, no frameworks
- Class-based for game entities (Car, Bullet, etc.)
- Functional for utilities and rendering
- Comments for major sections
- No semicolon-free style, use semicolons

### Key Integration Points
When modifying the game to load custom tracks:
1. Parse the grid data for collision detection (grid lookup is O(1))
2. Use waypoints array for AI pathfinding
3. Use startPositions for car placement
4. Use finishLine for lap counting (line-segment intersection)
5. Render backgroundImage as the track visual layer
6. Apply zone effects: wall=bounce, hazard=slow+damage, boost=speed, pit=heal
