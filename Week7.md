---
Week 7
---

# Lecture 1

# Distributional Semantics

> Semantics
Study of meaning: relationship between symbols and their denotata.

> Computational Semantics
The study of how to automate the process of constructing and reasoning with meaning representations of natural language expressions.

1. Formal Semantics: Construction of precise mathematical models
2. Distributional Semantics: statistical patterns of human word usuage

> Distributional Hypothesis

1. Word meaning is reflected in linguistic distributions.
2. Semantically similar words tend to have similar dist patterns.

> Distributional Semantic Models (DSMs)

1. Computational models that build contextual semantic representations from corpus data. (here, vectors)
2. Vecs are obtained through the statistical analysis of the linguistic contexts of a word.

1. Dist are vecs in multi-dimensional semantic space, that is, objects with a magnitude and a direction.
2. The _semantic space_ has dimensions which correspond to possible contexts, as gathered from given corus.

> Constructing Word Spaces

target words, context window, co-occurance matrix.
Build vectors out of these co-occurance counts.

distribution matrix = targets x contexts
                wheel       transport
automobile      1           1
car             1           2

Using simple vector product:

automobile . car = 3

---

# Lecture 2

# Distributional Models of Semantics

Words are treated as atomic symbols.
One-Hot representation.

> Building DSM

1. Linguistic steps

    Pre-process a corpus (to define targets and context) ->
    select the targets and contexts

2. Mathematical steps

    count target-context co-occurance ->
    weight the contexts ->
    build the distributional matrix ->
    reduce the matrix dimensions ->
    compute the vector distances

> Many Design choices,

1. Matrix Type
    word x document
    word x word

2. Weighting
    probabilities
    tf-idf
    pmi

3. Dimension reduction
    PCA
    LDA

4. vector comparision
    Euclidean
    Cosine
    Dice

> General Questions,

1. How do the rows(words) relate to each other?
2. How do the columns(context, documents) relate to each other?

Words as context:
1. Window size
2. Window shape: rectangular/triangular/other

> Context weighing: docs as contxt

word freq(fij), document length(|Di|), document frequency(F < 1/ Nj)

tf-idf: fij * log(N/Nj) [Use l2 normalization]

fij = word freq in doc
N = # of docs
Nj = freq in docs

> Context Weighing: Words as context

Association measures = provide more weight to contexts that are more significantly associated with a target word.
eg: Mutual info, log-liklihood ratio

> Pointwise Mutual Information (PMI)

    PMI(w1, w2) = log2 (P(corpus)(w1, w2) / P(ind)(w1, w2))
                = freq(w1, w2) / N = freq(w) / N

All PMI values < 0 are replaced with 0.
Bias towards infreq events.

inc PMI with dec P(wi).
If wj occurs 1, also with wi in corpus.

    delta(i, j) = (fij/ fij + 1) * (min(fi, fj) / min(fi, fj) + 1)

    PMIn(wi, wj) = delta(i, j) * PMI(wi, wj)


---

# Lecture 3

# Distributional Semantics: Applications, Structured Models

> term mis-match

A relevant doc may contain different words than the actual query.

> Using DSM for query expansion
re-make query for better retrieval

1. dist vecs are calcu
2. expanded query = sum of vecs

> Similarity measures for binary vectors:

    Dice coff = 2 |X int Y|
                -----------
                 |X| + |Y|

    Jaccard coff =  |X int Y|
                    ---------
                    |X un  Y|

    Overlap coff =    |X int Y|
                    -------------
                    min(|X|, |Y|)

J coff penailizes small # shared entities
O coff uses concept of inclusion

> Similarity Score for Vector Spaces

    Cosine similarity = cos(X, Y) = (X . Y) / (|X| |Y|)

    Euclidean = |X - Y| = sqrt(sum(xi - yi))
    x < X, y < Y


> Similarity Measure for P dist,

    L1-norm         = sum(pi - qi)
    LK - divergence = sum(pi . log(pi / qi))

> Attributional SImilarity
    words = a,b how similar their meaning is.
    eg,, dog and wolf

> relational similarity
    pair (a, b) and (c, d) with many relations
    eg: dog: bark and cat: meow

> Structured DSM

How to capture syntactic relation?
eg.     X solves Y
        Y is solved by X

1. Use Dependency Graph framework.
2. Two way matrix

Selection Preference = what comes after what?

---

# Lecture 4

# Word Embedding - I

1-of-N encoding (or one hot encoding) only limit to equality testing.

Word vector = vector of weights
Any word wi in the corpus is given a distributional representation by an embedding

    wi  = R(d) ; d-dimensional vector.
    
    d < [50, 1000]

Unlike ohe, here represenattion of wi is spread across entire vector.
SVD can also be thought as an embedding method.

> Analogy testing

a:b :: c: ?

d = arg max(wb - wa + wc)T.wx
    -------------------------
        ||wb - wa - wc||

> Learning word vectors
Two variations: CBOW and Skip-grams

---

# Lecture 5

# Word Embedding - II

> CBOW

Like a window of 9 with central focused word in the middle.

[an efficient method for] (learning) [high quality distributed vector]

The context words form the input layer.
Each word is encoded in ohe.
A single hidden and output layer.

Goal: To maximize cond P of observing focus word for the inp context.

inp(1 x V) . W1(V x N) = hidden(1 x N)

ohe input simply selects weight from the W mat and forms hidden input.

then another W2 is used to form the output.

> Skip-grams

Reverse of CBOW, focus word as a simple vector and outputs many grams vectors(context is present in the output.)

Goal: maximise the P of context word given focus word.

    J(theta) = sum(log P(wi + j | wt))

Gradient descent for parameters update in theta.

> Glove:

Count based model + direct prediction model

faster and scalable to bif corpora.

