# Déjà Code — Semantic Search for Competitive Programming Problems

Déjà Code explores whether natural-language search can find related competitive programming problems, even when the wording differs.

The notebook compares a lexical baseline with sentence embeddings and a hybrid ranker. It also explores problem clusters and inspects cases where similar wording does not mean the same problem-solving objective.

## What's included

- `notebooks/deja_code.ipynb` — the end-to-end Colab notebook
- `requirements.txt` — main Python dependencies
- `DECISIONS.md` — implementation choices and limitations
- `figures/` — place for exported plots from the notebook

## Dataset

The project uses the public CodeChef Competitive Programming dataset hosted on Kaggle:

https://www.kaggle.com/datasets/arjoonn/codechef-competitive-programming

The downloaded CSV initially contained 1,474 rows. The notebook cleans the problem statements and removes duplicate or very short entries; the processed dataset used in the experiments contains 1,429 problems.

This source was found independently. The task sheet names the CodeChef Competitive Programming Dataset but does not provide a download URL, so this repository does not claim that the Kaggle copy is the exact dataset intended by the organizers.

The dataset itself is not included in this repository. Download `questions.csv` from the source above.

## Run in Google Colab

1. Download or open `notebooks/deja_code.ipynb` in Google Colab.
2. Run the cells from top to bottom.
3. When prompted, upload either `questions.csv` or `questions.csv.zip`.
4. Wait for the sentence-transformer model to download on the first run.

A GPU is helpful for embedding generation but is not required. Runtime and memory use depend on the Colab session.

## Methods

- **TF-IDF:** lexical baseline using unigrams and bigrams.
- **Sentence embeddings:** `sentence-transformers/all-MiniLM-L6-v2` embeddings compared using cosine similarity.
- **Hybrid retrieval:** combines semantic and normalized lexical scores.
- **Clustering:** K-Means over embeddings, with PCA used to visualize the groups.
- **Evaluation:** the notebook includes tag-overlap evaluation and a small manually reviewed query set. These answer different questions and should not be treated as interchangeable ground truth.

## Observations

In the five-query manual comparison recorded during development, semantic retrieval had mean Precision@5 of 0.64, hybrid retrieval 0.52, and TF-IDF 0.36. This is a small exploratory sample, not evidence of general performance across all competitive programming queries. See `figures/retrieval_precision_at_5.png` for a plot of these values.

The four-cluster K-Means experiment had a silhouette score of about 0.04. The clusters overlap substantially; one smaller cluster had a stronger string-processing theme, while the others contained mixed topics. The cluster-size plot is in `figures/cluster_sizes.png`.

## Known limitations

- The dataset is relatively small and may be incomplete or dated.
- Some problem tags are missing or noisy.
- The manual benchmark has only five queries.
- Shared tags are an imperfect relevance proxy for automatic evaluation.
- Similar vocabulary can hide different objectives. For example, a maximum-bottleneck path is not the same as a shortest path, and counting sums is not the same as answering range-sum queries.
- Hybrid retrieval does not consistently outperform semantic retrieval.

## Possible next steps

- Add more manually judged queries and define relevance criteria before evaluation.
- Report Precision@k, Recall@k, and MRR on a larger benchmark.
- Explore intent-aware reranking and compare it against the current methods.
- Add an interactive search interface.
