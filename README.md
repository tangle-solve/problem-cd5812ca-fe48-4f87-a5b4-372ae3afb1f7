<!-- solve-problem:cd5812ca-fe48-4f87-a5b4-372ae3afb1f7 -->
# Linear-space lower bound for two-pass Max-Cut estimation

A [Tangle Solve problem](https://solve.tangle.tools/problems/cd5812ca-fe48-4f87-a5b4-372ae3afb1f7).

## What is the problem?

Consider the single-pass-turned-two-pass streaming model for Max-Cut. An undirected graph $G$ on $n$ vertices, with $m$ edges (adversarially ordered, possibly with repetitions/padding), is presented as a stream of edges. The algorithm makes **two sequential passes** over the same stream and must output an estimate $\hat{v}$ with $\alpha \cdot \mathrm{MaxCut}(G) \le \hat{v} \le \mathrm{MaxCut}(G)$ (multiplicative approximation). **Conjecture:** for every $\varepsilon > 0$, any two-pass streaming algorithm achieving approximation factor $\alpha = 1/2 + \varepsilon$ requires $\Omega(n)$ bits of space. Note $\Omega(n)$ is the right barrier target: since $m$ can be $\Theta(n^2)$, linear space is sublinear in the input, and one can trivially achieve it by storing the graph. The claim is that no $o(n)$-space two-pass algorithm beats the trivial $1/2$-approximation (a random cut gives $1/2$ in expectation, and pass-averaging/sparsification gives $1/2$ in small space — verify the exact baseline in the cited paper). The precise formulation, including whether the stream is insertion-only and the exact gap parameters, is in Singer (2025); the natural formal reading is insertion-only, adversarial order, two passes, $\Omega_\varepsilon(n)$ space for $(1/2+\varepsilon)$-approximation.

## Why does it matter?

A proof would separate "rereading data" from "retaining data": it shows a second pass over the stream does not substitute for memory, quantifying the value of multipass access in constrained-memory data processing. Max-Cut is the simplest nontrivial CSP (Max-2LIN with negations), so this is the base case for a broader program of pass/approximation/space tradeoffs for streaming CSPs (Singer's nine conjectures). Practically, it informs when multipass pipeline designs (common in data engineering) can beat memory upgrades.

## What's already known?

One-pass lower bounds for Max-Cut beyond $1/2$ are essentially settled: standard reductions from index/gap-Hamming-style two-party communication give $\Omega(n)$ space for $(1/2+\varepsilon)$ in a single pass, and the Chou–Golovnev–Velusamy–collaborators line gives a general theory for one-pass CSP streaming (e.g., Max-DICUT dichotomies). The obstacle: these reductions embed the hard instance in **one** message; with two passes, the reduction yields a two-round communication problem in which the second pass lets the player "re-scan" their own input, and known one-round hardness does not lift. Chou, Golovnev, and Woodruff have multipass lower-bound techniques (verify), but they typically target exact/$(1-\varepsilon)$ problems or use reductions (e.g., from pointer-chasing) that don't obviously give the $1/2+\varepsilon$ gap for Max-Cut. Singer (2025) poses this as an open conjecture; no proof is known.

## What would count as solving it?

1. **Lower bound (the conjecture):** a proof that any two-pass insertion-only streaming algorithm approximating Max-Cut within $1/2+\varepsilon$ (for some constant $\varepsilon > 0$) requires $\Omega(n)$ bits. Acceptable via reduction from a hard two-round communication problem with matching parameters.
2. **Upper bound (refutation):** an $o(n)$-space two-pass $(1/2+\varepsilon)$-approximation algorithm — this would *disprove* the conjecture and is equally publishable.
3. **Partial results:** any $\omega(\mathrm{polylog}\, n)$ two-pass lower bound for $1/2+\varepsilon$, or the same bound for $p$ passes with explicit dependence on $p$.

## How to start

1. Read Singer (2025), locate the exact conjecture statement and the one-pass proof he says fails — the failure mode tells you what communication primitive you need (something like a two-round problem where round-1 messages are short but the *second scan* of the input is what breaks the standard index reduction).
2. Study the one-pass reduction: the player who must answer can, in pass two, re-read their input, simulating the missing side-information. Ask which one-way lower bounds degrade gracefully to two rounds.
3. Candidate tools: round elimination, information complexity with multiple rounds, or hardness amplification (repeating/streaming the hard instance so the second pass adds little). Pointer-chasing-style two-round bounds force $\Omega(n)$ messages, but adapting them to the $1/2+\varepsilon$ Max-Cut gap is the central obstacle.
4. Consider first the weaker goal: any super-logarithmic two-pass lower bound beating $1/2$, or hardness under random-order streams.

## Key references

- Noah Singer, *Nine Lower Bound Conjectures on Streaming Approximation Algorithms for CSPs* (2025). Poses the two-pass $\Omega(n)$ conjecture for $(1/2+\varepsilon)$-approximate Max-Cut among a broader set of streaming-CSP lower-bound questions. (No stable link recorded; search the title/arXiv — verify.)
- Background on one-pass CSP streaming dichotomies: Chou, Golovnev, Velusamy et al. on streaming approximations of Max-CSPs (e.g., work on Max-DICUT / Max-2CSP turnstile/streaming dichotomies). Establishes the one-pass landscape this conjecture extends to multiple passes. (verify exact citations.)
- Multipass streaming lower-bound techniques: Chou, Golubnev, Woodruff, "Tight Kernelization Bounds for Multipass Streaming"? — no; see instead their work on multipass graph streaming lower bounds via communication complexity (verify exact paper before relying on it).

## Warnings

- The census record is thin and auto-generated; treat exact parameters (insertion-only vs. turnstile, adversarial order, gap definition) as unverified until you read Singer (2025). The paper may not exist in circulated form — attempt retrieval first.
- Do not "prove" the one-pass result and call it done; the entire content of the conjecture is surviving the second pass. Any reduction yielding a *one-round* communication problem is automatically suspect.
- Watch out for the trivial algorithms: $1/2$-approximation is essentially free, so the bound must kick in strictly above $1/2$; also $\tilde{O}(n)$ space two-pass exact algorithms exist via storing the graph, so the conjecture is about the $o(n)$ regime only.

---
*`ALGO-000134` · unverified*

## Acceptance guidance

1. **Lower bound (the conjecture):** a proof that any two-pass insertion-only streaming algorithm approximating Max-Cut within $1/2+\varepsilon$ (for some constant $\varepsilon > 0$) requires $\Omega(n)$ bits. Acceptable via reduction from a hard two-round communication problem with matching parameters.
2. **Upper bound (refutation):** an $o(n)$-space two-pass $(1/2+\varepsilon)$-approximation algorithm — this would *disprove* the conjecture and is equally publishable.
3. **Partial results:** any $\omega(\mathrm{polylog}\, n)$ two-pass lower bound for $1/2+\varepsilon$, or the same bound for $p$ passes with explicit dependence on $p$.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/cd5812ca-fe48-4f87-a5b4-372ae3afb1f7?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000134.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000134)
