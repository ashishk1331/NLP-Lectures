---
Week 2 starts
---

> Lecture 1

# Spelling Correction: Edit Distance

eg: I am writing this email on _behaf_ of...

close words:
    1. behalf
    2. behave

(isolated word error correction = correct words without context)

_We need a distance metric!_
edit distance = The minimum number of editing operations including ins, del and subs.

eg:

I N T E * N T I O N
| | | | | | | | | |
* E X E C U T I O N

d s s   i s

1. Each op costs 1 (Levenshetein) = 5
2. Or it costs 2 (alt ver) = 10

Very naive method would be to backtrack with all possible operations.
1. huge complexity (no. of operation)^(length of word)
2. Redundant paths


### Min Edit Distance Matrix

strings: len(X) = n, len(Y) = m
define: D[i, j]
    which stores the edit distance b/w X[i..j] and Y[i..j]
edit distance = D[n, m]

Use dynamic programming with tabulation,
(bottom-up)

Algorithm:

Initialization
D[i, 0] = i
D[0, j] = j

Recurrance relation:
For each i = 1..M
    For each j = 1..N
        D[i, j] = min | D[i - 1, j] + 1 [del]
                      | D[i, j - 1] + 1 [ins]
                      | D[i - 1, j - 1] + 2 if X[i] != Y[j] else 0 [subs]

Termination:
D[N, M] is distance

### Computing ALignments
places where edits took place
use a _backtrace_ = put a pointer from which cell we arrived here

Every non-decrasing path from (0, 0) to (N, M) is a valid alignment. However, pick the optimal one.

### Complexity

time = O(N * M)
Space = O(N * M)
Backtrace = O(N + M)


---

> Lecture 2

# Weighted Edit distance

typos:
1. similarity between vowels
2. mistypying from keyboard

Algorithm:

Initialization
D[0, 0] = 0
D[i, 0] = D[i - 1, 0] + del[x(i)]; 1 < i <= N
D[0, j] = D[0, j - 1] + ins[y(j)]; 1 < j <= M

Recurrance relation:
For each i = 1..M
    For each j = 1..N
        D[i, j] = min | D[i - 1, j] + del[X(i)]
                      | D[i, j - 1] + ins[Y(j)]
                      | D[i - 1, j - 1] + sub[X(i), Y(j)]
                      | D[i - 2, j - 2] if
                        x[i - 1] == y[j] and
                        x[i] == y[j - 1]
    where:
        xy   !=    yx
   (i-1)||i   (j-1)||j

Termination:
D[N, M] is distance

transpose(metathesis), "la" written as "al"

Ways of storing vocab and perform search:

1. Trie Data Structure
to efficiently find words

2. Generate words from the target within a specific edit distance
eg. Word len 9 and vocab size of 36 yeilds 114,324 possibilities.

3. Symmetric delete spelling correction
delete operation of target of distance <= 2
keep a vocab of deleted patterns

Types of spelling errors,
1. non-word error
    behaf -> behalf
2. real word error
    three -> their
3. homophones
    peas -> peace

For each word w, generate a candidate set
1. Find candidate words with similar pronunciation
2. Find candidate words with similar spelling
3. Include _w_ in candidate set


---

Lecture 3

# Noisy Channel Model for Spelling Correction

x = mispelled word

w' = argmax P(w|x) = argmax P(x|w)P(w)
     w < V                  ----------
                               P(x)

Maybe maintain a dictionary of all words.
We'll use _Bayes Theorem._

Non-word Spelling errors:
1. Words with similar spelling
    smaller edit diatnce
2. similar pronunciation
    
> Damerau - Levenshtien edit distance
ins, del, subs and transposition of two adjacent aplhabets

eg ->

x = acress

w = { 
    actress [del t], 
    cress   [ins a], 
    caress  [trans ca -> ac], 
    access  [subs c -> r], 
    across  [subs o -> e], 
    acres   [del s first], 
    acres   [del s second] 
}

1. 80% errors are in edit distance of 1
2. Mostly all are under or equal to 2 edit distance
3. Allow deletion of space and hyphen

Computing error probability: confusion matrix

del[x, y]    : count (xy typed as x)
ins[x, y]    : count (x type as xy)
sub[x, y]    : count (x typed as y)
transp[x, y] : count (xy typed as yx)

Find errors and corrections.

P(x|w) = | del[w(i - 1), w(i)] / count[w(i - 1), w(i)] if del
         | ins[w(i - 1), x(i)] / count[w(i - 1)] if ins
         | sub[x(i), w(i)] / count[w(i)] if sub
         | trans[w(i), w(i + 1)] / count[w(i), w(i + 1)] if trans

Now, we can calculate P(x|word)
eg, 
    c|ct    .000117
    a|#     .00000144

