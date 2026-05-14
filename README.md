# engram-benchmarks

Published benchmark artifacts for [Engram](https://lumetra.io), Lumetra's durable memory system for AI agents.

## LongMemEval-S, run 2026-04-20 → 2026-05-04

**458/500 = 91.6%** on the full [LongMemEval-S](https://arxiv.org/abs/2410.10813) benchmark, end to end, with the v44 composer prompt and a canonical user-profile pass.

| System | Score |
|---|---|
| Engram v44 (this run) | **458/500 = 91.60%** |
| Engram v44 on 120 stratified | 113/120 = 94.17% |

Full writeup: [Engram on LongMemEval](https://lumetra.io/engram-on-longmemeval) · [Why we open-sourced the composer prompt](https://lumetra.io/why-we-open-sourced-the-composer-prompt)

### Artifacts

All files live under [`longmemeval-s/20260420/`](./longmemeval-s/20260420):

| File | What it is |
|---|---|
| [`composer_prompt_v44.md`](./longmemeval-s/20260420/composer_prompt_v44.md) | The reference composer prompt. Drop-in against any retrieval system's output — has a `{profile}` slot, a `{question}` slot, and a `{memories}` slot. |
| [`engram_full500_summary.md`](./longmemeval-s/20260420/engram_full500_summary.md) | Final scorecard: overall, per-category breakdown, list of the 42 failing task IDs with predictions. |
| [`engram_full500_regrade_summary.json`](./longmemeval-s/20260420/engram_full500_regrade_summary.json) | 4-run regrade stability check on the same hypothesis set. Mean 451.25 / std 3.96 across regrades; 448 stably-pass, 50 stably-fail, 2 flippable. |

## Using the composer prompt

The prompt expects three template slots:

- `{profile}` — a structured per-user view. In Engram, this is the `explanation.profile` field of a `/v1/query` response. In your own stack, plug in whatever canonical-user-state object you produce.
- `{question}` — the user question being answered.
- `{memories}` — the retrieved memories, formatted however your retriever emits them.

Wire it against your retrieval system and run a benchmark. If you reproduce the number, or find a rule we missed, [open an issue](https://github.com/lumetra-io/engram-benchmarks/issues).

## License

MIT — see [`LICENSE`](./LICENSE). The composer prompt, the run summaries, and the metrics are all covered. Tune, fork, redistribute. If you fork the prompt and modify it for your domain, please rename your fork (e.g. `v44-derivative-for-X`) so "v44 composer prompt" keeps referring to the published artifact.

## Reproducing

The benchmark itself is from the LongMemEval team — clone [`xiaowu0162/LongMemEval`](https://github.com/xiaowu0162/LongMemEval) for the dataset and grader.

The retrieval system that produced these answers is Engram's hosted service. The composer prompt and profile-pass design are the public-facing contributions; the server-side retrieval and profile-generation pipeline are the closed-source layer. See [the writeup](https://lumetra.io/why-we-open-sourced-the-composer-prompt) for the reasoning behind that split.
