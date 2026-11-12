# Idea-ranking rubric (NeurIPS 2027 main track, Dan + mentor Xunjian Yin)

Score each dimension 1–5 (half points allowed). Weighted total out of 100 = sum(weight × score) / 5 × ... (computed as Σ wᵢ·sᵢ / Σ wᵢ × 20).

| # | Dimension | Weight | 5 = | 1 = |
|---|---|---|---|---|
| N | **Novelty / un-scooped** | 3 | No paper (incl. Jul–Nov 2026 arXiv) asks this question or runs this experiment; closest work leaves a clearly stated gap | Already done, or ≥2 close papers in last 6 months |
| S | **Surprise / falsifiable claim** | 2.5 | A one-sentence claim that, if true, changes how people build or evaluate self-improving agents; either outcome is publishable (e.g., ranking flip, Goodhart curve, "X doesn't compound") | Only incremental "+3 pts" result possible; null result unpublishable |
| R | **Relevance to mentor's agenda** | 2 | Directly about self-referential agents / meta-agents / self-improvement over long horizons / world models; builds on Gödel Agent, WeAct, BreakingWeb | Tangential |
| C | **NeurIPS competitiveness / rigor** | 2 | Clean experimental design (seeds, matched compute, held-out, multiple models/benchmarks), general insight beyond one benchmark, a reusable artifact (metric, protocol, generator) | Workshop-level; single benchmark; reviewers will say "engineering" |
| F | **Feasibility** (solo undergrad + mentor, ~6 months to mid-May 2027, ≤ ~$3k API, ≤ modest GPU) | 1.5 | Pilot in a week for <$50; full study < $1.5k; open/cheap models usable | Needs >$10k or frontier-only models or infra we don't have |
| T | **Trend / timing** | 1 | Rides a hot 2026 trend (harness evolution, RSI evaluation, agent plasticity, environment generation) while still ahead of the wave | Fading or saturated area |

Hard kill flags (any one → reject unless fixed): (K1) a 2026 paper already makes the same central claim with the same design; (K2) no outcome would be interesting (null result unpublishable); (K3) requires closed-model access we can't afford; (K4) the "contribution" is just combining two existing things ("we co-evolve both").

Score total = 20 × (3N + 2.5S + 2R + 2C + 1.5F + 1T) / 12. ≥75 strong, 60–75 viable with fixes, <60 park.
