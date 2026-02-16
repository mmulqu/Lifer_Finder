# New for Me — iNaturalist Lifer Mapper

A minimalist mobile-first web app that compares your iNaturalist life list against species observed in a specific area, revealing what's been seen there that **you haven't found yet**. A personal lifer-hunting tool for nature hikes.

## Architecture

```
┌──────────────────┐       ┌──────────────────────┐       ┌──────────────┐
│  Static Frontend │──────▶│  inat-api-proxy       │──────▶│ iNaturalist  │
│  (GitHub Pages)  │◀──────│  (existing CF Worker)  │◀──────│   API v1     │
└──────────────────┘       └──────────────────────┘       └──────────────┘
```

No new backend needed — uses your existing `inat-api-proxy` Cloudflare Worker for CORS proxying. All pagination and set-difference logic runs client-side.

## Features

- 🔍 Compare your iNat life list against any area
- 📍 **Radius mode**: Tap the map or use GPS to set center + radius (km)
- 🗺 **Place mode**: Search iNaturalist place polygons (parks, counties, etc.)
- 🦉 **Taxa filter**: Narrow by taxonomic group (birds, fungi, plants, odonata…)
- 📱 Mobile-first bottom-sheet UI, works great on phones during hikes
- 📋 Results sorted by observation count (most common = easiest to find)
- 🔗 Each species links to its iNaturalist taxon page

## Quick Start

### 1. Update the proxy URL

In `frontend/index.html`, confirm the `PROXY` constant points to your worker:

```javascript
const PROXY = "https://inat-api-proxy.intrinsic3141.workers.dev";
```

### 2. Deploy to GitHub Pages

```bash
cd frontend
git init
git add .
git commit -m "initial commit"
git branch -M main
gh repo create new-for-me-mapper --public --source=. --push
```

Enable Pages: Settings → Pages → Source: `main` / `/ (root)`.

### Local Development

Just open `index.html` in a browser. The proxy worker handles CORS.

## How It Works

1. Enter your iNaturalist username
2. Select an area (map tap + radius, or search an iNat place)
3. Optionally filter by taxon
4. Client paginates through the iNat species_counts API to get:
   - All species **you** have observed (your life list)
   - All species observed **in the area** by anyone
5. Computes set difference → species in area that aren't on your list
6. Sorted by observation count (most commonly seen = most findable)

## Future Ideas

- Individual observation markers (fetch actual obs, not just species counts)
- GPX route upload → buffer corridor search
- Seasonal / monthly filtering
- "Difficulty" score based on observation rarity
- Offline checklist export (PDF/CSV)
- eBird hotspot integration for birding

## Tech Stack

- Leaflet.js for mapping
- CARTO Voyager basemap
- Vanilla JS (no framework)
- DM Serif Display + DM Sans typography
- Single HTML file, zero build step
