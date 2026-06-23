# Closer — a question wheel & game night for two

A single-page web app for couples. Spin a wheel of honest questions on its own — or play a two-player game where every hit, capture, or win pops the wheel up with a question for whoever's on the receiving end.

🔗 **[Open `index.html`](index.html)** in any browser — no build step, no server, no dependencies.

## Two ways to play

**🎡 Just the Wheel** — tap Spin, it eases to a stop on a category, and a random question appears. The "whose turn" line alternates so you take turns going first.

**🎮 Game night** — pick a game (enter your names first); the wheel triggers at the right moment, and **whoever's on the receiving end answers**:

| Game | When the wheel pops up |
|------|------------------------|
| **Tic-Tac-Toe** | Win a round → the loser answers |
| **Connect Four** | Connect four → the loser answers |
| **Checkers** | Capture a piece → the captured player answers |
| **Battleship** | Land a hit → the hit player answers (Russian fleet: 1×4, 2×3, 3×2, 4×1) |

Each game can be played two ways:

- **📱 Same phone** — pass one device back and forth (Battleship adds a "pass the phone" step so fleets stay private).
- **🌐 Online** — one of you taps **Create a game**, shares the link; the other opens it and you're connected on separate devices. Online play needs the page on a public URL (see *Hosting* below). Online Battleship needs no pass-the-phone — each phone only shows your own fleet.

## Features

- **House-rules intro** on first load (be honest · no judgment · you can pass anytime).
- **8 category segments**, **256 questions** (32 each), with **no-repeat** logic until a category's bank is exhausted, then a reshuffle.
- **Always-upright labels** — each label counter-rotates as the wheel spins, so wherever it lands the text never tilts or flips upside down.
- A reusable wheel: the same spin/question component powers solo mode and every game.
- Mobile-first, hot-seat (one shared phone); keyboard accessible (Space/Enter to spin); honors `prefers-reduced-motion`.
- Optional tick sound + landing chime (toggle), confetti, and a 🌶️ spice meter on Hot & Spicy.

## Online play & hosting

Online uses peer-to-peer **WebRTC via [PeerJS](https://peerjs.com)** (loaded from a CDN) — no accounts, keys, or backend. One device hosts and is authoritative; the perfect-information games (Tic-Tac-Toe, Connect Four, Checkers) replay moves in lockstep, while Battleship exchanges shots so fleets never leave their own device.

To share a link, the page needs a public HTTPS URL. The simplest options:

- **GitHub Pages** — Settings → Pages → deploy from the `main` branch; the link becomes `https://<user>.github.io/wheel-spin/`.
- Any static host (Vercel, Netlify, Cloudflare Pages) — just point it at this repo.

> P2P connects directly between browsers and works on most home/phone networks; it can occasionally fail on locked-down/corporate Wi-Fi (there's no relay/TURN server). Same-phone mode always works offline.

## Tech notes

- Everything lives in one self-contained [`index.html`](index.html) (inline CSS + JS; PeerJS is the only external script, used only for online play).
- The wheel is SVG; the spin is driven by a CSS transition and resolves via `transitionend` with a guaranteed fallback, so it never hangs even if the tab is backgrounded mid-spin.
- Simple screen router (Home → Wheel / Games → Game) with the wheel as a shared overlay any game can invoke; online, the spin RNG/question is chosen by the spinner and mirrored so both peers land identically.
- `couples-question-wheel.md` is the original build spec and the source of truth for the question bank.
