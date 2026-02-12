# Development Log - Wasteland Racers

## 2026-02-12 - Session 1: Project Kickoff

### 14:00 - Project initialization
- Created memory.md, todo.md, log.md
- Researched Badlands (1990) and Super Off Road via Wikipedia
- Read frontend-design skill for best practices
- Reviewed user-uploaded screenshots of Badlands gameplay

### 14:05 - Building Phase 1 prototype
- Starting core game prototype: single-screen top-down racer
- Tech: HTML5 Canvas, vanilla JS, single file
- Target features: track, player car, AI, shooting, HUD

### 14:30 - Phase 1 prototype complete
- Built wasteland-racers.html with full game loop
- Features implemented:
  - Smooth spline-based race track with borders and start line
  - Player car with WASD/Arrow controls
  - 3 AI opponents with track-following AI
  - Cannon shooting (SPACE) with bullet physics
  - Nitro boost system (N key)
  - Mine obstacles and wrench pickups
  - Car-to-car collisions
  - Lap counting (3 laps)
  - Health system with stun/respawn
  - Post-apocalyptic visual style (barrels, wrecks, towers, pipes)
  - Explosion particle effects
  - Dust trail particles
  - Full HUD (position, lap, speed, ammo, health, nitro, time)
  - Menu screen with title and controls
  - Countdown before race
  - Results screen with times
  - Upgrade shop (speed, accel, tires, ammo, nitro, missiles)
  - Wrench currency for upgrades
- Output: /mnt/user-data/outputs/wasteland-racers.html

## 2026-02-12 - Session 2: Zone Editor & Architecture

### 15:00 - Planning zone editor system
- User wants to draw tracks in Photoshop and import as background images
- Need a zone editor to paint driveable areas over the image
- Discussed architecture: Claude Code vs Lovable/master prompt
- Building zone-editor.html as standalone tool
- Zone types: TRACK (driveable), WALL, PIT/HAZARD, BOOST, START/FINISH, AI WAYPOINTS

### 15:30 - Zone Editor complete
- Built zone-editor.html with full feature set
- Features:
  - Load background image (PNG/JPG from Photoshop)
  - Adjustable grid overlay (cols x rows, customizable)
  - Paint zones: Track, Wall, Hazard, Boost, Pit
  - Eraser tool
  - Adjustable brush size (1-10, mouse wheel support)
  - Bresenham line painting for smooth strokes
  - AI waypoint placement with visual path
  - Start position placement (up to 4 cars)
  - Start/finish line drawing
  - Undo support (Ctrl+Z, 50 levels)
  - Keyboard shortcuts (1-5, E, W, S, F, G, [, ])
  - Export JSON (zone data only or with embedded image)
  - Import JSON (load saved tracks)
  - Opacity controls for BG and grid
  - Status bar with live info
- Output: /mnt/user-data/outputs/zone-editor.html

### 15:45 - Created project infrastructure
- Built agents.md with full project documentation
- Built master-prompt.md with portable system prompt for Claude Code / Lovable / Cursor
- Includes: track JSON format spec, zone behavior, physics model, AI system, workflow

## 2026-02-12 - Session 3: Game Engine v2 - Track Loading & Combat

### 16:00 - Major game engine rewrite
- Rewrote wasteland-racers.html to v2 with zone-based track system
- Replaced spline-based collision with grid-based O(1) zone lookup
- All game systems now read from a unified `trackState` object

### 16:15 - Track JSON loading implemented
- Game can now load track JSON files exported from the zone editor
- Press L on menu screen to load a custom track file
- Supports: grid data, waypoints, start positions, finish line, background images
- Fallback: generates a default track from the original spline on startup
- Background image rendering for custom tracks with semi-transparent zone overlay

### 16:20 - Zone effects system
- Wall: cars bounce back, speed reversal
- Hazard: speed reduction (0.96x), random damage over time
- Boost: speed increase to 1.4x max speed
- Pit: gradual healing (+0.3/frame), slight speed reduction
- Off-track (empty/0): heavy friction (0.94x)
- Zone indicator in HUD shows current zone status

### 16:25 - AI waypoint system overhaul
- AI now follows waypoints from trackState (JSON or generated)
- Look-ahead blending: blends current + 2-ahead waypoints for smooth steering
- AI personality system: aggression, accuracy, nitroFreq randomized per car
- AI uses missiles when available

### 16:30 - Finish line lap counting
- Replaced track-index-based lap counting with line-segment intersection
- Uses cross product for direction detection (forward vs backward)
- Works with any finish line angle from the zone editor

