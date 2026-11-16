# LLM labeling prompts for Mobile JKN aspect-based sentiment analysis

This repository holds the prompts used to produce the LLM-consensus silver labels in the paper
"Feature Extraction vs. Fine-Tuning IndoBERT for Mobile JKN Aspect-Based Sentiment Analysis with LLM-Consensus Labels".
Each prompt contains the labeling guideline the models followed. The prompts are in Indonesian, exactly as they were sent.

| File | Used for |
|---|---|
| `prompt_round1_all_aspects.md` | Round 1: all three aspects (*Performa*, *Antarmuka*, *Fitur Layanan*) for every silver review. The guideline inside is a condensed version of the guideline given to the human annotators. |
| `prompt_round2_antarmuka.md` | Round 2: *Antarmuka* only, with the revised guideline, for 1,421 candidate silver reviews and the 372 gold validation reviews. |

## How the prompts were used

- Models: Claude Sonnet 5.5, Gemini 3.1 Pro, and DeepSeek V4.1 Flash, through their chat interfaces.
- Access dates: 7 November 2026 for round 1, 12 November 2026 for round 2.
- Each batch was a JSON file with the prompt (field `petunjuk`) and numbered review texts (field `ulasan`). Round 1 used 125 reviews per batch and round 2 used 100.
- The models saw only the normalized review text: no star rating, split, review ID, or other label.
- Output: one line per review, `<no> <code>`. Codes: `+` positive, `-` negative, `0` neutral, `.` absent, optionally followed by `?` when unsure.
- Consensus: per review and per aspect, the code given by at least two of the three models. When all three differ, the code of DeepSeek, then Claude, then Gemini is kept. For *Antarmuka*, the vote uses the round-2 codes.
- In round 2, candidate reviews are the silver reviews that at least one model labeled *Antarmuka* in round 1, or whose text contains interface or ease-of-use words.

The review texts and the labels are not included in this repository.
