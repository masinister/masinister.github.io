---
title: 'A Note on the Hardness of Verifying and Generating Reductions'
date: 2026-01-09
permalink: /posts/2026/01/reduction-correctness/
tags:
  - complexity theory
  - NP-completeness
  - polynomial hierarchy
---

<style>
@media (min-width: 64em) {
  .page {
    width: 80% !important; 
    margin-right: 0 !important;
    padding-right: 2em !important;
  }
}

.box-blue,
.box-red,
.box-green,
.box-black {
  display: block;
  box-sizing: border-box;
  width: 100%;
  padding: 0.5em 1em;
  margin: 1em 0;
  background: #f8f9fa;
}

.box-blue > :first-child,
.box-red > :first-child,
.box-green > :first-child,
.box-black > :first-child {
  margin-top: 0;
}

.box-blue > :last-child,
.box-red > :last-child,
.box-green > :last-child,
.box-black > :last-child {
  margin-bottom: 0;
}

.box-blue { border-left: 3px solid #2196F3; }
.box-red  { border-left: 3px solid #f44336; }
.box-green{ border-left: 3px solid #4CAF50; }
.box-black{ border-left: 3px solid #000; background: #f5f5f5; }

.theorem-box {
  text-align: center;
  border: 2px solid #f44336;
  padding: 0.75em;
  margin: 1em 0;
  background: #fff5f5;
  box-sizing: border-box;
  width: 100%;
}

.sc { font-variant: small-caps; }

.qed { text-align: right; margin-top: 0.5em; font-weight: 600; }
</style>

This is a follow-up to my earlier post on [The Karp Dataset](/posts/2026/01/karp-dataset-jmm/) meant to clarify the sense in which reductions are hard.

---

## Preliminaries

We work with binary strings over the alphabet $\\{0,1\\}$. Let $\\{0,1\\}^n$ denote the set of all $n$-bit strings, and $\\{0,1\\}^* = \bigcup_{n \ge 0} \\{0,1\\}^n$ denote the set of all finite binary strings. For a string $x$, we write $\lvert x \rvert$ for its length.

<div class="box-blue" markdown="1">
**Definition (Language).** A *language* (or *decision problem*) is a subset $L \subseteq \\{0,1\\}^\*$.

The *membership problem* for $L$ is: given $x \in \\{0,1\\}^*$, decide whether $x \in L$.
</div>

<div class="box-blue" markdown="1">
**Definition ($\mathsf{P}$).** A language $L$ is in $\mathsf{P}$ (polynomial time) if there exists a deterministic Turing machine $M$ (a "decider") and a polynomial $p$ such that for all $x \in \\{0,1\\}^*$:
- $M(x)$ halts in at most $p(\lvert x \rvert)$ steps, and
- $M(x) = 1$ if and only if $x \in L$.
</div>

<div class="box-blue" markdown="1">
**Definition ($\mathsf{NP}$).** A language $L$ is in $\mathsf{NP}$ (nondeterministic polynomial time) if there exists a polynomial-time computable predicate $R$ (a "verifier") and a polynomial $s$ such that

$$x \in L \;\Longleftrightarrow\; \exists w \in \\{0,1\\}^{\le s(\lvert x \rvert)} : R(x, w) = 1.$$

Here, we call $w$ a *witness* for $x$. A language is in $\mathsf{coNP}$ if its complement is in $\mathsf{NP}$.
</div>

<div class="box-blue" markdown="1">
**Definition (Polynomial Hierarchy).** The *polynomial hierarchy* is defined inductively:
- $\Sigma_0^p = \Pi_0^p = \mathsf{P}$.
- $\Sigma_{k+1}^p = \mathsf{NP}^{\Sigma_k^p} = \mathsf{NP}^{\Pi_k^p}$ (NP with oracle access to $\Sigma_k^p$ or, equivalently, $\Pi_k^p$).
- $\Pi_{k+1}^p = \mathsf{coNP}^{\Sigma_k^p} = \mathsf{coNP}^{\Pi_k^p}$ (coNP with oracle access to $\Sigma_k^p$ or $\Pi_k^p$).

Equivalently, $L \in \Sigma_k^p$ iff there exist a poly-time predicate $R$ and polynomials $s_1, \ldots, s_k$ such that

$$x \in L \;\Longleftrightarrow\; \exists w_1 \forall w_2 \exists w_3 \cdots Q_k w_k : R(x, w_1, \ldots, w_k),$$

where $\lvert w_i \rvert \le s_i(\lvert x \rvert)$ and the quantifiers strictly alternate starting with $\exists$. For $\Pi_k^p$, the quantifiers start with $\forall$.
</div>

Note that $\Sigma_1^p = \mathsf{NP}$ and $\Pi_1^p = \mathsf{coNP}$.

<div class="box-blue" markdown="1">
**Definition (Polynomial-time Reduction).** A *polynomial-time reduction* from $F$ to $G$ is a poly-time computable function $M$ satisfying:

$$x \in F \;\Longleftrightarrow\; M(x) \in G \quad \text{for all } x.$$

</div>

<div class="box-blue" markdown="1">
**Definition (Predicate Schema).** A *predicate schema* of depth $k$ is a tuple

$$S = (Q_1 w_1, \ldots, Q_k w_k : R(x, \vec{w});\; p, s_1, \ldots, s_k)$$

where:
- Each $Q_i \in \\{\exists, \forall\\}$ with **strict alternation**: $Q_{i+1} \neq Q_i$ for all $i < k$.
- $R : \\{0,1\\}^* \times \\{0,1\\}^* \times \cdots \times \\{0,1\\}^* \to \\{0,1\\}$ is a predicate computed by a deterministic Turing machine in time polynomial in $\lvert x \rvert + \sum_i \lvert w_i \rvert$. Since each $\lvert w_i \rvert \le s_i(\lvert x \rvert)$ for polynomials $s_i$, the runtime is equivalently $\mathrm{poly}(\lvert x \rvert)$.
- $p, s_1, \ldots, s_k : \mathbb{N} \to \mathbb{N}$ are polynomials.

The schema defines the language

$$L_S = \\{x \in \\{0,1\\}^* : Q_1 w_1 \in \\{0,1\\}^{\le s_1(\lvert x \rvert)} \cdots Q_k w_k \in \\{0,1\\}^{\le s_k(\lvert x \rvert)}\, R(x, w_1, \ldots, w_k)\\}.$$

We define $\mathrm{depth}(S) = k$, the number of quantifier blocks in the schema.
</div>

**Examples:**
- Depth $0$ reduces to $(R(x); p)$ with no quantifiers, so $L_S = \\{x : R(x)\\}$ where $R$ is simply a polynomial-time computable predicate. This captures exactly $\mathsf{P}$.
- Depth $1$ with leading $\exists$ captures $\mathsf{NP}$: "is there a witness $w$ such that $R(x,w)$ holds?"
- Depth $1$ with leading $\forall$ captures $\mathsf{coNP}$: "for all $w$, does $R(x,w)$ hold?"
- A depth-$k$ schema with leading $\exists$ defines a $\Sigma_k^p$ language; with leading $\forall$, a $\Pi_k^p$ language.

**Conventions.** Bounds $n, t, \ell, s$ are given in unary (e.g., $1^t$ rather than the binary representation of $t$). This ensures that checking "$M(y)$ halts in $\le t$ steps" is polynomial-time in the input size. Machines and circuits have polynomial-size encodings. We write $\Sigma_k$-QSAT (resp. $\Pi_k$-QSAT) for the canonical $\Sigma_k^p$-complete (resp. $\Pi_k^p$-complete) quantified Boolean formula satisfiability problem with $k$ alternating quantifier blocks.

---

## Verification

Given a candidate reduction $M$, bounds $1^n, 1^t$, and specifications of languages $F$ and $G$, does $M$ correctly reduce $F$ to $G$ on all $n$-bit inputs in time $t$? Verifying correctness requires checking a universal property—$M$ must preserve membership for *every* input—which introduces one quantifier alternation beyond the complexity of the language specifications themselves.

### General Verification Theorem

We first establish the general result for languages specified by predicate schemas. The important special cases of circuits and $\mathsf{NP}$ verifiers then follow as immediate corollaries.

<div class="box-blue" markdown="1">
**Definition (<span class="sc">Correct</span>).** 

**Input:** Tuple $\langle M, S_F, S_G, 1^n, 1^t, 1^\ell \rangle$.

**Question:** For all $y \in \\{0,1\\}^n$, does $M(y)$ halt in $\le t$ steps with $\lvert M(y) \rvert = \ell$, and satisfy

$$y \in L_{S_F} \;\Longleftrightarrow\; M(y) \in L_{S_G}\,?$$

</div>

<div class="box-red" markdown="1">
**Theorem 1.** Let $k = \max(\mathrm{depth}(S_F), \mathrm{depth}(S_G))$. Then <span class="sc">Correct</span> is $\Pi_{k+1}^p$-complete.
</div>

<div class="box-black" markdown="1">
*Proof.* Write $A(y)\equiv y\in L_{S_F}$ and $B(z)\equiv z\in L_{S_G}$. Each is a depth-$k$ quantified predicate.

*Membership.* The correctness condition is $\forall y\,(A(y)\Leftrightarrow B(M(y)))$. Negating gives

$$\exists y\;\bigl[(A(y)\land\neg B(M(y)))\lor(\neg A(y)\land B(M(y)))\bigr].$$

Writing $A$ and $B$ as depth-$k$ quantified formulas, the negation is a disjunction of two terms: $(A \land \neg B)$ and $(\neg A \land B)$. Each term is a conjunction of a $\Sigma_k^p$ predicate with a $\Pi_k^p$ predicate (since negation flips the leading quantifier). Concatenating these quantifier prefixes increases the alternation depth by at most one, so each term lies in $\Sigma_{k+1}^p$. A disjunction of $\Sigma_{k+1}^p$ predicates remains in $\Sigma_{k+1}^p$. Therefore the negation of the correctness condition lies in $\Sigma_{k+1}^p$, and the original universal property is in $\Pi_{k+1}^p$.

*Hardness.* Reduce from $\Pi_{k+1}$-QSAT. Given $\Phi=\forall u\,Q_1w_1\cdots Q_kw_k:\psi(u,\vec w)$, construct an instance of <span class="sc">Correct</span> as follows: set $S_F$ to encode the depth-$k$ predicate $Q_1w_1\cdots Q_kw_k:\psi(u,\vec w)$ (with $u$ as the input variable), set $S_G$ to be the trivial always-true language, and set $M=\mathrm{id}$ (the identity function). Then the correctness condition becomes $\forall u\,(u\in L_{S_F}\Leftrightarrow\mathsf{true}) = \forall u\,(u \in L_{S_F})$, which holds if and only if $\Phi$ is true. This is a polynomial-time reduction, establishing $\Pi_{k+1}^p$-hardness.

<div class="qed">$\square$</div>
</div>

### Corollaries for Circuits and NP Verifiers

The most natural ways to specify decision problems—Boolean circuits and $\mathsf{NP}$ verifiers—correspond to depth $0$ and depth $1$ schemas, respectively. Applying the general theorem immediately yields:

<div class="box-green" markdown="1">
**Definition (<span class="sc">Correct-Circ</span>).** 

**Input:** Tuple $\langle M, C_F, C_G, 1^n, 1^t, 1^\ell \rangle$ where $C_F$ (resp. $C_G$) is an $n$-input (resp. $\ell$-input) Boolean circuit.

**Question:** For all $y \in \\{0,1\\}^n$: $M(y)$ halts in $\le t$ steps, $\lvert M(y) \rvert = \ell$, and $C_F(y) = C_G(M(y))$?
</div>

<div class="box-green" markdown="1">
**Corollary 1.** <span class="sc">Correct-Circ</span> is $\mathsf{coNP}$-complete.
</div>

<div class="box-black" markdown="1">
*Proof.* A Boolean circuit defines a depth-$0$ predicate (no quantifiers, just a polynomial-time computation). By Theorem 1 with $k = 0$, verification is $\Pi_1^p = \mathsf{coNP}$-complete. Concretely, the correctness condition $\forall y : C_F(y) = C_G(M(y))$ is a single universal quantifier over a polynomial-time predicate. For hardness, reduce from Circuit-Tautology by setting $M = M_{\mathrm{id}}$, $C_G \equiv 1$, so correctness becomes "$C_F$ is a tautology." 

<div class="qed">$\square$</div>
</div>

<div class="box-green" markdown="1">
**Definition (<span class="sc">Correct-NP</span>).** 

**Input:** Tuple $\langle M, V_F, V_G, 1^n, 1^t, 1^\ell, 1^{s_F}, 1^{s_G} \rangle$ where $V_F, V_G$ are $\mathsf{NP}$ verifiers.

**Question:** For all $y \in \\{0,1\\}^n$: $M(y)$ halts in $\le t$ steps, $\lvert M(y) \rvert = \ell$, and

$$(\exists w\, V_F(y, w) = 1) \;\Longleftrightarrow\; (\exists w\, V_G(M(y), w) = 1)\,?$$

</div>

<div class="box-green" markdown="1">
**Corollary 2.** <span class="sc">Correct-NP</span> is $\Pi_2^p$-complete.
</div>

<div class="box-black" markdown="1">
*Proof.* An $\mathsf{NP}$ verifier defines a depth-$1$ predicate with leading $\exists$: $x \in L$ iff $\exists w\, V(x, w)$. By Theorem 1 with $k = 1$, verification is $\Pi_2^p$-complete. The correctness condition expands to

$$\forall y : (\exists w_F\, V_F(y, w_F)) \Leftrightarrow (\exists w_G\, V_G(M(y), w_G)).$$

To see this is in $\Pi_2^p$: the negation of the biconditional $P \Leftrightarrow Q$ is $(P \land \neg Q) \lor (\neg P \land Q)$. When $P = \exists w_F\, V_F(\cdot)$ and $Q = \exists w_G\, V_G(\cdot)$, both are $\Sigma_1^p$ predicates, so $\neg P$ and $\neg Q$ are $\Pi_1^p$. The negation is thus a disjunction of two terms, each being a conjunction of a $\Sigma_1^p$ predicate with a $\Pi_1^p$ predicate. Concatenating these quantifier prefixes yields a $\Sigma_2^p$ formula for each term. The negation of the correctness condition is therefore $\exists y$ plus a $\Sigma_2^p$ predicate, which lies in $\Sigma_2^p$; hence <span class="sc">Correct-NP</span> is in $\Pi_2^p$. Hardness follows from Theorem 1, or directly: reduce from $\Pi_2$-QSAT $\forall u\, \exists v : \varphi(u, v)$ by setting $V_F(u, v) = \varphi(u, v)$, $V_G \equiv 1$, and $M = M_{\mathrm{id}}$.

<div class="qed">$\square$</div>
</div>

---

## Generation

Given specifications $S_F, S_G$, does there *exist* a correct reduction?

<div class="box-blue" markdown="1">
**Definition (<span class="sc">Reducible</span>).** 

**Input:** Tuple $\langle S_F, S_G, 1^n, 1^t, 1^\ell, 1^s \rangle$.

**Question:** Does there exist a machine $M$ with $\lvert M \rvert \le s$ such that $\langle M, S_F, S_G, 1^n, 1^t, 1^\ell \rangle$ is in <span class="sc">Correct</span>?

</div>

<div class="box-red" markdown="1">
**Theorem 2.** Let $k = \max(\mathrm{depth}(S_F), \mathrm{depth}(S_G))$. Then <span class="sc">Reducible</span> is $\Sigma_{k+2}^p$-complete.
</div>

<div class="box-black" markdown="1">
*Proof.*

*Membership.* The question is

$$\exists M\ (|M|\le s)\; \bigl(\forall y\; A(y)\Leftrightarrow B(M(y))\bigr).$$

By Theorem 1 the inner predicate "$\forall y\;A(y)\Leftrightarrow B(M(y))$" is in $\Pi_{k+1}^p$, and the outer $\exists M$ (over descriptions of polynomial length since $s$ is unary) yields membership in $\Sigma_{k+2}^p$.

*Hardness.* Reduce from $\Sigma_{k+2}$-QSAT. Given

$$\Phi=\exists x\;\forall y\; Q_1w_1\cdots Q_k w_k:\psi(x,y,\vec w),$$

where $x \in \\{0,1\\}^r$ for some $r$ polynomial in the formula size, encode each candidate $x$ by a machine $M_x$ that outputs $x \| y$ on input $y$. Such a machine has description size $\lvert M_x \rvert = r + O(1)$. Since the size bound $s$ is given in unary, we can set $s$ to accommodate all machines of this form, and it is legitimate to quantify over all descriptions of size $\le s$. Put $S_F=\\{0,1\\}^n$ (the always-true language) and let $S_G$ interpret its input $z$ as a pair $(x,y)$ and test $Q_1\cdots Q_k\;\psi(x,y,\vec w)$. Then $M_x$ is a valid reduction iff $\forall y\;Q_1\cdots Q_k\;\psi(x,y,\vec w)$, so $\exists M$ passing <span class="sc">Correct</span> iff $\exists x\,\forall y\,Q_1\cdots Q_k\;\psi$, i.e., iff $\Phi$ holds. This is a polynomial-time reduction, establishing $\Sigma_{k+2}^p$-hardness.

<div class="qed">$\square$</div>
</div>

**Remark.** The unbounded problem ("does a poly-time reduction exist for all input lengths?") is undecidable. Our bounded version captures the complexity of finding reductions for a specific input length.

<div class="box-blue" markdown="1">
**Definition (<span class="sc">Reducible-Unbounded</span>).** 

**Input:** Turing machines $M_F, M_G$ specifying languages $L_F = L(M_F)$ and $L_G = L(M_G)$.

**Question:** Does there exist a polynomial-time computable function $f$ such that for all $x \in \\{0,1\\}^*$:

$$x \in L_F \;\Longleftrightarrow\; f(x) \in L_G\,?$$

</div>

<div class="box-red" markdown="1">
**Theorem 3.** <span class="sc">Reducible-Unbounded</span> is undecidable.
</div>

<div class="box-black" markdown="1">
*Proof.* We reduce from the language emptiness problem: given a Turing machine $T$, is $L(T) = \emptyset$? This is a non-trivial semantic property of Turing machines, hence undecidable by Rice's theorem. (Alternatively, one can show this directly via reduction from the halting problem.)

Given $T$, construct an instance of Reducible-Unbounded with $L_F = L(T)$ and $L_G = \emptyset$ (the empty language, specified by a machine that immediately rejects).

We claim: a polynomial-time reduction from $L_F$ to $L_G$ exists if and only if $L_F = \emptyset$.

- ($\Rightarrow$) Suppose $f$ is a reduction and $L_F \neq \emptyset$. Let $x \in L_F$. Then by correctness of $f$, we need $f(x) \in L_G = \emptyset$, a contradiction.

- ($\Leftarrow$) If $L_F = \emptyset$, then any function $f$ is a valid reduction: the condition $x \in L_F \Leftrightarrow f(x) \in L_G$ becomes $\mathsf{false} \Leftrightarrow \mathsf{false}$, which holds for all $x$.

Thus Reducible-Unbounded decides language emptiness, so it is undecidable.

<div class="qed">$\square$</div>
</div>

---

<small>*Disclosure: AI tools were used to assist with editing this post.*</small>
