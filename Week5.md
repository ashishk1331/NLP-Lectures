---
Week 5
---

# Lecture 1

# Syntax Parsing

> Syntax + Syntax Tree

> Constituent
A group of words acts as a single unit

> Part of Speech = Substitution test

Words can act as phrases

the man from Amhrest    Noun Phrase
extremely clever        Adjective Phrase
down the river          Prepositional Phrase
killed the rabbit       Verb Phrase

Joe grew potatoes
->
_The man from Amherst_ grew _beautiful russet potatoes_.

> Evidence for constituency exists
They appear in similar environments.
Can be placed in a number of different locations.

eg:
_On December twenty-sixth_ I'd like to fly to Florida.

I'd like to fly _on December twenty-sixth_ to Florida.

> Modeling Constituency = CFG

These rules expresses the ways in which the symbols of the language can be grouped and ordered together.

eg:
Noun Phrase = Proper Noun | Det + Nominal
(Nominal = noun | noun Nominal)

CFG G: (T, N, S, R)
T = set of terminals
N = set of non-terminals
S = start symbol
R = Rules

Here,
terminals = words
non - terminals = POS categories

Given CFG:
NP -> Det Nominal
NP -> Proper Noun
Nominal -> Noun | Noun Nominal

Det -> a | the
Noun -> flight

> How to make "a flight"?

NP -> Det Nominal -> "a" Noun -> "a flight"

This is called _derivation of the string_.
CFG can be used to generate random strings.

CFG defines a formal language = set of all sentences(string of words) that can be derived

Sentences < set = grammartical
Sentences > set = non-grammartical

> Context
It deefines that non-terminal on LHS of rule is independent.

If A -> BC,

1. write A as B followed by C regardless of the context in which A appears
2. Or replace B followed by A regardless of surrounding context

---

# Lecture 2

# Syntax - Parsing I

Parsing = process of deriving all possible strings from a grammar

Start from start symbol S.
Two search methods:

1. Top Down (goal oriented) = build tree starting from root, 

2. Bottom-Up (data-directed) = build tree from ground up, looks for places where RHS fits

eg: try parsing "book that flight"

> Top-Down vs Bottom-Up

1. TD: Aims for full sentences, but may include unmatched parts.
2. BU: Matches parts of the sentence, but may not form a full one.

The amount of wasted search depends on how much the grammar branches.
Use dynamic programming for parsing. (Caching or memoizing for a polynomial time parsing algorithm)

> CKY (Cocke - Kasami - Younger) algorithm
It requires normalizing the grammar.

1. Grammar must be converted to Chomsky normal form (CNF) where
    a. Either exactly two non-terminals on the RHS
    b. Or, 1 terminal symbol on the RHS
    c. only S should produce e
2. Parse bottom-up storing phrases formed from  all subsrtings in a triangular table(chart)

---

# Lecture 3

# Syntax - CYK, PCFGs

> CYK Algorithm

1. n = # of words, separated by n + 1 lines
2. x(ij) denotes the word b/w i & j
3. table for each xij containing all non-terminal spanning fr words i j
4. We build the Table bottom-up

CFG gives rise to ambiguities.

> Probablistic Context-free grammars (PCFGs)

PCFG: G = (T, N, S, R, P)

P(R) gives the P of each rule.

    X<N, sum(P(X -> x)) = 1

> P of trees and strings

1. P(t) = P of tree = prod(P of rules used)
2. P(w) = P of word = sum(P of trees which yield that word)

eg: "Book the dinner flight"

P(t1)  = .0009072
P(t2)  = .0006804
P(w15) = P(t1) + P(t2) = .0015876

> Fetaures:

1. Helps you choose the most likely parse when many trees are possible
2. It uses structure base P, not relationaships.
3. Grammartical errors aren't ruled out - they just get low P.
4. PCFG is worse than n-grams form modeling
5. Samll trees P > larger

> Important Questions:

Given W(1m) be a sentence, G a grammar and t a parse tree,

1. What is the most likely parse of sentence,
    
        argmax P(t|w1m, G)

2. What is the P of a sentence?
    
        P(w1m|G)

---

# Lecture 5

# PCFGs - Inside - outside P

How to find the most likely parse? CYK for PCFG

P(w1m | G) = Sum of P of all trees

Not good, use dynamic algo on inside P


