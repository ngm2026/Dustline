# DUSTLINE — 1v1 Online Mode

DUSTLINE now supports two-browser 1v1 duels with a short room code. Existing single-player gameplay remains available.

## Create and join a game

1. Both players open https://dustline-gsv.pages.dev/ and allow all game assets to finish loading.
2. **Host:** select **CREATE LOBBY** → **1V1 • FIRST TO 3**. The host automatically occupies player one. Share the displayed six-character code.
3. **Guest:** select **JOIN LOBBY**, enter the code, and press **JOIN 1V1**. Both browsers must stay connected to the internet.
4. When the guest connects, **START MATCH** becomes enabled for the host.
5. Host presses **START MATCH**. The two players enter the arena at opposite ends.
6. Shoot the opponent; after each elimination both players respawn. **First to three eliminations wins**. Both browsers show **YOU WON.** or **YOU LOST.** and return to the main menu after five seconds.

The remote player is rendered with the regular enemy character/skeleton, Mixamo walking cycle, permanently raised pistol pose from the first shooting-animation frame, shot-triggered recoil and Sketchfab pistol GLB. There are **no special aim or strafe animations**. Enemy AI is disabled in this mode. The P/O cheats are available only in single-player.

## Networking

This is a lightweight **WebRTC peer-to-peer 1v1 prototype** powered by PeerJS 1.5.5 and its default free public PeerServer Cloud signaling service. DUSTLINE remains a static Cloudflare Pages site; no secret API key or dedicated backend is required. The browsers exchange position updates (~20 times/second), shot events, health, and scores.

- The **host** decides health, eliminations, respawn rounds, and victory. The joining player reports shots and locally detected hits.
- The match is **casual**, not anti-cheat hardened or authoritative enough for tournaments.
- Both players must be online simultaneously. Room codes are ephemeral and disappear when the host leaves.
- Peer-to-peer data channels can fail on restricted corporate, school, hotel, or mobile networks because the free service does not guarantee TURN relaying. If the lobby will not connect, try a different network, allow WebRTC traffic, and refresh.
- PeerJS Cloud is a shared third-party signaling service, not a guaranteed production backend. To support large audiences or strict reliability, deploy a dedicated signaling server, TURN relay and server-authoritative game service.

## UI / Controls

- **CREATE LOBBY** and **JOIN LOBBY** appear under **SINGLE PLAYER** on the main menu.
- The **JOIN LOBBY** screen retains DUSTLINE's original background and includes the code entry field.
- The host can copy the room code, see when a player connects, and start the match.
- **WASD** moves; **mouse** aims; **left-click** shoots; **right-click** zooms; **R** reloads; **Esc** opens the pause menu. The online match continues while the pause menu is open. Use **LEAVE 1V1** to disconnect.

## Testing

The lobby handshake, code validation, host-start gating, round resets, first-to-three score logic, winner/loser messages, and five-second return were checked with mocked two-browser connections and simulated gameplay in the code authoring environment.

A live end-to-end test on two devices, including network traversal and rendering/animation, **has not been performed** here and is still recommended before calling this production ready.

## Three-stage jump animations (Nov 2026)

The original uploaded **W1_Stand_Aim_Jump_Start_IPC.fbx**, **W1_Stand_Aim_Jump_Air_IPC.fbx**, and **W1_Stand_Aim_Jump_End_IPC.fbx** have been sampled and **retargeted from their MotusMan skeleton** to DUSTLINE's existing Mixamo character. The game downloads compact source-derived poses from `assets/animations/jump_poses.js`, so no extra manual upload of the large FBX files is needed.

