DUSTLINE — Simplified animation-only FBX files

File placement in GitHub repository ngm2026/Dustline:
  assets/animations/Drawing_Gun_Animation.fbx
  assets/animations/Shooting_Animation.fbx

These are real binary FBX animation files derived from your uploads. Each
keeps the Mixamo skeleton and original keyed animation curves but removes
embedded textures, skin, geometry and meshes. No extra gun mesh is added.

IMPORTANT: GitHub must receive both FBXs at those exact paths before the
website can play them. They are NOT already in the GitHub repository.

How to add them in GitHub:
1. Extract this ZIP onto your computer.
2. Open ngm2026/Dustline, browse to assets/animations.
3. Click Add file > Upload files, then upload the two .fbx files.
4. Commit changes. Cloudflare Pages will redeploy automatically.

DUSTLINE's updated index.html already loads these specific FBX paths and
uses their upper-body animation tracks on the existing enemy rig. A safe
no-animation fallback is used when either file is missing or invalid.
