---
Week 4
---

# Lecture 1

Find best possible sentence from a list of tags.

## Viterbi Decoding

Intiution:
Optimal path for each state can be recorded. We need:

1. Cheapest cost to state j at step s: delat(j)(s)
2. Backtrace from that state to best predecessor phi(j)(s)

Computing values:

1. delta(j)(s + 1) = max(1 <= i <= N) delta(i)(s) P(tj | ti) P(W(s + 1)|tj)
2. phi(j)(s + 1) = argmax(1 <= i <= N) P(tj|ti) P(w(s + 1)|tj)

Final state = argmax[1, N]delat(i)(|S|), we can backtrace from there.

> Learning the Parameters
Two Scenarios:
1. Labeled Dataset
2. Corpus is available not labels

Methods for these scenarios:
1. Maximum Likelihood estimate
2. Baum-Welch Algorithm

---

# Lecture 2

# Baum-Welch Algorithm