### 16:35 - Missile system implemented
- Homing missiles that track nearest opponent within 400px
- Turn rate: 0.06 rad/frame, slight acceleration (1.005x)
- Damage: 35 (vs 15 for bullets), longer stun (30 vs 15 frames)
- Visual: red triangle with smoke trail particles
- Player fires with M key, AI uses missiles based on aggression
- Missile cooldown: 60 frames (separate from cannon cooldown)
- Purchasable in shop: 3 wrenches per missile

### 16:40 - Wrench reward system
- Wrenches now awarded based on race position: 1st=3, 2nd=2, 3rd=1, 4th=0
- Prevents double-awarding with resultsWrenchesAwarded flag

### 16:45 - Sample track created
- Created tracks/ directory
- Generated desert-oval.json: simple oval track with boost, hazard, pit zones
- 60x46 grid, 20 waypoints, 4 start positions

### 16:50 - Updated project documentation
- Updated log.md with all session 3 changes
- Updated todo.md with completed Phase 2 items
- Updated memory.md with new technical decisions

## 2026-02-12 - Session 4: Menu, Progression & Highscores (v3.0)

### 17:00 - Planning new features
- User requested: start menu, track progression, equipment shop, money system, highscore list
- Reviewed reference images showing Badlands-style arcade racing layout
- Planned: 5 built-in tracks, progression system, localStorage save, enhanced UI

### 17:10 - Added progression system
- New game states: TRACK_SELECT, HIGHSCORES (added to STATE object)
- Progression object tracks: currentTrackIndex, unlockedTracks, totalWrenches, money, highscores
- Save/load system using localStorage (wasteland-racers-save key)
- Player upgrades persist across sessions

### 17:15 - Created 5 built-in track definitions
- Track 1: "Wasteland Circuit" (EASY, 3 laps) - classic oval
- Track 2: "Scorched Serpent" (MEDIUM, 3 laps) - winding S-curves
- Track 3: "Rust Ring" (MEDIUM, 4 laps) - oval with chicane
- Track 4: "Toxic Speedway" (HARD, 3 laps) - hazard-heavy, extra hazard zones
- Track 5: "Thunderdome" (EXTREME, 4 laps) - complex multi-turn, triple hazards
- Each track defined by spline points, difficulty, laps, description, reward table
- generateTrackFromDef() builds any track from definition index

### 17:20 - Built proper start menu
- Two-option menu: START RACE and HIGHSCORES
- Arrow key navigation with visual selection indicator
- Animated background with moving lines
- Title with glow effect
- Shows progression info (tracks unlocked, money)
- Controls reference at bottom

### 17:25 - Added track selection screen
- Lists all 5 tracks with locked/unlocked status
- Shows: track number, name, difficulty (color-coded), description, laps, reward
- Displays best time per track from highscores
- Locked tracks show requirement (complete previous track)
- Card-based layout with hover highlight

### 17:30 - Enhanced results screen
- Table layout showing all racers with position, name, time, reward
- Player row highlighted with orange border
- Money earned summary box
- NEW RECORD indicator with blinking animation
- NEW TRACK UNLOCKED indicator when completing a track for first time
- Options: Enter=Shop, N=Next Track, R=Race Again, ESC=Menu

### 17:35 - Enhanced shop screen
- Split into two sections: Vehicle Upgrades (permanent) and Supplies (consumables)
- Card-based layout for each upgrade
- Visual upgrade bars for permanent upgrades (5 levels)
- Shows current stock for consumables
- Money-based economy (separate from wrenches)
- Cost display with color coding (affordable=green, unaffordable=red)
- Upgrade costs: Speed/Accel/Tires=$2, Ammo=$1, Nitro=$3, Missile=$4

### 17:40 - Added highscore system
- Hall of Fame screen showing all tracks with best times and positions
- Color-coded position badges (gold/silver/bronze)
- Track completion counter and total money earned stats
- DELETE key to reset all progress
- Data persisted in localStorage

### 17:45 - Implemented track progression
- Complete any track (finish the race) to unlock the next track
- Track 1 unlocked by default, rest locked
- Progression saved to localStorage
- Money reward system: position-based rewards per track (e.g. 1st on Track 1 = $4)
- claimRaceReward() handles: money, highscore save, track unlock

### 17:50 - Updated input handling and game loop
- All 7 game states handled in game loop switch
- Menu: UP/DOWN to select, ENTER to confirm
- Track Select: UP/DOWN, ENTER to race, ESC back
- Highscores: ESC/ENTER back, DELETE to reset
- Results: ENTER=shop, R=retry, N=next track, ESC=menu
- Shop: number keys for purchases (uses progression.money), ENTER=race, ESC=menu
- raceRewardClaimed flag prevents double-claiming rewards

### 17:55 - Updated documentation
- Updated log.md with all session 4 changes
- Updated todo.md with completed Phase 3 items
