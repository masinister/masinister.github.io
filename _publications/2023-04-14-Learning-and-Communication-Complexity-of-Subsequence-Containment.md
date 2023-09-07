---
title: "The Learning and Communication Complexity of Subsequence Containment"
collection: publications
permalink: /publication/2023-04-14-Learning-and-Communication-Complexity-of-Subsequence-Containment
excerpt: 'This paper proves tight bounds on the communication complexity and sample complexity (VC dimension) of subsequence-based classification.'
date: 2023-04-14
venue: 'ISIT'
paperurl: 'http://masinister.github.io/files/subsequence.pdf'
---
This paper proves tight bounds on the communication complexity and sample complexity (VC dimension) of subsequence-based classification. In the communication setting, Alice holds a binary sequence $x$, Bob holds a binary sequence $y$, and they want to determine if $y$ appears inside $x$ as a *subsequence* using minimal communication. It turns out that the trivial protocol where Bob sends all of $y$ to Alice is optimal, even when randomized protocols with small probability of error are allowed. In the learning setting, we consider a family of *classifiers* which label binary sequences based on containment of a fixed subsequence (or containment *in* a fixed supersequence). We prove that this family of classifiers has [VC dimension](https://en.wikipedia.org/wiki/Vapnik%E2%80%93Chervonenkis_dimension#Uses) which grows **linearly** with the length of the subsequence in question.

[Download paper here](http://masinister.github.io/files/subsequence.pdf)
