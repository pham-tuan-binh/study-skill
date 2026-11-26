# Paper page template

## The five questions

Every page answers the user's five reading questions, critically, in this order. Answered well, they explain both the paper and the field around it.

1. **Domain:** what domain is the paper solving? Is it a whole domain, or a sub-domain inside one? If so, name both.
2. **Measurement:** how does it measure its results, and what are they? Is this how papers in this domain usually measure results? The last part is crucial: a non-standard metric, a self-made benchmark or a protocol nobody else uses is how you spot a weak paper. Say so plainly when it happens.
3. **Method:** how does it achieve its results, and what is actually novel about the method?
4. **Gaps:** what can the paper not solve, and what are the vectors of improvement?
5. **Reimplementation:** what would be worth experimenting with or reimplementing from this paper, and what is required to do that (code, weights, data, hardware, compute)?

Pages are written as paragraphs in a storytelling manner, not as a Q&A. Pick the template that matches the kind of work: a method or algorithm, an eval or benchmark, or a dataset.

## Method or algorithm

The user's example of the voice:

> Paper A is solving full body tracking for humanoid robots.
>
> It measures success by calculating the delta between a given pose and the humanoid pose in real life. On this benchmark, it performs X better than the trailing policy.
>
> It does this by training a RL policy with a differentiable world model.
>
> While it performs well, there are still several gaps, like: 1) it measures the wrong metric 2) etc
>
> If we want to improve on this paper, we might want to look into: 1) replacing the physics training for the diff world model because X and Y
>
> Furthermore, we can use paper B's idea in this paper to improve.

```markdown
# <Name> (<Org>)

**Org:** … · **Date:** Mon YYYY · **Paper:** [arXiv:ID](https://arxiv.org/abs/ID) · **Code:** [GitHub](url) or not released · **Models:** …

<Name> is solving <sub-domain> within <domain>. <Why it is hard or why it matters>.

![Overview figure caption as a complete sentence.](images/01-overview.png)

It measures success by <metric and benchmark>. On <benchmark>, it reaches <result>, compared with <baseline numbers>. <Whether this is how the domain usually measures results; if not, what the standard is and why the difference matters.>

<Short paragraphs with more results and protocol caveats.>

![Results figure caption.](images/02-results.png)

It does this by <core mechanism>. <What is novel, as opposed to borrowed from prior work.>

<Two to four short paragraphs on the method as prose.>

![Method or robot figure caption.](images/03-method.png)

While it performs well, there are still several gaps, like:

1\) …

2\) …

If we want to improve on this paper, we might want to look into:

1\) … because …

2\) … because …

Furthermore, we can use <Other paper>'s <idea> to <improvement>.

To reimplement <the part worth experimenting with>, one would need <code, weights, data, hardware, compute>, <which of these are released>.

# Sources

- [arXiv:ID](https://arxiv.org/abs/ID) · [Project page](url) · [Code](url)
```

## Eval or benchmark

The user's example of the voice:

> X is a real eval for robots, covering X types of robots (links to them)
>
> It measures the following metric: 1) 2) 3)
>
> It is special because 1) 2) 3)
>
> But there are still several gaps: 1) it can't test 2) it can be easily hacked
>
> To use X, one need to do expose a standard interface for Y Z. If they already have the robot that X support, they can just F Y Z
>
> So far X has been tested with M N O P

```markdown
# <Name> (<Org>)

**Org:** … · **Date:** Mon YYYY · **Paper:** [arXiv:ID](url) · **Code:** [GitHub](url) · **Leaderboard:** [link](url) or none

<Name> is a <real, simulated or hybrid> eval for <domain>, covering <robots, tasks or environments, with links>.

![Overview figure caption.](images/01-overview.png)

It measures the following metrics: 1\) … 2\) … 3\) … <How this compares with the metrics other evals in the domain use.>

It is special because 1\) … 2\) … 3\) …

But there are still several gaps: 1\) it can't test … 2\) it can be gamed by …

To use <Name>, one needs to expose <interface: observation and action spaces, API, hardware>. If they already have <supported robot or setup>, they can just <steps>.

So far <Name> has been tested with <systems, with links and headline scores>.

# Sources
```

## Dataset

The user's example of the voice:

> A is a large scale dataset, containing X hours of Y data, for Z robot
>
> It is aiming to solve B by doing C
>
> The data mixture is 1) 2) 3)
>
> It has been validated on 1) 2) 3)
>
> And the tested baseline is 1) 2) 3)
>
> We can use A to blah blah blah

```markdown
# <Name> (<Org>)

**Org:** … · **Date:** Mon YYYY · **Paper:** [arXiv:ID](url) · **Data:** [link](url) · **License:** …

<Name> is a <large scale> dataset, containing <X hours or episodes> of <data type and modalities>, for <robots or embodiments>.

![Overview figure caption.](images/01-overview.png)

It is aiming to solve <problem in the domain> by <collection approach>. <How its size and diversity compare with the datasets the domain usually uses.>

The data mixture is 1\) … 2\) … 3\) …

It has been validated on 1\) … 2\) … 3\) … <tasks, benchmarks or real robots, with results and protocol>.

And the tested baselines are 1\) … 2\) … 3\) …

We can use <Name> to <what it enables for us, and what is required: format, loader, storage, embodiment match>.

# Sources
```

## Rules

- About 600 to 900 words, with 4 to 5 figures, placed between the paragraphs they illustrate.
- The opening paragraphs before the first figure become the hub entry, so make them stand alone.
- The "Furthermore" paragraph must reference another paper that is in the same study.
- If the paper's measurement is not how the domain usually measures results, say so in the measurement paragraph, not only in the gaps.
