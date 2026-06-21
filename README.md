# Closer — a question wheel for two

A single-page web app: a spinning wheel of question **categories** for couples. Tap **Spin**, the wheel eases to a stop on a category, and a random question from that category pops up — designed to spark curiosity, intentionality, and closeness.

🔗 **[Open `index.html`](index.html)** in any browser — no build step, no server, no dependencies.

## Features

- **House-rules intro** on first load (be honest · no judgment · you can pass anytime).
- **8 category segments** — Hot & Spicy, Heart to Heart, The Deep End, Just for Fun, Get to Know You, This or That, Dreams & Future, Memory Lane.
- **256 questions** (32 per category) with **no-repeat** logic until a category's bank is exhausted, then a reshuffle.
- **Alternating "whose turn" line** so each question fairly rotates who answers first.
- **Pass / Another question / Spin again** card actions.
- **Always-upright labels** — each label counter-rotates as the wheel spins, so wherever it lands the text never tilts or flips upside down.
- Mobile-first, warm romantic styling; keyboard accessible (Space/Enter to spin); honors `prefers-reduced-motion`.
- Optional tick sound + landing chime (toggle), confetti, and a 🌶️ spice meter on Hot & Spicy.

## Tech notes

- Everything lives in one self-contained [`index.html`](index.html) (inline CSS + JS).
- The wheel is SVG; the spin is driven by a CSS transition and resolves via `transitionend` with a guaranteed fallback, so it never hangs even if the tab is backgrounded mid-spin.
- `couples-question-wheel.md` is the original build spec and the source of truth for the question bank.
