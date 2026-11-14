# DUSTLINE enemy animation files

Put these two **real binary FBX files** in this folder:

- `Drawing_Gun_Animation.fbx` — original weapon-drawing animation, reversed for holstering
- `Shooting_Animation.fbx` — original shooting animation

Both have been simplified from the user's uploaded FBXs: the animation skeleton and keyframes remain, while embedded textures, meshes, skin and materials are removed.

The game already loads these paths in `index.html` and applies the upper-body motion to the original enemy rig. No separate fake gun object is required.

**Upload the .fbx files to this folder from the downloaded ZIP.** Their bytes cannot be pushed using the current GitHub text-edit connector.

If the FBXs aren't present, the game remains playable but uses no enemy draw/shoot character animations; the 1-second readiness and 5-second holster timing rules still apply.
