---
Week 9
---

# Lecture 1

# Topic Modeling

Filter out from large data what we need.

Like Search Based-on themes.

Provides methods for automatically summarizing, organizing and searching large electronic archives without prior tags or labeling.
Discover adnd organize texts according to the themes.

> Generative Modeling

docs = mixture of topics
assumes topics are generated before docs

So,

Each topic = dist over words
Each doc = mixture of corpus-wide topics
Each word <- one of the topics

important Stats:

1. All docs sahre same set of topics, but each have different proportions
2. Each word drawn from the topics, which are from the doc-decided topics.

> Problems

1. Hidden topic based like topics per docs
2. reversing the gen process


---

# Lecture 2

# Latent Dirichlet Allocation - I

Important Points:

1. BOW: gen model doesn't assumes any order
2. Capturing Polysemy: there is no notion of mutual exclusivity

> Graphical Model
1. Nodes are random variable
2. Edges denote dependence
3. Observed var are shared
4. Plates denoted replicated structure

conditional dep b/w the ensemble of random var


