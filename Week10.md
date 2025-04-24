---
Week 10
---

# Lecture 1

# Entity Linking - I

Task of identifying all mentions in text of a specific entity
Two phases:
1. Entity Annotation (or Candidate Selection)
2. Entity Resolution (or Entity Disambiguation)

> Issues:
1. name variations
2. entity ambiguity

_Wikification!_

> Common steps:
1. mention detection    MD
2. link generation      LG
3. disambiguation       DA

> Measure MD
keyphraseness(w)
Number of Wikipedia articles using it as an anchor, divided by # articles mentioning it

    = CF(wl) / CF(w) [collection freq w]
        |
        v
        [collection freq w as a link]

> Measure DA
commonness(w, c)
Particular sense occurance for a word

    = |L(w, c)| / sum (|L(w, c')|)

> Connot always be accurate
> Missing context


---


# Lecture 2

# Entity Linking - II

> Relatedness
Use un-ambi words to dis-ambi ambi words.

Each candidate sense and context term is represented by a single Wiki article.

The relatedness of a candidate sense is the weighted average of its relatedness to each context article.

sense and context term is represented by a single Wiki article.

The relatedness of a candidate sense is the weighted average of its relatedness to each context article.

1. Link P: word being link
2. Relatedness: resemblence to central doc = average semantic relatedness to all other context terms

- Wikipedia articles are used as training examples for a classifier.
- Positive examples = articles that were manually linked.
- Negative examples = articles that were not linked.
- Features from articles and mention contexts are extracted.
- These features help the classifier learn what to link.

3. Dis-ambi confidence = The confidence score for the classifer.
4. Generality = depth at which the link is present
5. Location and spread = first, last occurance and spread

---

# Lecture 3

# Info Extraction

Task of extraction structured info from unstructured data. [Organisation of info]

eg:
_Critical task_ = automatically identify mentions of biomedical entities in text
and link them to their corresponding entries in existing knowledge bases.

> 5 easy methods

1. hand-built patterns
2. bootstrapping methods
3. supervised methods
4. distant supervision
5. unsupervised methods

1. Rule based extraction.
    [org] in [loc]

2. Hearst's lexo-syntactic patt [hyponyms]
    X and other Y
    Y, including X

3. Berland and Charnaiak's patterns
    find all occurances of the pattern including building and basement

> Issues: Hand-built

1. low accuracy
2. difficult extendability

---

# Lecture 4

# Relation Extraction

> Bootstrapping approaches
When you don't have enough annotated text.

Get two related words: [Mark Twain, Elmira] = [X, Y]

1. X is buried in Y
2. The grave of X is in Y
3. Y is X's final resting place

Generate relation then.

No probabilistic interpretation.

> Supervised Relation Extraction
Find and label data.
Train a classifier on it.
Another classifier to decide if entities are related.

Features: dependency syntax features

> Gazetter and trigger word features

Kinship terms
parent, wife

Gazetter 
country names

> Relation Extraction classifier

1. SVM
2. MaxEnt (multiclass logistic regression)
3. Naive Bayes

Compute P/R/F1 for each relation,

Prec = # correctly extracted relations / # extractions
Reca = # correctly extracted relations / # gold relations
F1 = 2PR / (P + R)

> Points

1. Supervised approach have high accuracy
2. Issues:
    a. Labeling large training set
    b. doesn't generalize

---

# Lecture 5

# Distant Supervision

If two entities belong to a certain relation, any sentence containing those two entities is likely to express that relation.

> Approach

1. get sentences containing these entities
2. extract lots of noisy features from the sentences
    named entities, syntactic features, named entity tags
3. combine a classifier

> Hypernyms via distant supervision
Construct a noisy training set consisting of occurrences from a corpus
hyponym     - hypernym
Shakespeare - author

> Syntactic Dependency Paths
