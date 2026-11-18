# enoch-tiles

Pre-baked map tiles for the World of Enoch flat map: a north-pole azimuthal equidistant disc.

## Projection
- Centre = North Pole. Longitude 0 points down, +90 E points right.
- Normalised disc coordinates: `x, y` in [-1, 1], `x` right, `y` down.
- `r = (90 - lat) / 180`, measured in disc radii (equator at r = 0.5, South Pole / ice wall at r = 1).
- `x = r * sin(lon)`, `y = r * cos(lon)` (in disc radii, so `r = 1` is the rim).
- Level `L` is a `2^L x 2^L` grid covering the square [-1, 1] x [-1, 1]. Tile `(tx, ty)` covers
  `x in [tx/2^L*2-1, (tx+1)/2^L*2-1]`, `y in [ty/2^L*2-1, (ty+1)/2^L*2-1]`, row 0 at the top.
- Only tiles that touch the disc exist; others return 404 (treat as transparent / 0 m).

## Layers
| layer | URL template | size | format | levels |
|---|---|---|---|---|
| imagery | `imagery/{L}/{ty}/{tx}.webp` | 512 px | WebP (RGBA on the rim, alpha = disc edge) | 0-7 |
| height | `height/{L}/{ty}/{tx}.png` | 256 px | PNG, Terrarium encoding | 0-6 |

Terrarium decoding: `height_m = (R * 256 + G + B / 256) - 32768`. Real metres (integer), sea floor negative.
Pixels outside the disc are 0 m.

`tiles.json` describes all of this in machine-readable form.

## Sources and credits
- Imagery: **Sentinel-2 cloudless 2020 by EOX IT Services GmbH** (https://s2maps.eu), contains modified Copernicus Sentinel data 2020. Licensed CC BY-NC-SA 4.0 (non-commercial). Source: EOX WMTS zoom 7, reprojected.
  Mercator has no data poleward of 85.05 deg; those caps (central Arctic Ocean, interior Antarctica) are filled with a procedural sea-ice / ice-sheet texture blended to the edge colours. They are not imagery.
- Land elevation: **Copernicus DEM GLO-90** (c) DLR e.V. 2010-2014 and (c) Airbus Defence and Space GmbH 2014-2018, provided under COPERNICUS by the European Union and ESA; all rights reserved. Free licence: https://spacedata.copernicus.eu/documents/20123/121286/CSCDA_ESA_Mission-specific+Annex_31_Oct_22.pdf
- Bathymetry and gaps: **GEBCO Compilation Group (2024) GEBCO 2024 Grid** (doi:10.5285/1c44ce99-0a0d-5f4f-e063-7086abc0ea0d), ice-surface version.
- Elevation grid is 1 arc-minute (~1.85 km); Copernicus where it has land, GEBCO for sea and voids.

No NASA data is used.

`preview.jpg` is the whole disc from imagery level 3.
