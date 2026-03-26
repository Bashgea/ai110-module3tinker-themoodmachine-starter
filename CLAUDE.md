# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run the rule-based mood analyzer (evaluation + interactive demo)
python main.py

# Run the ML model (trains on SAMPLE_POSTS, then interactive demo)
python ml_experiments.py
```

There is no test suite or linter configured for this project.

## Architecture

This is a CodePath AI lab (Module 3) with two parallel mood classification approaches operating on the same dataset:

**Data layer** (`dataset.py`): Single source of truth for `POSITIVE_WORDS`, `NEGATIVE_WORDS`, `SAMPLE_POSTS`, and `TRUE_LABELS`. Both models read from here. `SAMPLE_POSTS` and `TRUE_LABELS` must always have the same length.

**Rule-based model** (`mood_analyzer.py` → `main.py`): `MoodAnalyzer` class tokenizes text via `preprocess()`, scores it via `score_text()` (word matching against the word lists), and maps the score to a label via `predict_label()`. The `explain()` method is already implemented as a reference; `score_text()` and `predict_label()` have TODOs the student must implement.

**ML model** (`ml_experiments.py`): Uses scikit-learn `CountVectorizer` + `LogisticRegression` trained on `SAMPLE_POSTS`/`TRUE_LABELS`. Evaluation is always training accuracy (no held-out test set) because the dataset is tiny.

**Reflection** (`model_card.md`): Template the student fills out after experimenting with both models.

## Lab Context

The primary task for the student is to:
1. Implement `score_text()` and `predict_label()` in `MoodAnalyzer` (at minimum one modeling improvement beyond basic word matching)
2. Expand `SAMPLE_POSTS` and `TRUE_LABELS` with 5–10 more examples including slang, emojis, sarcasm, and ambiguous cases
3. Compare behavior of the rule-based vs. ML model and complete `model_card.md`

Labels used across the project: `"positive"`, `"negative"`, `"neutral"`, `"mixed"`. Labels returned by `predict_label()` must match those used in `TRUE_LABELS` for accuracy to be meaningful.
