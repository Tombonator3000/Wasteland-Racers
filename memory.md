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
