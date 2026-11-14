# DUSTLINE — GitHub Pages

This is the **character + walking animation test** for DUSTLINE.

## Play

After GitHub Pages is enabled on the `main` branch at the repository root, the game is available at:

`https://YOUR_GITHUB_USERNAME.github.io/dustline/`

The 3D engine loads from a public CDN, so an internet connection is required.

## Development

- `index.html` — game code, UI and gameplay. Update this file for most improvements.
- `assets/character.fbx` — user's character mesh and textures.
- `assets/walking.fbx` — user's walking animation.
- `assets/handgun.png` — user's handgun view.
- `.nojekyll` — GitHub Pages static hosting marker.

The game fetches FBX assets over HTTP(S), so use GitHub Pages or another local HTTP server, not the file:// protocol.

## Publishing

In the GitHub repository, select Settings > Pages > Deploy from a branch > main > /(root) > Save.
Changes committed to `main` automatically redeploy the game to the same URL.