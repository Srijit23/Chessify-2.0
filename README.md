# SrijitChess - Online Multiplayer Chess Game

A real-time multiplayer chess game built with HTML, CSS, JavaScript, and WebSocket technology.

## Features

- Play chess against other players in real time
- Create and join game rooms
- Spectator mode
- Real-time move synchronization
- Guest lobby with an online-player list and direct challenges (test branch)
- Responsive design
- Practice mode against computer

## Tech Stack

- Frontend: HTML, CSS, JavaScript, Chess.js, custom Chessboard compatibility layer, Bootstrap
- Backend: Node.js, Express, WebSocket (`ws`)
- Frontend hosting can use Netlify; persistent WebSocket backend can use Render

## Local Development

1. Install dependencies: `npm install`
2. Start the WebSocket server: `npm start`
3. Open `http://localhost:3000/play.html`
4. The local page defaults to `ws://localhost:3000` when opened on localhost.

## Real-time guest lobby (test branch)

The `feature/realtime-player-lobby` branch adds guest display names, a live online-player list, direct challenges with a 30-second expiry, and manual room creation/joining. The frontend deliberately does **not** fall back to the existing live Render URL.

### Test without touching the live game

1. Create a **new** Render Web Service from this repository, using branch `feature/realtime-player-lobby`. Do not change the existing production service.
2. Set the service start command to `npm start`; Render supplies `PORT`. No database is required for the temporary lobby.
3. Copy the new service's URL and edit `lobby-config.js` to set `window.CHESSIFY_WS_URL = 'wss://YOUR-NEW-SERVICE.onrender.com';`.
4. Deploy the test frontend (for example, a separate Netlify site/branch deploy) from this branch. Keep the production Netlify site and production Render service unchanged.
5. Test in two separate browsers or an incognito window: enter two different names, challenge, accept, make moves, send chat, and test manual room creation/joining.

The online list and guest identities live in server memory and disappear when the server restarts. This is intentional for a temporary guest lobby. Render free instances may sleep after inactivity, so the first connection can take a little time.

### Security and compatibility notes

- The server assigns opaque player IDs; display names are not identity tokens.
- Challenges are validated by the server, expire after 30 seconds, and can only be accepted by the intended recipient.
- Chat messages are delivered to the opponent only and rendered as text, not HTML.
- `server.js` is the persistent WebSocket backend used by `npm start`. The legacy `api/socket.js` serverless handler is not the target for the Render test service.
- This is a guest lobby, not a login/account system. Display names are temporary and not verified.
