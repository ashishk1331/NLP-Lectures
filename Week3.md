---
Week 3 starts here
---

# Advanced Smoothing algorithms

1. Good Turing
    Use the count of things we have seen once to estimate the count of tings we have never seen

<s>I am here</s>
<s>who am I</s>
<s>I would like</s>

freq of occuring
I     = 3       N(1) = 4
am    = 2       N(2) = 1
here  = 1       N(3) = 1
who   = 1
would = 1
like  = 1

> Idea

1. Reallocate the P mass of n-grams that occur r + 1 times in the training data to the n-grams that occur r times
2. Reallocate mass to never seen n-grams from observed n-grams

> Adjusted Count

For each count c, an adjusted count c* is computed as:
    
    c* = (c + 1)N(c + 1)
         ----------------
                N(c)
where, N(c) is the number of n-grams seen c times.

If c = 0,
P*GT(things with freq c) = N1 / N

> Issues:

1. For small c, N(c) > N(c + 1)
2. For high c, too jumpy

Replace empirical K(k) with a best-fit law once counts get unreliable

Count       Good Turing c*
0           0.0000027
1           0.446
2           1.26

bi-grams that don't occur in the corpus:

=  V^2 - N(b)
   ----------
       N

### Absolute Discounting Interpollation

    c* = c - .75

P(abs)(wi|wi - 1) = c(wi|wi - 1) - d 
                    ---------------- + lambda(wi - 1)P(wi)
                        c(wi - 1)

### Kneser-Ney Smoothing

eg. I can't see without my reading ...(glasses | Francisco)

Maybe, F is more common than glasses

[---TODO---]

---

# Lecture 2 - Computational Morphology

Morphology - internal structure of words
Smaller units = morphemes

eg:
    dogs       = dog + s
    unladylike = un + lady + like

> Allomorphs
variants of the same morpheme but cannot be replaced by one another
eg: un-happy
    in-comprehensible
    im-possible

> Bound vs Free Morphemes

Bound can't exists alone.
Free carry individual meaning.
eg:
    un    = bound
    happy = free

> Stems and Affixes

Stems(roots): The core meaning bearing units
Affixes: bits adhering to the stems to change their meanings and grammartical functions

Mostly, Stems = Free and Affixes = Bound.

Prefixes and suffixes.

Infix: 'n' in 'vindati' (he knows)

Circumfixes: precedes and follows the stem

berg       -> 'mountain'
ge-berg-te -> 'mountains'

Content Morphenes:
carry some semantic meaning

Functional Morphemes:

> Inflectional Morphology
grammartical: number, tense, case
eg: bring, brought, brings, bringing

> Derivational Morphology
Creates a new words by changing parts-of-speech: logic, logical, illogical, illogicality, logician

Concatenation:
process of joining morphemes
eg: hope + less

Phonological/graphemic changes:
book + s[s], shoe + s[z]
happy + er -> happier

> Reduplication: part of word or entire word in doubled.

> Suppletion
'irregular' relation between the words
go - went, good - better

> Morphological Internal Changes
sing -> sang -> sung

> Compounding
words formed by combinig two words

rain + bow, over + do

> Acronyms

> Blending
breakfast + lunch = brunch
smoke + fog -> smog

> Clipping
laboratory -> lab
dormitory  -> dorm

> Lemmatization

word -> lemma

saw -> {saw, see}

> Morphological Analysis
saw -> {saw | verb.past, saw: noun}

> Tagging
Peter _saw_ her -> { see: verb.past }

> Morpheme Segmentation
de-nation-al-iz-ation

> Generation
see + verb.past -> saw

> Issues:

1. boy -> boys
    fly -> flys -> flies

2. Toiling -> toil
    duckling -> duckl

3. Getter -> get + er
    Doer -> do + er
    Beer -> be + er?

To solve these we must know stems, we need lexicons or dictionary.

Morphotatctics: which classes of morphemes the follow

Size Problems with lexicon:

Eng: 317K forms for 90K lexical entries
Sanskrit: 11M for 170K

---

# Lecture 3

# Finite state methods for morphology

