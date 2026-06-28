# ankihelper

Generate Anki `.apkg` decks from JSON (MCQ) or TSV (basic) card data.

_Part of the [Seya](https://github.com/tataang/Seya) study ecosystem._

---

## Installation

```bash
pip install ankihelper
```

Or for local development:

```bash
git clone https://github.com/tataang/AnkiHelper.git
cd AnkiHelper
pip install -e .
```

---

## CLI usage

```bash
# JSON — multiple-choice question cards
ankihelper "Cloud Computing" cards.json

# TSV — basic front/back cards
ankihelper "Calc 2 Formulas" formulas.tsv

# Explicit output path (recommended for subprocess callers)
ankihelper "Cloud Computing" cards.json --output /path/to/deck.apkg

# Override format detection
ankihelper "My Deck" cards.data --format tsv
```

On success the resolved `.apkg` path is printed to stdout (exit 0).  
On failure a human-readable message is printed to stderr (exit 1).

---

## Input formats

### JSON — MCQ cards

```json
{
  "cards": [
    {
      "question": "What does CPU stand for?",
      "choices": ["Central Processing Unit", "Core Power Unit", "Control Processing Unit"],
      "correct_answer": "Central Processing Unit",
      "explanation": "CPU stands for Central Processing Unit.",
      "tags": ["chapter-1"]
    }
  ]
}
```

`correct_answer` must be an element of `choices` — validated on parse.  
`tags` is optional.

### TSV — basic cards

Tab-delimited UTF-8. Optional header row (`front\tback`) is auto-detected and skipped.

```
front	back
What is spaced repetition?	A technique that spaces reviews over time.
What is active recall?	Actively retrieving information from memory.
```

---

## Library usage

```python
from pathlib import Path
from ankihelper import AnkiCardDeck, MCQCard, BasicCard

deck = AnkiCardDeck(deck_name="My Deck")

deck.add_card(MCQCard(
    question="What does RAM stand for?",
    choices=["Random Access Memory", "Read Access Module"],
    correct_answer="Random Access Memory",
    explanation="RAM is the primary short-term memory of a computer.",
    tags=["hardware"],
))

deck.add_card(BasicCard(
    front="What is a CPU?",
    back="The central processing unit — the brain of a computer.",
))

deck.save_deck(Path("output/my_deck.apkg"))
```

---

## Running tests

```bash
pip install pytest
pytest tests/
```

---

## What is Anki?

[Anki](https://apps.ankiweb.net/) is a free flashcard program that uses spaced repetition and active recall to maximise long-term retention. `ankihelper` generates `.apkg` files that can be imported directly into Anki.
