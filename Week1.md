## Why NLP is hard?

1. Lexical Ambiguity
    eg -> Will      Will    will   Will's   will?
           |         |       |      |        |
           v         v       v      v        v
        Modal Verb  Name    verb   Name     noun

    eg -> Rose rose to put rose roes on her rows of roses.

2. Language imprecision and vagueness 
    (Structural Ambiguity)
    eg -> The man saw the boy with the binoculars.
                          |---------------------|
                             a boy with bino
              |---------------------------------|
                        man used bino

    eg -> It is very warn here.

    eg (joke) -> Why is the teacher wearing sun-glasses?
            Because the class is so bright.


3. News Headlines
    eg -> Teacher strikes idle kids
    eg -> Hospital are sued by 7 foot doctors

Activity: find at least 5 meanings of:
    I made her duck.
    a. made her bend low
    b. cooked duck for her
    c. cooked duck bought by her
    d. made a duck from clay like a toy
    e. transformed her into a duck

    I made(create / cook) her(of / for) duck(noun / verb)

> Inability to diferentiate the phonetics and context.

4. Catalan numbers
    Number of possible meanings depending on the number of phrases present in the sentence.

Diff b/w NL and computer lang:
1. no ambiguity
2. prog lang designed for efficient & deterministic parsing.

5. Non standard English
    used on social media
    eg -> C U.. I'll txt u l8r (see you, i will text you later)

6. Segmentation issues
    eg -> the New York-New Haven Railroad
              |------| |-------|
                  |-------|

7. Use of idioms
    eg -> (dark horse) more than 1 meaning
    eg -> ball in your court
    eg -> burn the midnight oil

8. Neologisms (new words)
     eg -> unfriend, retweet, google

9. New sense's of word
    eg -> sick (unwell or cool)

10. Tricky New Entities
    eg -> Where is A Bug's Life playing ...
    eg -> Let It Be was recorded ...

### Tools Required
    1. knowledge about lang and world

    Use prob models!
    P("mansion" -> "house") is high.


---


## Empirical Laws

### Function words vs Content Words

    func = lower lexical meaning, for structuring sentences(make grammartical like articles, pronouns, prepositions, etc.)

    cont = they convey important meanings in a sentence(nouns and adjectives)

    freq(func) >>> freq(cont) in any english literature

### Type-Token distinctions
    
    Concept       Instances of concept
    |-----|       |------------------|
     type               tokens

    type = unique words
    tokens = number of words

    Type/Token Ratio = unique words count / number of occuring words
    It helps identify the rate at which new word in introduced in the text.

    eg:
    ttr(_Mark Twain's Tom Sawyer_) = 0.112
    
    Valid Range for ttr = (0, 1]

    high ttr -> tendency to use new words (eg, news)
    low ttr -> same words usage (eg, conversation)
    
    inc in number of tokens = dec in ttr

    Better measure for ttr: run a window of 1000 tokens and then take mean of all.
    
    Reverse analogy,
    
    Word freq       freq of freq
        1               3993
        2               1292
        3                664
        -                -

    3993(50%) words types appear only once.(greek: happax legomena - read only once)

### Zipf's law
    
        f = k/r

    Says that the frequency of a word appearing a text is *inversely proportional* to the rank of it  when arranged in decreasing order of their frequency.

    eg, f1 = 50 and f2 = 150
        f1 = 3.f2
    
    Probability(r) = f(r)/N

    Correlation = Number of meanings and word frequency
        m = k.(f^1/2) = k / (r^1/2)

    Correlation: word lenght and word frequency
        f = k / len

    Observation:
    1. 50% of vocab(types) occur only once(rare words)
    2. ~100 words occured for 50% of the total (common words)

### Vocabulary Growth
    
    1. Heaps's Law
        V = size of vocab
        N = no. of tokens

            |V| = K(N^b)

        k ~ 10 - 100
        b ~ 0.4 - 0.6


---

# Text Processing

Tokenization = process of segemwnting a string into words

Sentence Segmentation
Challenges involved:

1. ! ? are unambiguous
2. . for abbreviations, floats
    Approach: build a binary classifier via if-else, regex or ml(like decision tree)

    Popular algorithms: ID3, CART, C4.5 - to identify the splits.

    Also = SVM, log reg and NNs.
    
    Choose correct set of features for unambiguous tokenization.

Tokenization is language based.

Libraries:
1. NLTK toolkit, python
2. Standard CoreNLP, java
3. UNIX cmds

Issues in tokenization:

1. Finland's -> Finland Finlands Finland's?
2. San Franciso -> one token or two
3. m.p.h. -> ???

For info retrieval, use the same convention for documents and queries.

### Handling Hyphenation

types,
1. End-of-Line hyphens: initia-tive
2. Lexical hyphens: co-, pre-
2. Semantically determined: case-based

Language specific interpretations,

1. French
    l'ensemble should be mapped to un ensemble
2. German
    There could be big ass words = compounded together. SO, we need a compound splitter.
3. Chinese, Japanese and Sanskrit
    Words written without spaces.
    Induces word-forming ambiguity.
    Sanskrit also has word concatenation(sandhi) operation problem.

## Operations on Sanskrit

    Longest words in languages:
    Sanskrit(431 letters)  >>  English

    Now to perform "Word Segmentation" on Sanskrit words we use Greedy algorithms like Maximum Matching.

    Sanskrit
    ^
    |
    |
    pointer keeps on moving

    In case of English, it is for hashtags. eg: #musicmondays.

    General assumption behind the design:

    Sentences from Classical Sanskrit may be generated bby a regular relation R of the Kleene closure W* of a regular set W of words over a finite alphabet E(sigma).

    W = vocab of (inflected) words (padas)
    R = sandhi

    Analysis of a sentences:
    inverse the R over all W.

### Normalization

    eg: U.S.A. and USA should be kept together
    1. Reduce all letters to lower case
        exception nouns.

### Lemmatization
    
    Reduce inflections or variant forms to base form:
        am, are, is -> be
        car, cars, car's -> car
    
    How to find the correct dictionary head word? ans. Morphology.

### Morphology
    Studies the internal structure of words and how they are built up from smaller meaningful units called *morphemes*.

    Stems: core meaning bearing units
    Affixes: Bits and pieces adhering to stems
        un-, anti- (preffixes)
        -ity, -ation (suffixes)

### Stemming
    Striping affixes to obtain the stem word.
    eg:
        automatic -> automat
    
    Algorithms: Porter's algorithm
    
    Step 1:
        sses -> ss (caresses -> caress)
        (*vowel*)ing -> phi (walking -> walk)
        ... many if-else rules
    
    Step 2:
        ational -> ate (relational -> relate)

