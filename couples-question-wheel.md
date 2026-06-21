# Couples Question Wheel — Build Spec

A single-page web app: a spinning wheel of question **categories**. The user clicks **Spin**, the wheel spins, lands on a category, and a question from that category pops up. The goal is to spark curiosity, intentionality, and closeness between two people.

---

## 1. Concept & Purpose

This is a conversation tool for couples. The wheel itself holds the **categories** (Hot & Spicy, Heart to Heart, etc.). When it stops on a category, the app surfaces one random question from that category's bank.

The questions are designed to:
- Provoke curiosity and genuine thought
- Encourage intentional, honest conversation
- Help two people understand each other more deeply — across moods, from playful to vulnerable

Keep the tone warm, inviting, and a little romantic. Never clinical.

---

## 2. How It Works (UX Flow)

0. **House rules card (first load only).** Before the wheel is usable, show a brief intro card that sets the tone. Three short lines plus a **Begin** button:
   - *Be honest — there are no wrong answers.*
   - *No judgment. This is a safe space for both of you.*
   - *You can pass on any question, anytime.*

   This is what turns the page from a trivia toy into something that gives both people permission to go deep. Dismissing it reveals the wheel. (Don't show it again for the rest of the session.)

1. Page shows the wheel (8 colored category segments), a fixed pointer at the top, and a big **Spin** button.
2. User taps **Spin** → the wheel spins with realistic easing (fast start, slow finish) and lands on a random segment.
3. When it stops, a card animates in showing:
   - The category name + emoji
   - **A "whose turn" line** at the top of the card that alternates each time — e.g. *"You answer first"* → next card *"Your partner answers first"* → and so on. This makes it a shared act instead of one person quizzing the other. (Both people answer; the line just sets who goes first.)
   - One random question from that category
4. Below the question card, three small actions:
   - **Pass** → judgment-free skip; pulls a new question from the *same* category without making it feel like flinching
   - **Another question** → also pulls a new question from the same category (no re-spin)
   - **Spin again** → resets and spins the wheel

   *(Pass and Another question behave the same mechanically — both draw a fresh question from the current category. They're separated so that skipping something that hits a nerve feels normal and intentional, not like avoidance.)*
5. **No-repeat logic:** within a session, don't repeat a question from a category until that category's bank has been exhausted, then reshuffle.

Optional nice-to-haves (only if easy):
- A subtle tick sound as the wheel passes each segment.
- A small "spice meter" note on the Hot & Spicy card so it's clear that category leans flirty.

---

## 3. The Categories (the wheel segments)

| # | Category | Emoji | Vibe | Color |
|---|----------|-------|------|-------|
| 1 | Hot & Spicy | 🔥 | Flirty, sensual, romantic (tasteful) | `#E8455F` |
| 2 | Heart to Heart | 💗 | Emotionally intimate, tender | `#F4789B` |
| 3 | The Deep End | 🧠 | Therapy / psychology / growth | `#6C5CE7` |
| 4 | Just for Fun | 🎈 | Silly, playful, light | `#FDB44B` |
| 5 | Get to Know You | 🌱 | Personal, curiosity | `#2ECC8F` |
| 6 | This or That | ⚖️ | Quick preference picks | `#4AA8E0` |
| 7 | Dreams & Future | 🌅 | Hopes, plans, vision together | `#FF8C5A` |
| 8 | Memory Lane | 📸 | Nostalgia, shared history | `#B084CC` |

**256 questions total** (32 per category).

---

## 4. Design Direction

- **Single self-contained `index.html`** — inline CSS + JS, no build step, no external libraries required (a small confetti or sound lib is optional). Mobile-first; it should feel great on a phone held between two people.
- **Wheel:** SVG or `<canvas>`, 8 equal segments using the colors above, each labeled with its emoji (and short name if it fits). A clear pointer/arrow at the top center.
- **Palette:** warm, soft background (e.g. a gentle gradient like `#FFF5F2 → #FDEBF2`), rounded corners, generous spacing, one friendly display font for headings (e.g. a rounded sans like Poppins/Quicksand) and a clean readable font for questions.
- **Motion:** the spin should ease out over ~4–5 seconds and land convincingly. The question card should fade/scale in.
- **Accessibility:** high contrast text on cards, large tap targets, works with keyboard (Enter/Space triggers Spin).
- **Respect `prefers-reduced-motion`:** if the user's OS has reduce-motion enabled, skip the long spin animation — instead resolve the landed category quickly (a brief fade or a ~0.4s settle) so no one gets queasy. Same outcome, no whirling.
- **Spin mechanic:** pick a random target segment, then animate `rotation = (fullSpins * 360) + offsetToLandOnSegment`, using a CSS cubic-bezier ease-out or JS easing. Resolve which segment is under the pointer when motion ends.

---

## 5. Technical Notes for the Build

- Track `usedQuestions` per category (e.g. an index array you shuffle and pop from). When empty, reshuffle the full list.
- Keep all question data in the `CATEGORIES` array below — it's the single source of truth. Wheel segments map 1:1 to array order.
- Landing math: with 8 segments, each spans 45°. After the spin, compute the segment the pointer points to and show a question from `CATEGORIES[thatIndex]`.
- No backend, no storage needed. Everything runs client-side in one file.

---

## 6. Question Bank (drop-in data)

Paste this directly into the page's `<script>`. It's valid JS — strings use double quotes, apostrophes are safe inside them.

```js
const CATEGORIES = [
  {
    name: "Hot & Spicy",
    emoji: "🔥",
    color: "#E8455F",
    questions: [
      "What's the first thing you noticed about me physically?",
      "What's something I do that you find irresistibly attractive?",
      "Where's the most adventurous place you'd want to kiss me?",
      "What outfit of mine do you love seeing me in the most?",
      "Describe your idea of a perfect romantic night, start to finish.",
      "What's a fantasy you've been a little shy to bring up?",
      "What's your favorite way for me to flirt with you?",
      "What's the most romantic moment we've shared so far?",
      "What's a small gesture that instantly turns up the heat for you?",
      "If we had the house to ourselves for 24 hours, how would you want to spend it?",
      "What's something you'd love to try together that we haven't yet?",
      "What part of your day do you most look forward to with me?",
      "What's your favorite memory of us being close?",
      "What song instantly puts you in a romantic mood?",
      "What's the most attractive non-physical thing about me?",
      "How do you most like to be pursued?",
      "What's a compliment about your body you've always wanted to hear?",
      "What's your love language when the lights go down?",
      "On a getaway, where would you most want to be alone with me?",
      "What's something flirty you wish I did more often?",
      "What's your favorite kind of kiss?",
      "When have you caught yourself thinking about me at an inconvenient time?",
      "What makes you feel most desired?",
      "What's the boldest thing you'd whisper to me in a crowded room?",
      "What's your idea of the perfect slow dance?",
      "What's something about our chemistry you can't quite explain?",
      "What's a look I give you that you secretly love?",
      "If you could relive one passionate moment with me, which one?",
      "What's the most spontaneous romantic thing you'd want to do?",
      "What's a tiny touch that means a lot to you?",
      "What detail about me tends to stick in your memory?",
      "What's something that always reignites the spark for you?"
    ]
  },
  {
    name: "Heart to Heart",
    emoji: "💗",
    color: "#F4789B",
    questions: [
      "When do you feel most loved by me?",
      "What's something you've never told me but want to?",
      "When have you felt closest to me?",
      "What does feeling safe with someone mean to you?",
      "What's a fear you have about us that you'd like to share?",
      "What part of yourself do you hide from most people but show me?",
      "What do I do that makes you feel truly seen?",
      "When was the last time I made you feel proud to be with me?",
      "What's something you need more of from me emotionally?",
      "What's a moment you felt vulnerable with me and were glad you were?",
      "What does 'home' feel like to you, and am I part of it?",
      "What's a way I've helped you grow?",
      "What's something about us you're grateful for but rarely say out loud?",
      "When you're hurting, how do you most want me to show up?",
      "What's a wound from your past that still shapes how you love?",
      "What do you hope I never forget about you?",
      "What's the bravest thing you've ever done for love?",
      "What makes you feel emotionally distant, and how can I tell?",
      "What's a part of our relationship you wish we protected more?",
      "When did you first feel like you could fully be yourself with me?",
      "What's something kind I said once that you still hold onto?",
      "What does forgiveness look like to you?",
      "What's a need of yours you find hard to ask for?",
      "How do you most want to be comforted on your hardest days?",
      "What's something about me you admire but don't say enough?",
      "When do you feel most disconnected from me, and what helps?",
      "What does emotional intimacy mean to you?",
      "What's a dream you've been afraid to share with me?",
      "What would make you feel more cherished day to day?",
      "What's something you've forgiven me for that I may not even know about?",
      "When have you felt most understood by me?",
      "What do you most want us to be able to talk about more openly?"
    ]
  },
  {
    name: "The Deep End",
    emoji: "🧠",
    color: "#6C5CE7",
    questions: [
      "What's a pattern from your family you don't want to repeat?",
      "What does a healthy disagreement look like to you?",
      "How do you know you're stressed before you say anything?",
      "What's a belief about relationships you absorbed growing up?",
      "What tends to trigger a defensive reaction in you, and why?",
      "How did your parents handle conflict, and how did it affect you?",
      "What do you do when you feel criticized?",
      "What's a story you tell yourself about your own worth?",
      "When you shut down, what's usually going on underneath?",
      "What does 'being a good partner' really mean to you?",
      "What's something you're working on in yourself right now?",
      "How do you tend to react when you feel ignored or left out?",
      "What's a fear about yourself you'd rather I not see?",
      "What helps you feel grounded when emotions run high?",
      "What does trust have to look like for you to feel secure?",
      "What's a way you've changed since we got together?",
      "When do you feel most like your true self?",
      "What's a recurring argument we have, and what do you think it's really about?",
      "How do you most want us to repair after a fight?",
      "What's an expectation you didn't realize you had until it wasn't met?",
      "What do you need in order to feel respected?",
      "How do you process disappointment?",
      "What does emotional safety require from me?",
      "What's a value of yours you'd never compromise on?",
      "When you imagine us five years from now, what worries you?",
      "What part of your childhood still influences your reactions today?",
      "How do you want us to handle the differences in how we were raised?",
      "What's a hard truth about yourself you've come to accept?",
      "What does 'doing the work' in a relationship mean to you?",
      "What's something you wish you understood better about yourself?",
      "How do you want us to grow as a couple this year?",
      "What would help you feel more like a team with me?"
    ]
  },
  {
    name: "Just for Fun",
    emoji: "🎈",
    color: "#FDB44B",
    questions: [
      "If we were a duo with a team name, what would it be?",
      "What's the weirdest thing you'd do if you were invisible for a day?",
      "If I were an animal, which one would I be?",
      "What's the most ridiculous argument we've ever had?",
      "If we got matching tattoos as a joke, what would they be?",
      "What's a totally useless talent you secretly have?",
      "If our life were a TV show, what genre would it be?",
      "What's the silliest nickname you'd give me?",
      "If you could read my mind for one hour, when would you choose?",
      "What's the most embarrassing song on your playlist?",
      "If we entered a competition together, which one would we win?",
      "What's a food combination you love that grosses people out?",
      "If we swapped bodies for a day, what's the first thing you'd do?",
      "What's your go-to karaoke song?",
      "If aliens landed and asked you to explain 'us,' what would you say?",
      "What's the dumbest way you've ever injured yourself?",
      "If we opened a business together, what would it be?",
      "What's a fictional couple we remind you of?",
      "If you could give me one superpower, what would it be?",
      "What's the worst fashion phase you ever went through?",
      "If our relationship had a theme song, what would it be?",
      "What's something silly that always makes you laugh?",
      "If we were stranded on an island, what's your survival role?",
      "What's the most childish thing you still enjoy?",
      "If you had to describe me in three emojis, which ones?",
      "What's a gentle prank you'd love to pull on me?",
      "If we had a mascot, what would it be?",
      "What's the strangest dream you've had about me?",
      "If you could rename me, what would my new name be?",
      "What's a hill you'll die on that doesn't actually matter?",
      "If our love had a flavor, what would it taste like?",
      "What's the funniest thing that's happened on a date with me?"
    ]
  },
  {
    name: "Get to Know You",
    emoji: "🌱",
    color: "#2ECC8F",
    questions: [
      "What's a small thing that made you happy this week?",
      "What did you want to be when you grew up?",
      "What hobby would you pick up if you had unlimited time?",
      "Who has influenced you the most in life?",
      "What's something you're proud of that you rarely talk about?",
      "What's your most-used app, and what does that say about you?",
      "What's a book, show, or movie that changed how you think?",
      "What's something you believed as a kid that you've since outgrown?",
      "What's your ideal way to spend a day completely alone?",
      "What's a skill you've always wanted to learn?",
      "What's the best advice anyone ever gave you?",
      "What's a tiny ritual that makes your day better?",
      "Who in your life knows you best, and why?",
      "What's something on your bucket list right now?",
      "What's a place that feels like it belongs to you?",
      "What do people misunderstand about you most?",
      "What's a cause or issue you genuinely care about?",
      "What's the most spontaneous thing you've ever done?",
      "What's a compliment you received that stuck with you?",
      "What's something you collect or have a soft spot for?",
      "What's your relationship with your family like these days?",
      "What's a moment you realized you were growing up?",
      "What's something that always recharges you?",
      "What's a fear you've mostly overcome?",
      "What's the proudest moment of your life so far?",
      "What's a question you wish people asked you more?",
      "What's something you're curious about lately?",
      "What's a friendship that shaped who you are?",
      "What's your favorite way to be taken care of when you're sick?",
      "What's a part of your routine you'd never give up?",
      "What's something you've changed your mind about recently?",
      "What makes you feel most like yourself?"
    ]
  },
  {
    name: "This or That",
    emoji: "⚖️",
    color: "#4AA8E0",
    questions: [
      "Beach vacation or mountain getaway?",
      "Big party or quiet night in?",
      "Early bird or night owl?",
      "Sweet or savory?",
      "Texting or calling?",
      "Planner or go-with-the-flow?",
      "City life or countryside?",
      "Comedy or thriller?",
      "Coffee or tea?",
      "Save it or spend it?",
      "Window seat or aisle seat?",
      "Sunrise or sunset?",
      "Spontaneous road trip or carefully planned itinerary?",
      "Cook at home or eat out?",
      "Books or podcasts?",
      "Dogs or cats?",
      "Summer or winter?",
      "Loud and social or calm and cozy?",
      "Lead the dance or follow?",
      "Surprise gifts or pick your own?",
      "Stay in your hometown or move somewhere new?",
      "Mornings together or late nights together?",
      "Big dreams or steady comfort?",
      "Handwritten note or long voice message?",
      "Adventurous food or stick to your favorites?",
      "Concert or museum?",
      "One big trip a year or lots of little ones?",
      "Talk it out immediately or take space first?",
      "Matching everything or your own style?",
      "Live in the moment or plan for the future?",
      "Movie at the theater or couch and a blanket?",
      "Be the giver or the receiver of surprises?"
    ]
  },
  {
    name: "Dreams & Future",
    emoji: "🌅",
    color: "#FF8C5A",
    questions: [
      "Where do you picture us living in ten years?",
      "What's a dream you want us to chase together?",
      "What does your ideal ordinary day look like five years from now?",
      "What tradition do you want us to start?",
      "What's a place you want us to see before we're old?",
      "What kind of home do you dream of making together?",
      "What do you hope people say about our relationship?",
      "What's a goal you want to hit this year, and how can I help?",
      "If money were no object, what would our life look like?",
      "What do you want to be doing for work in the future?",
      "What does growing old together look like to you?",
      "What's a milestone you're excited for us to reach?",
      "What do you hope we never stop doing?",
      "What kind of parents (or pet-parents) do you imagine us being?",
      "What's a fear about the future you'd like us to face as a team?",
      "What adventure should be next on our list?",
      "What do you want our home to feel like to guests?",
      "What's something you want to build or create in your lifetime?",
      "What does success look like for you, beyond money?",
      "What's a skill you want us to learn together?",
      "What do you want our holidays to look like someday?",
      "What's a promise you'd want us to keep no matter what?",
      "Where do you want to celebrate our next big anniversary?",
      "What legacy do you want to leave behind?",
      "What's a version of us you're excited to become?",
      "What do you hope stays exactly the same about us?",
      "What's one big risk you'd take if I had your back?",
      "What does retirement look like in your daydreams?",
      "What's a cause we could champion together?",
      "What do you want to teach the people who come after us?",
      "If we wrote a five-year plan tonight, what's line one?",
      "What future moment do you most look forward to sharing with me?"
    ]
  },
  {
    name: "Memory Lane",
    emoji: "📸",
    color: "#B084CC",
    questions: [
      "What's your favorite memory of our first date?",
      "When did you realize you were falling for me?",
      "What's a moment with me you replay often?",
      "What's the first gift I gave you that you still think about?",
      "What were your first impressions of me?",
      "What's a trip or outing of ours you'd relive in a heartbeat?",
      "What's something I did early on that won you over?",
      "What's the funniest memory we share?",
      "When did you first feel like 'we' were a real thing?",
      "What's a hard time we got through that made us stronger?",
      "What song reminds you of an early moment with us?",
      "What's a tiny everyday memory of mine you treasure?",
      "What's the most nervous I ever made you?",
      "What did you tell your friends about me at the start?",
      "What's a place that holds a special memory for us?",
      "What's the best surprise I ever gave you?",
      "When did you know you wanted this to last?",
      "What's a photo of us that's your favorite, and why?",
      "What's something about our beginning you miss?",
      "What's the most thoughtful thing I've ever done for you?",
      "What moment made you feel truly chosen by me?",
      "What's a memory that makes you laugh every time?",
      "What was the first inside joke we created?",
      "What's a meal or food tied to a memory of us?",
      "What's a milestone of ours you're most proud of?",
      "What's something we did for the first time together?",
      "What's the sweetest text I ever sent you?",
      "When did you feel most supported by me?",
      "What's an ordinary day with me you'd love to bottle up?",
      "What's a memory you hope our future selves never forget?",
      "What's the moment you knew I was different from anyone else?",
      "If you made a highlight reel of us, what's the opening scene?"
    ]
  }
];
```

---

## 7. Build Prompt (paste into Claude Code)

> Build a single self-contained `index.html` couples question wheel using the `CATEGORIES` data from this spec. Requirements:
> - **House rules intro card on first load** with three lines (be honest / no judgment / you can pass anytime) and a **Begin** button that dismisses it to reveal the wheel. Don't show it again that session.
> - An 8-segment spinning wheel (SVG or canvas), one segment per category, using each category's `color` and `emoji`. Fixed pointer at top center.
> - A large **Spin** button. On spin, animate the wheel with an ease-out over ~4–5s and land on a random segment. Determine the landed category from the pointer position.
> - When it stops, show an animated card with: a **"whose turn" line that alternates each card** (e.g. "You answer first" / "Your partner answers first"), the category name + emoji, and one random question from that category.
> - Card actions: **Pass**, **Another question** (both draw a fresh question from the same category, no re-spin), and **Spin again**.
> - No-repeat logic per category until its bank is exhausted, then reshuffle.
> - Mobile-first, warm romantic styling (soft gradient background, rounded cards, friendly heading font). Keyboard accessible (Space/Enter spins).
> - **Honor `prefers-reduced-motion`:** skip the long spin and resolve quickly with a short fade when reduce-motion is on.
> - No backend, no external dependencies required (confetti/sound optional). Everything in one HTML file.
> - Header text: a short title like "Closer — a question wheel for two" and one line explaining the purpose.

---

### Notes
- The **Hot & Spicy** bank is flirty/romantic but tasteful — safe to use in mixed company. If you ever want an "after dark" toggle that swaps in a spicier bank, that's an easy add-on (just a second questions array and a switch).
- Want me to also generate the finished `index.html` from this spec, or adapt it to React/Vite to match your usual build setup? Just say the word.
