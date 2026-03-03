# Project Guidelines

## Code Style
- Primary implementation lives in `create_map_poster.py`; keep changes surgical and avoid broad refactors.
- Follow existing Python style: `snake_case` names, module-level constants in `UPPER_SNAKE_CASE`, and pragmatic type hints where already used.
- Keep line length compatible with repo linting (`max-line-length=160`).
- Prefer existing CLI/output style: `argparse` flags and user-facing `print(...)` status messages (`✓`, `⚠`, `✗`) instead of introducing a logging framework.

## Architecture
- Main flow: CLI (`argparse`) → geocoding (`Nominatim`) → OSM fetch (`osmnx`) → rendering (`matplotlib/geopandas`) → output file.
- Key files:
  - `create_map_poster.py`: CLI, caching, OSM fetch, rendering pipeline.
  - `font_management.py`: Google Fonts lookup/download and local font caching.
  - `themes/*.json`: visual theme definitions used by `load_theme()`.
- Preserve rendering layer intent and road hierarchy in `create_map_poster.py` (`get_edge_colors_by_type`, road width/color rules).

## Build and Test
- Run (uv): `uv run ./create_map_poster.py --city "Paris" --country "France"`
- Run (python): `python create_map_poster.py --city "Paris" --country "France"`
- List themes: `python create_map_poster.py --list-themes`
- Lint: `flake8 . --count --statistics --max-line-length=160`
- Lint: `pylint . --max-line-length=160`
- Type check: `mypy . --ignore-missing-imports --no-strict-optional`
- Syntax check: `python -m compileall . -q`
- Batch variation script: `bash test/all_variations.sh`

## Project Conventions
- Theme schema must stay compatible with existing keys in `themes/*.json` (`bg`, `text`, `water`, `parks`, `road_*`, etc.).
- Keep theme loading behavior stable: filesystem themes first, fallback defaults from `create_map_poster.py`.
- Respect output and cache conventions:
  - Output: `posters/{city}_{theme}_{timestamp}.{format}`
  - Cache: directory from `CACHE_DIR` (default `cache/`) with pickle-backed objects.
- Preserve script-aware typography behavior (Latin spacing vs non-Latin natural spacing) and Chinese font handling.

## Integration Points
- External services/libraries used directly:
  - Nominatim via `geopy` (`get_coordinates`)
  - OSM data via `osmnx` (`fetch_graph`, `fetch_features`)
  - Plotting via `matplotlib` + `geopandas`
  - Font fetching via `requests` in `font_management.py`
- Avoid changing public CLI flags in `create_map_poster.py` unless explicitly requested.

## Security
- Network access is required for geocoding and optional font download; keep timeouts/user-agent/rate-limit behavior intact.
- Cache uses `pickle`; treat cache files as trusted-local data and avoid expanding pickle usage scope.
- On failures, current behavior prints traceback for debugging; do not add secret-bearing output.