- **Start**: first ~0.16 seconds of take-off, based on eight pose samples from the 0.5-second source clip
- **Air**: ~0.61 seconds, based on 13 samples from the 2.0-second source clip
- **End**: ~0.19 seconds after landing, based on nine samples from the 0.93-second source clip
- The game synchronizes each player's jump height in the existing ~20 Hz WebRTC updates; the other player's model rises and lands instead of standing still on the ground
- The 9mm pistol retains its grip position and follows the right hand's animated **rotation**, including during take-off, airborne movement, landing, and gunfire
- The normal walking animation is paused during a jump; gunfire can still temporarily blend over the jump pose
- The jump timings can be adjusted in `JUMP_PHASE_DURATION` in `index.html`, and the actual vertical arc follows the original 7.5 initial velocity and 19.5 gravity physics

**Important:** Because the supplied jump files use a different skeleton, this is a joint-rotation retarget and resampled approximation, not full original FBX playback. The transfer and timing passed code/data checks, but the final animation and grip alignment still need a two-browser visual test.

## Idle and stationary left/right turns (MoCap Online Free Pistol Animation Pack)

The following user-supplied binary FBXs were parsed, sampled, and retargeted into `assets/animations/stand_poses.js` without requiring an additional manual GitHub upload:

- `W1_Stand_Aim_Idle_IPC.fbx`: 2-second loop, 21 resampled pose frames; shown when a remote multiplayer player stands still
- `W1_Stand_Aim_Turn_In_Place_L_Loop_IPC.fbx`: 0.667-second loop, 13 resampled pose frames; shown when the remote player stands still and turns left using their mouse or horizontal-look controls
- `W1_Stand_Aim_Turn_In_Place_R_Loop_IPC.fbx`: 0.667-second loop, 13 resampled pose frames; same for turning right

All three use the uploaded **MotusMan** skeleton. Their 22 mapped body-joint rotations were sampled **relative to the common starting idle pose** and blended onto the existing Mixamo body, so the current character and gun models are not replaced. The standing turn poses keep the hips, legs, and feet planted while the body rotates to face the networked yaw; upper-body and wrist articulation remains animated. Smooth blending is applied when stopping, beginning to turn, and resuming movement. Both idle and turns preserve the player's real textured pistol: the gun's grip remains at the animated hand, and its orientation follows the wrist.

**Animation priority:** jumping (Start / Air / End) takes precedence, then normal walking while moving, then idle / turning while stationary. The existing firing animation overlays all of those states when the player shoots.

Turn direction is detected from changes in local camera yaw, encoded in the existing 20 Hz WebRTC player-state messages. Positional movement and facing are still controlled by the networked state, not by root motion in the animation FBXs. These clips are **source-derived resampled retargets**, not byte-for-byte FBX animation playback.

MoCap Online is thanked in DUSTLINE's **Settings → Thank You / Credits** screen for its **Free Pistol Animation Pack**. Source animation and texture alignment should still be reviewed in a live two-player visual test.

## Always-armed PvP opponents and shot recoil

During multiplayer 1v1, the remote opponent **always holds their pistol up**. Their base upper-body/hand pose is the ready-to-fire **first frame of the actual Shooting_Animation.fbx**, not the Drawing_Gun_Animation.fbx sequence. There is no multiplayer draw or holster transition. The existing hand-mounted GLB remains visible during walking, idle, stationary turns, and jumps. When the player fires, the full firing clip blends over the held-gun pose for recoil and smoothly returns to the same ready position. Five-bot single-player animation behavior is unchanged. The finished appearance still benefits from a live two-browser visual check.

## Pistol attachment — no frame-by-frame stutter

The textured GLB pistol is now **parented to the Mixamo right-hand bone** using Three.js `Object3D.attach`, rather than following through world-space position/quaternion assignments every render tick. The pistol's original grip alignment, orientation, and world scale are calibrated once at character creation, after the PvP avatar is placed and facing its initial direction. The joint hierarchy carries the gun during walking, stationary turns, jumps, and shot recoil. The same attachment and safe cleanup also apply to the solo bot models. This is an animation hierarchy fix; a live two-browser check is still advised to verify the final visual alignment.
