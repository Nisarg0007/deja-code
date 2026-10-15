# Technical Decisions

## Why compare three retrieval methods?

TF-IDF gives a simple lexical baseline. Sentence embeddings let the search compare meaning across different wording. The hybrid method tests whether combining lexical and semantic signals helps. In the current small benchmark, hybrid retrieval does not consistently beat semantic retrieval, so it is presented as an experiment rather than an improvement claim.

## Why use sentence embeddings?

The task is about finding related problems whose storylines or wording may differ. `sentence-transformers/all-MiniLM-L6-v2` provides a practical 384-dimensional representation that can be compared using cosine similarity.

## Why cluster the embeddings?

Clustering is an exploratory way to inspect the embedding space. K-Means with four clusters was selected from the configurations tried, but the silhouette score was low (about 0.04). The clusters should not be treated as reliable algorithm-topic labels.

## Main failure case

A result can share words or broad concepts with a query while having a different objective. For example, `RG_01 — Maximal Score Path` maximizes the minimum edge weight along a path; a shortest-path problem minimizes total path distance. This motivates intent-aware reranking as a possible next step.

## Evaluation caveats

The manually reviewed benchmark contains five queries, which is too small to support broad conclusions. Tag-overlap evaluation is separate and uses noisy, incomplete tags as a relevance proxy. Neither should be presented as definitive ground truth.

## What I would improve next

I would create a larger query set with relevance judgments made before running the comparison, then compare the same metrics across TF-IDF, semantic, and hybrid retrieval. After that, I would test a reranking step that considers the actual objective in the full statement.
