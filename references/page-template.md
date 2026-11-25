# Paper page template

The user's own "how I ingest a paper" order. Here is their example of the voice:

> Paper A is solving full body tracking for humanoid robots. It measures success by calculating the delta between a given pose and the humanoid pose in real life. On this benchmark, it performs X better than the trailing policy. It does this by training a RL policy with a differentiable world model. While it performs well, there are still several gaps, like: 1) it measures the wrong metric 2) … If we want to improve on this paper, we might want to look into: 1) replacing the physics training for the diff world model because X and Y. Furthermore, we can use paper B's idea in this paper to improve.

## Skeleton

```markdown
# <Name> (<Org>)

**Org:** … · **Date:** Mon YYYY · **Paper:** [arXiv:ID](https://arxiv.org/abs/ID) · **Code:** [GitHub](url) or not released · **Models:** …

<Name> tackles <problem>. <Why it is hard or why it matters>.

<One or two more short paragraphs on the problem, if needed.>

![Overview figure caption as a complete sentence.](images/01-overview.png)

It measures success by <metric and benchmark>. On <benchmark>, it reaches <result>, compared with <baseline numbers>.

<Short paragraphs with more results and protocol caveats.>

![Results figure caption.](images/02-results.png)

It does this by <core mechanism>.

<Two to four short paragraphs covering the loop, perception, actions, skills and memory as prose.>

![Method or robot figure caption.](images/03-method.png)

While it performs well, there are still several gaps, like:

1\) cost or latency …

2\) evaluation size or protocol …

3\) a limitation the authors state …

If we want to improve on this paper, we might want to look into:

1\) … because …

2\) … because …

Furthermore, we can use <Other paper>'s <idea> to <improvement>.

# Sources

- [arXiv:ID](https://arxiv.org/abs/ID) · [Project page](url) · [Code](url)
```

## Rules

- About 600 to 900 words, with 4 to 5 figures.
- The opening paragraphs before the first figure become the hub entry, so make them stand alone.
- The "Furthermore" paragraph must reference another paper that is in the same study.
