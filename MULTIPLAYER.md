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