> Finite State Automata
A directed graph with nodes(states) and egdes with labels.
Start and accepting state.
Recognizes regular languages. i.i., language defined by regular expressions.

Define the language using NFA. like noun + s for plural.
eg: car + s = cars

> Properties:

1. Recongnize word in linear time
2. Convert to Automaton with minimum states
3. NFA -> DFA

Not sufficient for morphological analysis.
Eg: find lemma for a word.

## Transducers

Translates one string to another
It has two strings on each edge.

> Two-level morphology

Eg: cat -> cats
    fox -> foxes

Lexical       F O X
Intermediate  F O X ^ S
Surface       F O X E S

Place a placeholder (^) to fill in at another place.

Ways to address phonological/graphemic variations:

1. Linguistic approach: components with concatenation
2. Engineering approach: into endings

---

# Lecture 4

# Tagging

Given a text, identify parts of speech.

THe -> pronoun
boy -> noun

1. Open Class Words
nouns, verbs, adjectives, adverbs
new ones keep on adding

2. Close Class Words
pronouns, determiners, prepostions, connectives
they are limited.

Using the UPenn tagset,

The grand jury commented on a  topic.
DT  JJ    NN   VBD       IN DT NNS

> Why is tagging hard?

eg:
The back door: back/JJ
on my back: back/NN

We require context to tag correctly.

Ambiguity in the Brown corpus:
1. 40% of word tokens are ambigious
2. 12% of word types are ambiguous.

For a given word, a tag might be more possible than another.
eg: race

Help:

1. Some words may only be nouns
2. Some words are ambiguous

Local context:
1. Two determiners rarely folow each other
2. Two base form verbs rarely follwo each other
3. Det almost always followed by an adj

> Rule based approach
1. Assign each word a list of tags
2. winnow down to a single list using rules

> Statistical tagging
1. A pre-tagged text with probabilities

mark word as MD change to NN when NN/DT.

## Types of models,
Philosphy of how we choose the model and its flow.

1. **Generative (Joint) Models**
    Class generates the data.
    eg: naive Bayes, Hidden Markov Models, etc.

    joint P(x, y)

    class -> [data]

2. **Discrinminative Conditional Models**
    The hidden data is generated from the class.
    eg: Logistic Regression, maximum entropy models, etc.

    posterior P(y | x)

    [data] -> class

Others: SVMs, perceptron, etc.

---

# Lecture 5

# Hidden Markov Models

In generative model.
Probablities for

Words = w1, ... , wn
Tags = t1, ... tn

T' = argmax P(T|W)
   = argmax P(t1, ... , tn | w1, ... , wn)
   = argmax P(W|T) P(T)
            -----------
                P(W)

P(W) is common

   = argmax P(W|T) P(T)

P(W|T) = P(w1 ... wn|t1...tn) P(t1...tn)

   = argmax prod(i = [1, n]) P(wi|w0...wi - 1)P(ti|t0...ti - 1)

   = P(wi|ti) P(t1|ti - 1) [Bigram Assumption] 

P(ti|ti - 1) = C(ti-1, ti)
               -----------
                C(ti - 1)

P(NN|DT) = C(DT, NN) =  56509  = 0.49
           ---------   -------
            C(DT)       116454

## Markov Chain
First order Markov Model

eg: Weather example
Three types of weather: sunny, rainy, foggy
q(n): variable denoting the weather on the nth day
P(qn|qn - 1...q1) = P(qn|qn - 1)

## Hidden Markov Models

tag Transition probabilities p(ti|ti - 1)

        Tomorrow
        Sunny       Rainy       Foggy
Todays

Sunny   .8          .05         .15
Rainy   .2          .6          .2
Foggy   .2          .3          .5

Q: P for sunny after sunny after rainy?
P(S|S, R) = P(R) P(S|R) P(S|S, R)

Use a generative model with states and emitions.

In POS tagging,

states = words
outputs = tags

Elements of Hidden Markov Model:

1. states (tags)
2. outputs (words)
3. Initial state (beginnning of sentence)
4. State transition P(tn|tn - 1)
5. Symbol emission P(wi|ti)
