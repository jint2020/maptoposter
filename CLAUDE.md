# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

City Map Poster Generator - generates minimalist map posters for any city in the world using OpenStreetMap data, OSMnx, and matplotlib.

## Commands

```bash
# Run the script (uses uv if available, otherwise python)
uv run ./create_map_poster.py --city "Paris" --country "France"
python create_map_poster.py --city "Paris" --country "France"

# List available themes
python create_map_poster.py --list-themes

# Linting (max line length: 160)
flake8 . --count --statistics --max-line-length=160
pylint . --max-line-length=160

# Type checking
mypy . --ignore-missing-imports --no-strict-optional

# Syntax validation
python -m compileall . -q
```

## Code Architecture

### Main Entry Point
- [create_map_poster.py](create_map_poster.py) - Single 33KB script containing all logic
- [font_management.py](font_management.py) - Font loading and Google Fonts integration

### Data Flow
```
CLI (argparse) → Geocoding (Nominatim) → Map Fetch (OSMnx) → Rendering (matplotlib) → PNG Output
```

### Key Modules
| Module | Purpose |
|--------|---------|
| `get_coordinates()` | City → lat/lon via Nominatim geocoding |
| `create_poster()` | Main rendering pipeline |
| `get_edge_colors_by_type()` | Road color by OSM highway tag |
| `load_theme()` | JSON theme → dict |

### Rendering Layers (z-order)
```
z=11  Text labels
z=10  Gradient fades
z=3   Roads (via ox.plot_graph)
z=2   Parks
z=1   Water
z=0   Background
```

### Theme System
Themes are JSON files in `themes/` directory. Required properties:
- `bg`, `text`, `water`, `parks`
- Road colors: `road_motorway`, `road_primary`, `road_secondary`, `road_tertiary`, `road_residential`, `road_default`

### OSM Highway Hierarchy
```
motorway, motorway_link → Thickest (width 1.2)
trunk, primary → Thick (width 1.0)
secondary → Medium (width 0.8)
tertiary → Thin (width 0.6)
residential, living_street → Thinnest (width 0.4)
```

### Script Detection
The script automatically detects text scripts (Latin vs Non-Latin) for typography:
- Latin: Letter spacing applied ("P  A  R  I  S")
- Non-Latin: Natural spacing ("東京")
- Detection: If >80% alphabetic chars are Latin (U+0000-U+024F), spacing is applied