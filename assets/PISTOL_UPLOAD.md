# Enemy pistol asset — one-file upload

The game code already loads the user's real downloaded, fully textured **`pistol.glb`** at this exact repository path:

```
ngm2026/Dustline/assets/pistol.glb
```

Upload the **GLB file itself** inside the existing `assets` folder on GitHub. **Do not** upload an enclosing folder or ZIP. The filename is case-sensitive and must be exactly `pistol.glb`.

Direct upload page: https://github.com/ngm2026/Dustline/upload/main/assets

Once uploaded and deployed on Cloudflare Pages, the game loads the original GLB once with Three.js `GLTFLoader`, reuses the meshes and embedded materials/textures for every enemy, aligns the grip with the animated right hand and points the barrel along the enemy's forward direction. The pistol is visible when an enemy is holding it or firing, but hidden during a draw animation. Existing shooting FBX animations are unchanged.

The gun loader degrades safely if the file is missing; gameplay still runs, but the pistol is not visible until the file is uploaded.

## Model credit

**9mm Pistol** by **TORI106** on Sketchfab.
- Original model: https://sketchfab.com/3d-models/9mm-pistol-43bc09f5aace4346a8a2b6e1580fc03f
- License: Creative Commons Attribution 4.0, https://creativecommons.org/licenses/by/4.0/

Imported into DUSTLINE and scaled/positioned for character animation; no replacement fake geometry is used.