Now, we calculate P(w) as frequency of occuring of _w_ in the corpora.
            P(x|word)   P(word)
    c|ct    .000117     .0000231
    a|#     .00000144   .000000544

Using a bigram model,
Giving the context

"...versatile acress whose ..."

P(actress|versatile) = 0.000021
P(across|versatile)  = 0.000021

Using following token to add bias,

P(whose|actress) = 0.001
P(whose|across)  = 0.000006

Hence, We multiply both preceding and following tokens' probability to get the score ofr right token.

### Real Word spelling errors

1. The study was conducted mainly _be_ John Black.
2. The design _an_ construction of the ssytem.

Create a candidate set.
Choose the sequence W that maximizes the P(W|X).
eg:

to -> of -> the

instead of,

too -> on -> the

> Simplification: One error per sentence

---

# Lecture 4

## N-gram Language models

eg:

The office is about fifteen _minuets_ from my house.

Now, even _minuets_ is in the vocab.

Hence, we require context to correctly guess the word.

Use a language model:

1. Speech Recognition
    P(I saw a van) >> P(eyes awe of an)

2. Machine Translation
    P(high winds) > P(large winds)

3. Context Sensitive Spelling Correction
4. Natural Langugage Generation
5. Completion/Suggestion [Predictive Text Input]

> Goal:
Compute the P of a sentence or sequence of words

P(W) = P(w1, w2, w3, ... , wn)

> Related Task:
P of next token:

P(w4|w1, w2, w3)

Basic Idea:
Use chain rule of P for making predictions of next word in the sentence.

    P(B|A) = P(A, B)
             -------
               P(A)

P(A, B) = P(B | A) P(A)
P(A, B, C) = P(A) P(B|A) P(C|A, B)

Hence we can create a chain of P of words.

Count and divide:

P(office | about fifteen minutes from) = Count(about fifteen minutes from office) / Count(about fifteen minutes from)

> Problem:

We may not have sufficient data for P calculation.

Simplification:

P(office | ...) ~ P(office | from)

OR

P(office | ...) ~ P(office | minutes from)

### Markov Assumption
kth order Markov Model
focus on only _k_ previous words

P(w1, w2, ..., wn) ~ prod(wi | w(i - k), ... , w(i - 1))

### N-gram model
which uses only N-1 words of prior context, that resembles N-1 order Markov Model.

Language has long-distance dependencies,

"The computer which I had just put into the machine room on the fifth floor crashed."

crashed -> computer [14 steps away]

### Estimating N-gram P
use maximum likelihood estimate
count = c

P(wi|wi - 1) = c(wi - 1, wi) / c(wi - 1)

Like, (given 3 sentences)

P(I|<s>) = 2/3

> Normalize by unigrams.

P(<s>I want english food</s>) = P(I|<s>) . P(want| I) . P(english | want) . P(food | english) . P(</s> | food)
                              = 0.000031

### Issues:

1. Everything in log sapce
    a. avoids underflow
    b. adding is faster than multiplying
        log(p1 x p2) = log(p1) + log(p2)

2. Handling zeros - use smoothing

---

# Lecture 5

# Evaluation of Language Models and Basic Smoothinig

Intrinsic evaluation: Perplexity 
Intuition: The Shannon Game

How well can we predict the next word?

eg: I like my pizza with ...

We have scores for each completion and rank models accordingly.

> Perplexity is the inverse probability of the test data, normalized by the number of words:
        PP(W) = P(w1, w2, ..., wn) ^ (-1/n)

Applying chain rule:
        
        PP(W) = prod(1/ P(wi|w1 - wi - 1)) ^ (1/n)

For bigram model,

        PP(W) = prod(1 / P(wi|wi - 1)) ^ 1/n

Lower Perplexity = better model

### Shannon Visualization Method

Use the language model to generate word sequences

1. Choose a random bigram (<s>, w) as per its probability
2. Choose a random bigram (w, x) as per its probability
3. And so on until we choose </s>

If a bigram learnt by model doesn't exists in the test data, while comparing the P of bigram will be 0. Hence, the PP score will become 0.

In Shakespeare corpora, there are possible 844 Mil bigrams but in reality there are 300,000 bigrams only.

### Smoothing
steal P mass to generalize better

### Laplace Smoothing (Add one estimation)
Pretend as if we saw each word one more time that we actually did

MLE estimate for bigram = P(MLE)(wi|wi - 1) = c(wi - 1, wi) / c(wi - 1)

ADD-1 estimate = P(Add - 1)(wi | wi - 1) = c(wi - 1, wi) + 1 / c(wi - 1) + V

After this, all 0 P change to smaller Ps.

### Add-K estimation (MLE)

    P(Add-k)(wi|wi - 1) = c(wi - 1, wi) + k / c(wi - 1) + kV


