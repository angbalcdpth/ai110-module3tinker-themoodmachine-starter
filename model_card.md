# Model Card: Mood Machine

This documents two versions of the Mood Machine sentiment classifier — a rule-based model and a simple ML model — both built and evaluated in this lab.

## 1. Model Overview

**Purpose:** Read a short post and label its mood as `positive`, `negative`, `neutral`, or `mixed`.

**Rule-based model:** Scans a sentence for known positive and negative words, adds up a score, and picks a label based on whether the total is positive, negative, or zero. Words like *love* and *hate* count double.

**ML model:** Learns which words tend to appear in positive vs. negative posts by training on the labeled examples in `dataset.py`. No hand-written rules — it figures out patterns from the data.



## 2. Data

**Size:** 19 labeled posts (started with 6, added 13 during the lab).

**How labels were chosen:** Each post was read and assigned the label that best matched its overall tone — not just individual words. Some posts were genuinely hard to call:
- *"Feeling tired but kind of hopeful"* → `mixed` (both feelings are real)
- *"That movie was so bad it was actually funny 😂"* → `mixed` (could also be `negative`)
- *"Love that my laptop crashed right before submission"* → `negative` (sarcasm — *love* is not genuine)

**What's in the dataset:** slang (*lowkey*, *no cap*), emojis, sarcasm, and posts with conflicting emotions.

**Limitations:** Only 19 examples, labeled by one person, mostly informal American English.

## 3. How the Rule Based Model Works (if used)

**How scoring works:**
- Known positive words: +1 (strong ones like *love*, *amazing*: +2)
- Known negative words: -1 (strong ones like *hate*, *terrible*: -2)
- Negation: *not happy* flips the score of *happy* from +1 to -1
- Emojis are converted to word-like tokens (e.g. `😂` → `laugh_emoji`) but are not scored yet
- Final label: positive score → `positive`, negative → `negative`, zero → `neutral`

**Works well on:** Simple sentences with known words like *"I hate this"* or *"I love today"*.

**Breaks on:** Sarcasm, unknown words (*proud*, *hopeful*, *crisis*), and anything requiring context. Can never produce a `mixed` label. **Accuracy: 0.42**.

## 4. How the ML Model Works (if used)

**How it learns:** It counts which words appear in each labeled post, then learns which word patterns go with which labels.

**Training data:** The same 19 posts in `dataset.py`.

**Accuracy: 1.00** — but this is on the data it already saw. That number means it memorized the examples, not that it truly understands language.

**Works well on:** Picked up sarcasm correctly in training because words like *stuck* and *traffic* appeared alongside `negative` labels.

**Breaks on:** Any sentence it has never seen before. One wrong label in training can throw off multiple predictions.

## 5. Evaluation

| Model | Accuracy |
|---|---|
| Rule-based | 0.42 / 19 posts |
| ML | 1.00 / 19 posts (training set only) |

**Rule-based: got right**
- *"I love this class so much"* → `positive` (*love* +2)
- *"I hate getting stuck in traffic"* → `negative` (*hate* -2)
- *"I am not happy about this"* → `negative` (negation rule catches *not happy*)

**Rule-based: got wrong**
- *"I absolutely love getting stuck in traffic for an hour"* → said `positive`, real label `negative`. It saw *love* and stopped. Sarcasm is invisible.
- *"Great, another group project where nobody replies 💀"* → said `positive`, real label `negative`. *Great* scored +1; the 💀 emoji and frustrated tone didn't register.
- *"Feeling tired but kind of hopeful"* → said `negative`, real label `mixed`. *Hopeful* isn't in the vocabulary so it scored 0 and *tired* (-1) won.

**ML vs rule-based:** The ML model got all 19 right — including the sarcasm cases — because words like *stuck* and *traffic* appeared in negative training examples. But since it was tested on the same data it trained on, that score doesn't mean much for new sentences.

## 6. Limitations

- Only 19 training examples — too few to trust accuracy numbers.
- Both models were tested on the same data they trained on, so real-world accuracy is unknown.
- Sarcasm breaks the rule-based model every time a positive word is used sarcastically.
- The rule-based model can never output `mixed`.
- Common mood words like *proud*, *hopeful*, *terrified*, *empty* aren't in the vocabulary and score 0.
- Emojis are recognized during preprocessing but don't affect the final score.

## 7. Ethical Considerations

- A person saying *"I guess I'm fine"* might be in distress. This model would call that `neutral` and miss it entirely.
- The dataset is informal American English only. Dialects, other languages, and cultural slang the model hasn't seen will get misread or ignored.
- All labels were assigned by one person — sentiment isn't objective, and others might reasonably disagree.
- This model analyzes personal messages. Any real use of a system like this should be opt-in and transparent.

## 8. Ideas for Improvement

- Create a separate test set (20+ examples the model never trained on) for honest accuracy
- Add missing mood words to the vocabulary: *proud*, *hopeful*, *terrified*, *empty*, *burned out*
- Give emoji tokens actual scores: `sad_emoji`/`dead_emoji` → -1, `smile_emoji` → +1
- Add a `mixed` label to the rule-based model for weak scores (e.g. score = ±1)
- Grow the dataset to at least 100 labeled examples
- Try a pre-trained transformer model (like BERT) which understands context and handles sarcasm much better
