# Skill Benchmark: verified-research

**Model**: claude-opus-5-5 (subagents, single-agent mode); baselines reused from iteration 1
**Date**: 2026-10-03T18:52:54Z
**Evals**: 1, 2, 3, 4, 5 (1 runs each per configuration)

## Summary

| Metric | With Skill | Without Skill | Delta |
|--------|------------|---------------|-------|
| Pass Rate | 98% ± 5% | 74% ± 7% | +0.24 |
| Time | 1172.7s ± 722.5s | 687.3s ± 235.3s | +485.4s |
| Tokens | 264049 ± 102227 | 179745 ± 25556 | +84304 |

## Notes

- Round 2 re-ran only the with-skill tests, one at a time; the without-skill answers are round 1's (the baseline condition didn't change).
- Pass rate is unchanged at 98% with the skill vs 74% without; the checks were already near the ceiling in round 1, so round 2's gains show up in the process numbers below rather than the pass rate.
- Less over-research: web searches and fetches per run dropped about 30-45% (Supabase ~131 to 87, AI coding ~143 to 96, Jordan ~94 to 51, debugging ~76 to 55). No run lost work: notes were written during research, and none was interrupted.
- Verification still pays: round-2 checks reworded 4-10 claims per report to match their sources, removed one contradicted claim (Jordan) and one unverifiable sentence (AI coding), and corrected two vendor facts (Supabase OIDC support, SQL Connect timeline).
- Every report now has a 'What this means for you' section with situation-specific gotchas (e.g. Dammam needs invoiced billing; Firestore's location is permanent; existing customer data needs consent).
- Cost: on the three evals with clean timing for both, the skill took 392s vs 423s (quick), 1,307s vs 766s (fact-check) and 1,818s vs 873s (comparison); about 1.7-2.1x longer and 1.6-1.8x the tokens on research questions. That is the price of the separate verification and red-team passes.
- Eval 2's round-2 run read the grading assertions from its run folder before researching (my setup error, fixed for evals 3-5); treat its 9/9 as unreliable. Round 1's clean run of the same question got the Stripe detail right independently.
- Still missing: the quick answer again omitted the Node 26 Yarn/Corepack breakage the baseline caught. Prose length is now within limits for Supabase, Jordan and AI coding (2,533 vs 2,500), but the fact-check report ran ~1,600 words against a 1,200 target.
- Evals 4-5 ran with small efficiency fixes added mid-round (red team before verification; verify by source). AI coding re-opened 28 sources for 54 claims, versus about one fetch per claim in round 1.