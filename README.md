# A/B Test Analysis: Does Moving a Game Gate Change Player Retention?

Statistical analysis of a randomised experiment on the mobile game *Cookie Cats*: the first gate in the game was moved
from level 30 to level 40, and new players were randomly assigned to one version. The project tests whether the change
affects retention and ends with a ship / don't-ship recommendation.

The same method (randomised groups, hypothesis tests on rates, confidence intervals, power) applies to landing pages,
email subject lines and pricing changes.

## Data
- Cookie Cats A/B test dataset: 90,189 players, 5 columns
  (`userid`, `version`, `sum_gamerounds`, `retention_1`, `retention_7`)
- `gate_30` = control (44,700 players), `gate_40` = treatment (45,489 players)
- Copy used: https://github.com/yanny-alt/Cookie-Cats-A-B-Testing-Project

## Approach
1. Defined hypotheses, primary metrics (1-day and 7-day retention), secondary metric (game rounds) and the decision rule
2. Checked experiment validity with a sample ratio (chi-square) test
3. Compared retention with a two-proportion z-test and 95% confidence intervals, with a Bonferroni check for two metrics
4. Confirmed the results with a 10,000-replicate bootstrap
5. Computed the minimum detectable effect to judge whether non-significant results are informative
6. Compared game rounds with a Mann-Whitney U test (heavily skewed data)

## Results
| Metric | gate_30 | gate_40 | Difference (95% CI) | p-value |
|---|---|---|---|---|
| Day-1 retention | 44.82% | 44.23% | -0.59 pp (-1.24 to +0.06) | 0.074 |
| Day-7 retention | 19.02% | 18.20% | -0.82 pp (-1.33 to -0.31) | 0.0016 |

- Day-7 retention is significantly lower with the gate at level 40 (a 4.3% relative drop). The result holds after the
  Bonferroni correction (threshold 0.025) and in the bootstrap.
- Day-1 retention and game rounds (Mann-Whitney p = 0.050) show no reliable difference. The day-1 effect is smaller than the
  minimum detectable effect (0.93 pp), so it is inconclusive, not proof of no effect.
- Sample ratio check: 49.56% vs 50.44% (p = 0.0086), a small imbalance noted as a caution.

![Retention by gate position](images/02_retention_rates.png)
![Bootstrap differences](images/03_bootstrap_differences.png)

## Recommendation
Keep the gate at level 30. Moving it to level 40 lowers 7-day retention with no offsetting benefit in the measured metrics.

## Limitations
- Small, unexplained sample ratio imbalance between groups
- Only short-term retention and rounds played were measured; revenue and long-term retention are unknown
- Single run of the experiment, using a public copy of the data

## How to run
1. Open `ab_test_cookie_cats.ipynb` in Google Colab or Jupyter.
2. Run all cells. The data loads from a public URL.
3. Charts are saved to the `images/` folder.

## Next steps
- Measure revenue per player and day-14 / day-30 retention
- Check consistency across player segments
- Investigate the cause of the sample ratio imbalance

## Author
Tamanna Akter Swarna
