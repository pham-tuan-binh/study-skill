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

The user's example of the arc, in their own drafty voice. It shows the order and the kind of content for each paragraph; the finished page is written in polished prose per `writing-style.md`.

> Paper A is solving whole body tracking for humanoid robots, which is a sub domain of humanoid control
>
> It measures success by the delta between the reference pose and the robot pose in real life (mpjpe). This is pretty standard for tracking papers so numbers are kinda comparable. On this benchmark it does like 30% better than the trailing policy
>
> It does this by training a RL policy with a differentiable world model instead of a normal physics sim. The novel part is backproping through the world model so it learns faster, the rest is mostly from prior work
>
> While it performs well, there are still several gaps, like: 1) mpjpe doesnt tell u if the motion is stable or just looks close 2) only tested on one robot etc
>
> If we want to improve on this paper, we might want to look into: 1) replacing the diff world model with physics training for the hard contact parts because the world model drifts there 2) testing on more robots etc
>
> Furthermore, we can use paper B's motion retargeting in this paper to get more diverse reference motions
>
> To reimplement this, we need their code (released) and a humanoid like g1, plus a few gpus for training. weights arent out so have to train from scratch

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

# Retrospective

*As of <Mon YYYY>.* <Verdict first: superseded, still the reference for something specific, or current.>

<What came after: entries or one-liners in the study that beat or extend it, with comparison numbers and protocol caveats.>

<What is still worth reading or reimplementing it for.>

# Sources

- [arXiv:ID](https://arxiv.org/abs/ID) · [Project page](url) · [Code](url)
```

## Eval or benchmark

The user's example of the arc, in their own drafty voice. It shows the order and the kind of content for each paragraph; the finished page is written in polished prose per `writing-style.md`.

> X is a real eval for robots, covering like 4 types of robots (franka, ur5, aloha, so100) across a few labs
>
> It measures the following metric: 1) success rate per task 2) progress score for partial completion etc
>
> It is special because 1) its real robot not sim so no sim2real gap 2) anyone can submit a policy remotely and it runs on their hardware etc
>
> But there are still several gaps: 1) it cant test long horizon stuff, most tasks are under a minute 2) it can be easily hacked if u overfit to the fixed scenes etc
>
> To use X, one need to expose a standard interface for obs and actions (like a policy server that takes images + proprio and returns joint actions). If they already have a robot that X support, they can just plug in the server and run the eval script
>
> So far X has been tested with pi0, openvla, octo etc

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

# Retrospective

*As of <Mon YYYY>.* <Verdict first: superseded, still the reference for something specific, or current.>

<What came after: entries or one-liners in the study that beat or extend it, with comparison numbers and protocol caveats.>

<What is still worth reading or reimplementing it for.>

# Sources
```

## Dataset

The user's example of the arc, in their own drafty voice. It shows the order and the kind of content for each paragraph; the finished page is written in polished prose per `writing-style.md`.

> A is a large scale dataset, containing X hours of teleop data, for Z robots (mostly franka, some aloha and mobile ones)
>
> It is aiming to solve the lack of diverse real world data for generalist policies by doing crowdsourced teleop across a bunch of labs with the same setup. It's like K times bigger than D which is what most papers train on rn
>
> The data mixture is 1) tabletop single arm stuff, most of it 2) bimanual 3) some mobile manipulation in homes. all have rgb, proprio and language
>
> It has been validated on 1) E sim benchmark, goes from S1% to S2% 2) real robot on F unseen tasks 3) zero shot on G which isnt in the data
>
> And the tested baseline is 1) H trained on D 2) same model but without the mobile data 3) I finetuned on A
>
> But there are still gaps: 1) its mostly one robot so the cross embodiment claim is kinda weak 2) success labels are self labelled so hard to filter bad demos
>
> We can use A to pretrain before finetuning on our arm. need to convert from rlds, like T TB of storage, and map the action space to ours

```markdown
# <Name> (<Org>)

**Org:** … · **Date:** Mon YYYY · **Paper:** [arXiv:ID](url) · **Data:** [link](url) · **License:** …

<Name> is a <large scale> dataset, containing <X hours or episodes> of <data type and modalities>, for <robots or embodiments>.

![Overview figure caption.](images/01-overview.png)

It is aiming to solve <problem in the domain> by <collection approach>. <How its size and diversity compare with the datasets the domain usually uses.>

The data mixture is 1\) … 2\) … 3\) …

It has been validated on 1\) … 2\) … 3\) … <tasks, benchmarks or real robots, with results and protocol>.

And the tested baselines are 1\) … 2\) … 3\) …

But there are still several gaps: 1\) … 2\) …

We can use <Name> to <what it enables for us, and what is required: format, loader, storage, embodiment match>.

# Retrospective

*As of <Mon YYYY>.* <Verdict first: superseded, still the reference for something specific, or current.>

<What came after: entries or one-liners in the study that beat or extend it, with comparison numbers and protocol caveats.>

<What is still worth reading or reimplementing it for.>

# Sources
```

## Retrospective

Every page ends with a `# Retrospective` section right before `# Sources`, written after the whole study exists. An example of the arc in the user's drafty voice, built from their own comments on MAESTRO:

> As of nov 2026. This is kinda outdated now, it was nov 2025 so expected. Still worth reading for the harness ablation tho, it shows the harness matters more than the tools
>
> Since then AMP1 basically does the same thing but lets the model write its own skills, 104/108 real trials, and JustRUA shows a plain harness with good scene geometry goes from 29.7 to 50.3 on RoboDojo with no learned policy
>
> The evals are thin (5 trials per setting) and π₀ zero shot isnt a fair baseline, so dont read too much into the numbers

## Rules

- About 600 to 900 words, with 4 to 5 figures, placed between the paragraphs they illustrate.
- The opening paragraphs before the first figure become the hub entry, so make them stand alone. Never put status or successor notes there; they belong in the Retrospective.
- The Retrospective has 2 to 4 short paragraphs, opening with *As of Mon YYYY.* so readers know when the verdict was made, then verdict first, then successors with numbers, then what is still worth reading for. No callouts.
- The "Furthermore" paragraph must reference another paper that is in the same study.
- If the paper's measurement is not how the domain usually measures results, say so in the measurement paragraph, not only in the gaps.
