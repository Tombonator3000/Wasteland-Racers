# Project Memory - Wasteland Racers

## Project Overview
- Clone/tribute inspired by Badlands (1990) and Super Off Road (1989)
- Top-down, single-screen arcade racing with combat
- Post-apocalyptic setting
- 2D graphics, HTML5 Canvas
- Features: racing, shooting, upgrades, track editor

## Key Design Decisions
- Single HTML file with embedded JS/CSS for artifact compatibility
- Canvas-based rendering
- Keyboard controls for player input
- AI opponents
- Upgrade shop between races

## Tech Stack
- HTML5 Canvas
- Vanilla JavaScript
- No external dependencies
- Zone editor: standalone HTML tool, exports JSON track data
- Track format: grid-based zones + waypoints + start positions + finish line + optional base64 background image

## Game Mechanics (from source games)
- Badlands: 3 cars, 3-lap races, wrenches for upgrades, cannons, missiles, mines, barricades
- Super Off Road: up to 4 players, nitro boosts, money/points for upgrades, 8+ tracks

## Architecture Decisions (Session 3)

### Track System
- Unified `trackState` object holds all track data (grid, waypoints, startPositions, finishLine, bgImage)
- Default track generated from cardinal spline, converted to 60x46 grid
- Custom tracks loaded from JSON files via file input (press L on menu)
- Grid-based collision: O(1) zone lookup via `getZoneAt(x, y)`
- Two rendering paths: default (opaque zones + environment) and custom (background image + transparent overlay)

### Collision & Physics
- Replaced spline-based `closestTrackPoint()` with grid cell lookup
- Zone effects applied per frame: wall=bounce, hazard=slow+damage, boost=speed, pit=heal
- Finish line detection: line-segment intersection with cross-product direction check
- Bullets collide with wall zones (explode on impact)

### AI System
- AI follows waypoints from trackState (either generated or from JSON)
- Look-ahead blending: current waypoint + 2 ahead for smoother paths
- Personality system: each AI has randomized aggression, accuracy, nitroFreq

### Combat
- Cannons: 15 damage, 15 stun, SPACE key, 12 frame cooldown
- Missiles: 35 damage, 30 stun, M key, homing (0.06 rad/frame turn), 60 frame cooldown
- Missiles target nearest opponent within 400px, slight acceleration (1.005x)
- Purchasable in shop: ammo=1 wrench, nitro=2 wrenches, missile=3 wrenches

### Progression
- Wrenches awarded by finishing position: 1st=3, 2nd=2, 3rd=1, 4th=0
- Upgrade levels: speed, accel, tires (max 5 each)
- Consumables: ammo, nitro, missiles (no max)

## Deployment (Session 5)

### GitHub Pages
- `index.html` serves as the landing page (post-apocalyptic themed)
- Links to `wasteland-racers.html` (game) and `zone-editor.html` (editor)
- Game is fully self-contained, no build step, no CDN dependencies
- Enable GitHub Pages in repo Settings > Pages > Deploy from branch (main)
- Game URL: `https://<username>.github.io/Wasteland-Racers/`

### Responsive Design
- Canvas CSS: `max-width: 100vw; max-height: 100vh` scales to fit viewport
- Browser key defaults prevented (no scrolling when pressing arrows/space)
- Game internal resolution stays 900x700 (CSS scales the canvas element)

## File Structure
```
wasteland-racers/
  index.html               # Landing page for GitHub Pages
  wasteland-racers.html    # Main game (v3.1)
  zone-editor.html         # Track editor
  agents.md                # Instructions for AI tools
  memory.md                # This file
  todo.md                  # Task tracking
  log.md                   # Development log
  master-prompt.md         # Portable system prompt
  tracks/                  # Exported track JSON files
    desert-oval.json       # Sample oval track
  assets/                  # Background images, sprites (future)
```
