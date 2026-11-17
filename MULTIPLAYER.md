# DUSTLINE — 1v1 Online Mode

DUSTLINE now supports two-browser 1v1 duels with a short room code. Existing single-player gameplay remains available.

## Create and join a game

1. Both players open https://dustline-gsv.pages.dev/ and allow all game assets to finish loading.
2. **Host:** select **CREATE LOBBY** → **1V1 • FIRST TO 3**. The host automatically occupies player one. Share the displayed six-character code.
3. **Guest:** select **JOIN LOBBY**, enter the code, and press **JOIN 1V1**. Both browsers must stay connected to the internet.
4. When the guest connects, **START MATCH** becomes enabled for the host.
5. Host presses **START MATCH**. The two players enter the arena at opposite ends.
6. Shoot the opponent; after each elimination both players respawn. **First to three eliminations wins**. Both browsers show **YOU WON.** or **YOU LOST.** and return to the main menu after five seconds.

The remote player is rendered with the regular enemy character/skeleton, Mixamo walking cycle, final-frame guard pose, real shooting animation and Sketchfab pistol GLB. There are **no special aim or strafe animations**. Enemy AI is disabled in this mode. The P/O cheats are available only in single-player.

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
