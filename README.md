# fitts1954-reproduction

> Replicating the figure that launched a thousand touchscreens. A hands-on reproduction of Fitts (1954) in Python.
[
![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)
](https://colab.research.google.com/drive/18AUciiLZpwrD0ZU9x50PGlHv8vY8xy1Y?usp=sharing)

---

## What is this?

**1954.**

Psychologist Paul Fitts made a bet : that human movement could be described mathematically. Not approximately, not qualitatively but **precisely**, using the same information theory Shannon had just applied to communication channels.

He was right ! His model which is now called **Fitts' Law** predicts how long it takes to move toward a target from just two measurements: distance and width. It has since survived decades of replication across mice, touchscreens, robotic arms, eye trackers, and even brain-computer interfaces.

This project picks up that 1954 paper, extracts its data, and **rebuilds the core figure from scratch** — regression line, throughput, the works. Not to prove Fitts right (he is), but to practice what good science actually looks like: transparent methods, honest comparison, and documented reasoning.

---

## I. The figure

The target is **Figure 2** from the original paper: a scatter plot of Movement Time (MT) against the Index of Difficulty (ID), with a linear regression fit.

| Original (Fitts, 1954) | This replication |
|---|---|
| *Not reproduced here for copyright reasons* | ![Reproduced figure](figures/scatter-plot) |
| *See Fitts (1954), Figure 2. DOI: [10.1037/h0055392](https://psycnet.apa.org/doiLanding?doi=10.1037%2Fh0055392)* | |

### Results at a glance

| Metric | Fitts (1954) | This replication |
|--------|-------------|------------------|
| R² (MT ~ ID) | ≈ 0.966 | **0.977** |
| Mean Throughput | 10.10 bits/s | **13.05 bits/s** |
| Slope b | ≈ 94 ms/bit | **77 ms/bit** |

The linear relationship holds cleanly (R² = 0.977, p < .001). The throughput gap is real and expected — explained in full in [Section III](#iii-known-discrepancies).

---

## II. How the model works

Fitts framed motor control as an information-transmission problem. A movement toward a target carries a certain amount of information — harder targets (farther, or smaller) carry more.

```
ID  = log2(2D / W)      — Index of Difficulty, in bits
                           D: distance to target
                           W: target width

MT  = a + b x ID        — Movement Time is linear in ID

TP  = ID / MT           — Throughput: how fast the motor system
                           processes that information (bits/second)
```

The punchline: **b and TP are essentially constants across people and devices** — at least within a reasonable range. That is what made the law so durable.

### Data

12 experimental conditions from Table 1 of Fitts (1954), serial tapping task with a 1-oz stylus. Target distance D varies across 2, 4, 8, and 16 inches; target width W across 0.5, 1, and 2 inches. Movement times are reported as condition means.

---

## III. Known discrepancies

The replication is faithful but not identical. That gap deserves an explanation, not a footnote.

**Rounded data.** The paper publishes group means, not raw trials. Fitting a regression on summary statistics rather than individual observations slightly distorts slope and variance estimates — there is no fix for this without the original dataset.

**ID formula variant.** Fitts used `log2(2D/W)`. MacKenzie (1992) later proposed a Shannon-corrected version `log2(D/W + 1)`, which systematically produces lower ID values and higher throughput estimates. We use the original formulation here for historical fidelity — but it is worth knowing both exist.

**No effective width correction.** ISO 9241-9 defines throughput using endpoint variability: `We = 4.133 x SD` of the landing distribution, rather than the nominal target width. Without raw trial data, this correction cannot be applied — which inflates our throughput estimate relative to a modern replication.

None of this is a problem. These discrepancies are *the point* — they show exactly why data sharing and methodological transparency matter.

---

## IV. Repository structure

```
fitts1954-reproduction/
├── README.md
├── figures/
│   └── fitts1954_replication.png
└── fitts1954_replication.ipynb
```

The notebook runs entirely in **Google Colab** — no setup, no environment, just open and run.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

Dependencies if running locally: `numpy`, `pandas`, `matplotlib`, `scipy`.

---

## V. Why working on Fitts now ?

This project sits at the intersection of two things : an interest in scientific reproducibility, and a practical need to deeply understand the metrics behind **[NeuralTrack](https://github.com/)** — a web-based cognitive experiment that uses Fitts' Law as its measurement backbone.

NeuralTrack, a repo I made, collects Movement Time, computes Throughput, and tracks motor precision (Straightness Index, jitter) to detect cognitive load signatures. Before implementing those metrics, it made sense to deeply go back to the source and not just cite Fitts, but actually run his numbers.

That turned out to be a better learning experience than expected.

---

## References

- Fitts, P. M. (1954). The information capacity of the human motor system in controlling the amplitude of movement. *Journal of Experimental Psychology, 47*(6), 381–391 - https://psycnet.apa.org/doiLanding?doi=10.1037%2Fh0055392
- MacKenzie, I. S. (1992). Fitts' law as a research and design tool in human-computer interaction. *Human-Computer Interaction, 7*(1), 91–139 - https://www.tandfonline.com/doi/abs/10.1207/s15327051hci0701_3
- Soukoreff, R. W., & MacKenzie, I. S. (2004). Towards a standard for pointing device evaluation. *International Journal of Human-Computer Studies, 61*(6), 751–789 - https://www.sciencedirect.com/science/article/abs/pii/S1071581904001016?via%3Dihub
- ReScience C - http://rescience.github.io

---

*Cognitive Neuroscience — Scientific reproducibility portfolio.*
