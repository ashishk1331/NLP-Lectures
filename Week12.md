---
Week 12
---

# lecture 1

# Sentiment Analysis

Use Cases,
1. movie review
2. product review
3. public sentiment
4. politcal sentiment
5. prediction

Used to assess _emotion_, _mood_, _attitdudes_, _traits_ and _stances_.

Task levels:
1. neg or pos
2. rank it 1 - 5
3. identify source, target and context

> Baseline Algorithm:
1. Tokenization
2. feature extraction
3. model: Naive Bayes

> Tokenization Issues
1. capital
2. emoticon
3. word lengths
4. handling neg: didn't

> NaiveBayes

    C[nb] = arg max P(cj) prod P(xi|cj)

Remove all duplicates from the doc.
Can't love for words for scoring.

---

# Lecture 2

# Sentiment Analysis - Affective Lexicons

Libs like : the great enquirer, SentiWordnet, etc.

> Valence/Arousal Dimension


    anger   |   excitement
  --------------------------
    sadness |   relaxation

valence = the pleasantness of stimulation
arousal = the intensity of provoked emotions
dominance = the degree of control exerted

---

# Lecture 3

# learning affective lexicons

Steps:
1. Label seed set of adjectives
2. Expand seed set to conjoined adjectives
    using _and_, _but_
3. contruct a graph
    polarity and similarity is assigned to each word pair

outputs polarity lexicon -> pos and neg

> Turney Algorithm

1. Extract a phrasal lexicon from reviews
2. Learn polarity of each phrase
3. review rating = avg polarity of phrases

Use _PMI_ to calculate co-occurance measure.

Polarity(phrase) = PMI(phrase, excellent) - PMI(ohrase, poor)
                 = log2 (hits(phrase NEAR excellent) hits(poor)) / (hits(excellent) hits(phrase NEAR poor))

Use Wordnet to find polarity.

---

# Lecture 4

# Computing with affective lexicons

Learn words from online reviews
Each review has words and a rating usually 1 - 5.

Use P for how likely a word occurs in a n-star rating

P(w|c) = f(w, c) / sum( f(w, c))

Make them comparable with scaled P, = P(w|c) / P(w)

Using sentiment lexicons also works.

Like,

Excellent   +5      Not excellent   -5
good        +3      Not good        -3
terrible    -5      Not terrible    +5
bad         -3      Not bad         +3

Instead used a polarity shifter:

    = (max points - score) + 1

eg: good = 3, not good = (3 - 4) - 1

> Intersifiers
Amplifiers: very        inc score
Downtoners: slightly    dec score

---

# Lecture 5

# Aspect Based Sentiment Analysis

Frequent Phrases + rules
1. get all highly frequent phrases
2. filter rules
3. get aspects

If aspects are well understood, use supervied classification.

> Use Cases:

1. Summarization
2. Comparision
3. Machine translation
4. Question Answering
5. text processing
