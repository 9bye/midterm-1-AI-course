# Midterm 1: answers

GitHub username: 9bye
Session code: kettle-62
Fingerprint: a8806029

## AI-use declaration

I used AI tools and Python, as this exam permits, and I declare it here. First, I ran the course generator (make_my_data.py) to create my personal data files. I then used an AI coding assistant (Z.ai CLI, GLM model) to debug and extend a Python script that computes every number in this file directly from my_data/: recall@k and MRR@10 from retrieval_ranks.csv, 95 % Wilson intervals with the course formula (z = 1.96), annual costs from the DATA.md prices and traffic, pipeline accuracies and weakest cells with their intervals, the page and cost arithmetic, and the Part 3 numbers from my five question tables. The assistant also helped draft the 1d decision, the 2b memo and the Why lines; I checked each of them against the DATA.md tables and the lecture material. Gemini was used earlier to explore the formulas. No other sources were used: all numbers are synthetic and come only from my generated data, and I can explain every table and sentence at the oral check.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.550 | 0.633 | 0.683 | 0.597 |
| B | 0.333 | 0.633 | 0.717 | 0.481 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.633 | 0.507 | 0.744 |
| A, k=5 | 0.683 | 0.558 | 0.787 |
| B, k=3 | 0.633 | 0.507 | 0.744 |
| B, k=5 | 0.717 | 0.592 | 0.815 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 15980.34 |
| A, k=5 | 21321.49 |
| B, k=3 | 10017.67 |
| B, k=5 | 11383.71 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=3

The annual budget is $13,700. Over budget: (A, k=3) at $15,980/yr, (A, k=5) at $21,321/yr. (B, k=5) has the highest affordable recall (0.717, 95 % Wilson [0.592, 0.815]), but its interval overlaps (B, k=3)'s [0.507, 0.744]: the gap is not statistically demonstrated, so no winner is claimed between them. We therefore take the cheapest statistically tied candidate, (B, k=3), at $10,018/yr. Recall@3 of the choice by language: EN 0.750, RU 0.650, KK 0.500 — Kazakh-speaking customers (KK) are served worst.

### 1e. Next measurement (at most 60 words)

Run Model M end-to-end on 300 fresh support queries (100 per language), scoring final answers for (B, k=3) against (B, k=5). Recall alone cannot show whether the extra context improves answers; if answer accuracy ties, the cheaper choice stands, and if the alternative wins clearly, we switch.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.894 | KK, degraded | 0.800 | 0.627 | 0.905 | 27736.35 |
| L | 0.806 | RU, degraded | 0.633 | 0.455 | 0.781 | 10800.00 |

Personal-data pages per year sent abroad under H: 465375

### 2b. Memo (at most 300 words)

Pipeline H reaches 0.894 overall accuracy and L reaches 0.806, but averages hide each pipeline's worst-served group. H's weakest cell is Kazakh, degraded scans: 0.800 (95 % Wilson [0.627, 0.905]). L's weakest cell is Russian, degraded scans: 0.633 ([0.455, 0.781]). Thirty documents per cell make both intervals wide, so the exact drops are uncertain, but both worst cells sit far below the averages. Because 15 % of pages carry an IIN (465,375 pages/year), Law No. 94-V applies, and Lecture 5.1's three questions shape the design. Where does the scan go? Under H, to a data centre abroad: a cross-border transfer of personal data; under L, nothing leaves the bank. Who stores it? Under H, a foreign vendor and its subprocessors; under L, our own IT. For how long? Under H, retention is a vendor policy we cannot verify; under L, we control and audit deletion. What we know and must find out: for H we would need the vendor's data-processing agreement, retention schedule and deletion proofs — a vendor's statement is not a compliance review; for L we hold the logs ourselves. Verifiable safeguards for either pipeline: automated IIN masking before storage, egress-firewall rules for scan data, raw scans deleted within 24 hours, mandatory manual review of every degraded scan. Choice: Pipeline L at $10,800/yr against H's $27,736/yr — cheaper, and it keeps IIN data on-premise, avoiding an unlawful cross-border transfer. Residual risk: L's 0.633 accuracy on degraded Russian scans; the mandatory manual review contains it.

## Part 3: Explain your own numbers

### Q1

Number: 2.57
Why (at most 40 words): The bill weights input and output tokens by different prices ($2 vs $12 per million). KK uses 3.17x the input but only 1.84x the output tokens, so the price-weighted bill ratio (2.57) differs from the raw token ratio (3.01).

### Q2

Number: 13.5
Why (at most 40 words): Only EN is cut (576 cl100k = 592 e5 tokens > 512; RU 288, KK 171 stay inside). The tail past token 512 is silently truncated: e5-small raises no error, the embedding just never sees it.

### Q3

Number: 80.2
Why (at most 40 words): Top-p did not rescale anything: it cut the tail after 2 tokens (mass 0.93) and renormalised, making unlikely tokens unsampleable, while temperature would keep all tokens and only change sharpness.

### Q4

Number: 10
Why (at most 40 words): The accuracy leaderboard prefers X (67 vs 64): abstaining earns nothing and wrong guesses are free. Penalised, Y wins 44 to 34 — so the public metric teaches models to guess rather than say "I don't know".

### Q5

Number: 2.65
Why (at most 40 words): 53 tokens per parameter vs Chinchilla's 20: the model is over-trained by the compute-optimal criterion. Chinchilla concluded GPT-3-class models were undertrained — too large for their token budgets — and favoured smaller models on more data.
