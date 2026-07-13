Chalk & Cart — Voice Command Shopping Assistant

A voice-driven grocery list app. Say a command like "add two apples" or
"find toothpaste under 5 dollars," confirm the parsed item on a product
card, and it's added to your running list.

Try it

Open index.html in Chrome (voice recognition needs Chrome/Chromium's
Web Speech API — other browsers fall back to the text input automatically).

How it works


Voice input —--- the browser's built-in SpeechRecognition API
converts speech to text, no external API key needed. A language
dropdown (English / Spanish / Hindi) sets recognition.lang for basic
multilingual support.


Command parsing —---- a lightweight rule-based parser extracts intent
(add / remove / search), quantity (digits or number words like "two"),
and item name from the transcript, then matches it against a small
product catalog (name, price, category).


Smart suggestions — ---substitutes and "in season" tags come from a
small hardcoded lookup table rather than a trained model. This is a
deliberate scope cut: the goal was to demonstrate the full user flow
(recognize → suggest → confirm → cart) rather than build a real
recommendation engine in the time available.

Cart —----- items collect in a receipt-style panel with running total,
editable quantity/price before adding, and one-tap remove.
Fallback — a text input covers browsers without mic support and
makes the app testable without a working microphone.


Guided mode ("Cook with me") — a small conversational layer on top
of the same input pipeline. The assistant asks what you're making,
looks it up in a hardcoded 30-recipe dataset (recipe → ingredient
list), then walks through the ingredients one at a time. Every
ingredient shows a brand + rating picker before it's added to cart.
Bot lines are read aloud with the browser's built-in SpeechSynthesis
API (free, no key).


Real Hindi support — the language dropdown doesn't just tell the
browser to transcribe Hindi (that part was free); it also carries a
hand-built vocabulary (HINDI_ITEMS, HINDI_ADD_WORDS,
HINDI_REMOVE_WORDS, HINDI_SEARCH_WORDS, HINDI_NUM_WORDS) so
commands like "दूध चाहिए" or "दो सेब जोड़ो" are actually understood, not
just transcribed. This is a rule-based word-list, not a translation
model, so it only covers vocabulary already in the catalog — a real
product would swap this for a proper NLU/translation model.
Purchase-history suggestions — every add (voice, guided mode, or
tap) is tallied in an in-memory purchaseHistory counter. Once an
item's been bought twice and isn't currently in the cart, it surfaces
in a "Based on your shopping" row ("Running low on milk?"). This
resets on page reload since there's no backend; a real version would
persist counts per-user in Firestore.


Data

The recipe and brand datasets are hand-authored (10 recipes, 4 multi-brand
ingredients with realistic Indian retail pricing in ₹) rather than pulled
from Kaggle's recipe-ingredients-dataset. That dataset is from a cuisine-
classification competition — each entry is {cuisine, ingredients} with no
recipe name — so it doesn't have the "name → ingredient list" structure this
feature needs. It's a better fit for expanding the product vocabulary than
for the guided-recipe flow itself.

What I'd do with more time

Checkouts, Payment option
Swap the rule-based parser for an LLM call (e.g. free-tier Groq/Gemini)
for more flexible phrasing
Persist the cart and purchase history (Firestore) to power real
"you're running low on X" suggestions
Expand the product catalog and add real translation for multilingual
commands rather than just a language flag


Deploying

This is a static single-file app — no build step. Easiest options:


Firebase Hosting: firebase init hosting → point public dir at this
folder → firebase deploy
GitHub Pages: push this repo, enable Pages on the main branch
Netlify/Vercel: drag-and-drop deploy of this folder
