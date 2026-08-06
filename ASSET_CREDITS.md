# Asset credits

## Kenney — Space Shooter Redux

- Source: https://opengameart.org/content/space-shooter-redux
- Creator: Kenney (https://kenney.nl)
- License: Creative Commons Zero 1.0 Universal (CC0 1.0)
- Local license copy: `assets/kenney/LICENSE.txt`

Used and visually adapted at runtime:

- `starfield-black.png` — subtle moving sky layer
- `enemy-scout.png` — enemy scout body silhouette
- `audio/*.ogg` — energy, combat, confirmation, and loss sound effects

`defender-drone.png` is no longer used — it was a radially symmetric badge
with no front or back, so it never actually read as a body silhouette once
drawn rotated in play. The defender body is now hand-drawn vector artwork,
matching the enemy's own vector fallback style. The file is left in
`assets/kenney/` unreferenced rather than deleted, in case it's wanted again.

The charge cores, charge arcs, glows, projectiles, structures, battlefield effects,
defender bodies, and all gameplay visuals not listed above remain original
Mishmar Protocol artwork.
