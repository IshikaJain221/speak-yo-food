# Chalk & Cart — Voice Command Shopping Assistant

A voice-driven grocery list app. Say a command like "add two apples" or
"find toothpaste under 5 dollars," confirm the parsed item on a product
card, and it's added to your running list.

## Try it
Open `index.html` in Chrome (voice recognition needs Chrome/Chromium's
Web Speech API — other browsers fall back to the text input automatically).

## How it works
- **Voice input** — the browser's built-in `SpeechRecognition` API
  converts speech to text, no external API key needed. A language
  dropdown (English / Spanish / Hindi) sets `recognition.lang` for basic
  multilingual support.
- **Command parsing** — a lightweight rule-based parser extracts intent
  (add / remove / search), quantity (digits or number words like "two"),
  and item name from the transcript, then matches it against a small
  product catalog (name, price, category).
- **Smart suggestions** — substitutes and "in season" tags come from a
  small hardcoded lookup table rather than a trained model. This is a
  deliberate scope cut: the goal was to demonstrate the full user flow
  (recognize → suggest → confirm → cart) rather than build a real
  recommendation engine in the time available.
- **Cart** — items collect in a receipt-style panel with running total,
  editable quantity/price before adding, and one-tap remove.
- **Fallback** — a text input covers browsers without mic support and
  makes the app testable without a working microphone.
- **Guided mode ("Cook with me")** — a small conversational layer on top
  of the same input pipeline. The assistant asks what you're making,
  looks it up in a hardcoded 10-recipe dataset (recipe → ingredient
  list), then walks through the ingredients one at a time. If an
  ingredient has more than one brand (bread, milk), it pauses and shows
  brand cards — pick by tap or by saying the brand name — before moving
  to the next item. Bot lines are read aloud with the browser's built-in
  `SpeechSynthesis` API (free, no key), so it's a real two-way voice
  exchange, not just one-shot commands. This is implemented as a small
  state machine (`awaiting_recipe → collecting → choosing_brand → done`)
  layered over the existing parser — free mode and guided mode share the
  same product catalog and cart.

## What I'd do with more time
- Swap the rule-based parser for an LLM call (e.g. free-tier Groq/Gemini)
  for more flexible phrasing
- Persist the cart and purchase history (Firestore) to power real
  "you're running low on X" suggestions
- Expand the product catalog and add real translation for multilingual
  commands rather than just a language flag

## Deploying
This is a static single-file app — no build step. Easiest options:
- **Firebase Hosting**: `firebase init hosting` → point public dir at this
  folder → `firebase deploy`
- **GitHub Pages**: push this repo, enable Pages on the `main` branch
- **Netlify/Vercel**: drag-and-drop deploy of this folder
