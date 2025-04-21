---
Week 6
---

# Lecture 1

# Dependency Grammars and Parsing

> Phrase Structural representation

Tree of tags until words are formed as leaves.

> Dependency Structure Representation

Connects words by putting arrows between words.

                    DOBJ
           |------------------|
    SBJ    |    JOB           v
He <---- sent ----> her  a  letter

Arrows show relation of typed by some rules.

Arrows connect (governor, head, superior, regent) -> (modifier, dependent, inferior, subordinate)

> Criteria for H and D:

1. H determines the syntactic category of C; H can replace C.
2. D specifies H.
3. H is obligatory; D may be optional.
4. H selects D and determines whether D is obligatory.
5. The form of D depends on H (agreement or government).
6. The linear position of D is specified with reference to H.

> Comparision

1. Phrase structures explicitely represent
    Phrases -> non-terminal nodes
    Structural Categories -> non-terminal labels

2. Dependency Structures explicitely represent
    Head-dependent relations (directed arcs)
    Functional Categories (arc labels)

> Dependency Graph

Graph G of set of V nodes and set of A arcs (edges).

V -> labeled with word forms
A -> labeled with dependency types

Arc(wi, d, wj) -> connects wi with wj with label d

> Formal Conditions:

1. **Connectedness**: Syntactic structure is complete
2. **Acyclicity**: structure is hierarichal
3. **Single-head**: every word has at most one syntactic head
4. **Projectivity**: no crossing of deps

> Dependency Parsing:
Given input: x = w1, w2, ..., wn
      output: dep graph G

> Parsing Methods:

1. Deterministic Parsing
2. Maximum Spanning Tree Based
3. Constraint Propagation Based

---

# Lecture 2

# Trasition Based Parsing: Formulation

Derive a single syntactic representation (dependency graph) through a deterministic sequence of elementary parsing actions.

c = (S, B, A)

S = stack of partially processed words
B = buffer of input words remaining
A = set of labeled arcs

Trasition system:
S = (C, T, cs, C1)

C = set of configs
T = set of transitions
cs = initialization function
C1 < C = set of terminal configurations

Initialization: ([]s, [w1, ..., wn]b, {})
Termination: (S, []b, A)

[--DO NUMERICAL--]

---

# Lecture 3

# Transition based parsing: Learning

> Classifier based Parsing:

1. Deterministic parsing requires an _oracle_.
2. An oracle can be approximated by a _classifier_.
3. A classifier can be trained using a _treeback_ data.

[--TODO--]

---

# Lecture 4

# MST-based Dependency Parsing

Starting from all possible connections find the MST.

> Multi-digraph

It is a digraph where multiple arcs between vertices are possible.
(i, j, k) < A: kth arc from i to j

A Directed Spanning Tree of a multi-digraph G(V, A) is a subgraph G'(V', A') such that:
1. V' = V
2. A' < A and |A'| = |V| - 1
3. G' is acyclic

> The MST Problem
Find the spanning tree G' of the graph G that has the highest weight

    G' = argmax Sum(w(i-j, k))

> Chu-Liu-Edmonds Algorithm (Greedy Approach)

1. Each v < V selects highest weight edge
2. If tree, it must be MST.
3. If not, there must be a cycle.
    Identify and contract cycle to a single edge.
    Recalculate edge weights.

eg: John saw Mary.

---

# Lecture 5

# MST-based Dependency Parsing: Learning

Arc weights are linear classifiers.

    w(i -> j, k) = w.f(i, j, k)

Features:

1. Identites of the word wi and wj for a label lk.
    head = saw(Verb) & dep = with(Preposition)

2. Number of words between wi and wj, and their orientation.
    distance = 3 and direction = right

> Now formula,

    G = argmax w.f(G)


