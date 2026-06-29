# The Mood Machine

The Mood Machine is a simple text classifier that begins with a rule based approach and can optionally be extended with a small machine learning model. It tries to guess whether a short piece of text sounds **positive**, **negative**, **neutral**, or even **mixed** based on patterns in your data.

This lab gives you hands on experience with how basic systems work, where they break, and how different modeling choices affect fairness and accuracy. You will edit code, add data, run experiments, and write a short model card reflection.

---

## Repo Structure

```plaintext
├── dataset.py         # Starter word lists and example posts (you will expand these)
├── mood_analyzer.py   # Rule based classifier with TODOs to improve
├── main.py            # Runs the rule based model and interactive demo
├── ml_experiments.py  # (New) A tiny ML classifier using scikit-learn
├── model_card.md      # Template to fill out after experimenting
└── requirements.txt   # Dependencies for optional ML exploration
```

---

## Getting Started

1. Open this folder in VS Code.
2. Make sure your Python environment is active.
3. Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

4. Run the rule-based starter:

    ```bash
    python main.py
    ```

If pieces of the analyzer are not implemented yet, you will see helpful errors that guide you to the TODOs.

To try the ML model later, run:

```bash
python ml_experiments.py
```

---

## What You Will Do

During this lab you will:

- Implement the missing parts of the rule based `MoodAnalyzer`.
- Add new positive and negative words.
- Expand the dataset with more posts, including slang, emojis, sarcasm, or mixed emotions.
- Observe unusual or incorrect predictions and think about why they happen.
- Train a tiny machine learning model and compare its behavior to your rule based system.
- Complete the model card with your findings about data, behavior, limitations, and improvements.
- The goal is to help you reason about how models behave, how data shapes them, and why even small design choices matter.

---

## Tips

- Start with preprocessing before updating scoring rules.
- When debugging, print tokens, scores, or intermediate choices.
- Ask an AI assistant to help create edge case posts or unusual wording.
- Try examples that mislead or confuse your model. Failure cases teach you the most.

---


## TF Reflection

The core concept students should understand is how language can be ambiguous, and turning it into numbers requires real decisions like what counts as positive, how negation changes meaning, and where the boundary between "mixed" and "neutral" sits. Students may struggle with `score_text`, specifically with negation because the order of tokens matters and a simple word-lookup approach breaks down. Labeling the dataset could also prove to be difficult, posts with sarcasm or mixed emotions will show students "correct" labels aren't so obvious, and that those choices shape how the model behaves. The ML comparison will probably surprise people because a 1.00 accuracy sounds impressive until they realize the model trained and tested on the same data, so it's essentially memorizing rather than learning. AI was helpful for explaining errors and suggesting test cases, but it also introduced a real bug by adding sentiment words like `bad` to the negation trigger set, a good example of why AI output needs to be read critically, not just copied. I would guide a student by asking them to print the tokens and running score for that one sentence, then ask which token they think is causing the wrong result in order to get them to reason through their own code.
