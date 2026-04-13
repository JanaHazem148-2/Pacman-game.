# Pac-Man

A faithful Pac-Man clone built in Python using Pygame, featuring all four classic ghosts with individual AI behaviours, power-up mechanics, sound effects, and a full board layout.

---

## Gameplay

- Eat all dots and power pellets to win
- Avoid the four ghosts — contact costs a life
- Collect a power pellet to turn ghosts blue and eat them for bonus points
- 3 lives per game — press **Space** to restart after game over or victory

---

## Controls

| Key | Action |
|-----|--------|
| ← → ↑ ↓ | Move Pac-Man |
| Space | Restart after game over / victory |

---

## Scoring

| Action | Points |
|--------|--------|
| Dot | 10 |
| Power pellet | 50 |
| 1st ghost eaten | 200 |
| 2nd ghost eaten | 400 |
| 3rd ghost eaten | 800 |
| 4th ghost eaten | 1600 |

Ghost score doubles with each consecutive ghost eaten during a single power-up.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3 |
| Graphics & Input | Pygame |
| Pathfinding | A\* (via `heapq`) |
| Board Layout | `board.py` — 33×30 integer grid |

---

## Project Structure

```
pacman/
├── pacman.py          # Main game loop, player logic, ghost AI, collision detection
├── board.py           # 33×30 board layout encoded as a 2D integer grid
├── assets/
│   ├── player_images/
│   │   └── 1.png – 4.png      # Pac-Man animation frames
│   ├── ghost_images/
│   │   ├── red.png             # Blinky
│   │   ├── pink.png            # Pinky
│   │   ├── blue.png            # Inky
│   │   ├── orange.png          # Clyde
│   │   ├── powerup.png         # Frightened ghost (blue)
│   │   └── dead.png            # Dead ghost (eyes only)
│   └── sounds/
│       ├── pacman_eatfruit.wav
│       ├── pacman_eatghost.wav
│       ├── pacman_dies_y.wav
│       └── pacman_intro.wav
└── freesansbold.ttf            # Font for score/UI text
```

---

## Board Encoding (`board.py`)

The board is a 33×30 grid of integers. Each value maps to a tile type:

| Value | Tile |
|-------|------|
| 0 | Empty (black) |
| 1 | Dot |
| 2 | Power pellet (big dot) |
| 3 | Vertical wall segment |
| 4 | Horizontal wall segment |
| 5 | Top-right corner arc |
| 6 | Top-left corner arc |
| 7 | Bottom-left corner arc |
| 8 | Bottom-right corner arc |
| 9 | Ghost house gate |

---

## Ghost AI

Each ghost has a distinct movement strategy:

| Ghost | Colour | Behaviour |
|-------|--------|-----------|
| **Blinky** | Red | Pursues Pac-Man directly; continues straight until forced to turn |
| **Pinky** | Pink | Turns left/right aggressively toward Pac-Man; up/down only on collision |
| **Inky** | Blue (cyan) | Turns up/down aggressively; left/right only on collision |
| **Clyde** | Orange | Turns in any direction whenever it gives a positional advantage |

**During a power-up**, ghosts flee toward the opposite corner of the screen. Once the power-up ends they resume chasing. Dead ghosts return to the ghost house at increased speed (4 px/frame) and revive on entry.

**Speed summary:**

| State | Speed |
|-------|-------|
| Normal | 2 px/frame |
| Power-up active | 1 px/frame |
| Eaten (returning) | 4 px/frame |

The project also includes a partial A\* pathfinding implementation (`Node` class + `a_star` function using Manhattan distance heuristic) that is not yet integrated into the main ghost movement.

---

## Prerequisites

- Python 3.8+
- Pygame

```bash
pip install pygame
```

---

## Running the Game

```bash
python pacman.py
```

Make sure the `assets/` folder and `freesansbold.ttf` are in the same directory as `pacman.py`.

---

## Window

- Resolution: **900 × 950 px**
- Frame rate: **60 fps**
- Score, power-up status, and remaining lives are displayed in the bottom bar
