Statistics for Paper
================
Mikey Saugstad
March 19, 2018

Public Deployment
-----------------

NOTE: The public deployment dataset being used right now is not all that recent, so do not draw conclusions from what is below right now.

### Top-line numbers (no filtering)

Note that this is the only section for the public deployment where we are not filtering out users below the labeling frequency threshold (I am also filtering out researcher data below for now).

TODO: anything else?

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 153094   | 20664      | 45166      | 21639    | 1294      | 1417  | 8521           | 251795 |

### Data characteristics

This is the start of filtering out users with low labeling frequency (also filtering out researchers).

TODO: anything else?

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 89293    | 9256       | 32560      | 18174    | 685       | 1074  | 4722           | 155764 |

There have been a total of 19768 audits by our users across 13045 streets, averaging 1.52 audits per street.

### User stats and tool usage

TODO: missions started vs missions completed (not sure we can do this; I expect that it would be difficult, without much benefit)

Below are the medians for a few metrics (followed by sums), split by user group. For all user groups, the minimum threshold to be included in this list was that they have completed at least one audit task and that their labeling threshold is above 3.75 labels per 100 meters.

| role       | n\_users | miles | missions | audits | minutes\_audited | minutes\_per\_1k\_ft | labels | labels\_per\_100m | sessions | mins\_per\_sess |
|:-----------|:---------|:------|:---------|:-------|:-----------------|:---------------------|:-------|:------------------|:---------|:----------------|
| Anonymous  | 371      | 0.081 | 0        | 1.0    | 8.80             | 18.908               | 21     | 14.868            | 2        | 4.590           |
| Turker     | 130      | 0.348 | 4        | 4.5    | 24.09            | 12.803               | 57     | 10.476            | 1        | 22.685          |
| Registered | 190      | 0.544 | 4        | 8.0    | 28.65            | 7.661                | 73     | 7.278             | 1        | 19.818          |

| role       | n\_users | miles    | coverage | missions | audits | hours\_audited | labels | &gt;1 sess |
|:-----------|:---------|:---------|:---------|:---------|:-------|:---------------|:-------|:-----------|
| Anonymous  | 371      | 88.834   | 8.3%     | 370      | 1333   | 85.369         | 14159  | 73%        |
| Turker     | 130      | 1018.386 | 95%      | 3097     | 13234  | 445.934        | 104907 | 22%        |
| Registered | 190      | 394.097  | 37%      | 1226     | 5201   | 158.970        | 36939  | 37%        |

### Possible Story 1: Data overlap and agreement between users

Amongst all the data collected in DC, how much of DC is labeled by multiple users and what is the disagreement amongst them? (see comment in Outline document for details on implementation)

Turk Study
----------

This is most of the data... I think there are just a couple "conditions" (i.e., "sets of routes") that were missing some amount of data in my local dump, so I need to investigate. I also think I may have failed to remove the conditions for anonymous users who didn't place any labels. So most of these numbers are trustworthy (though they aren't the *final* numbers), I would just be wary about drawing conclusions from the anonymous user data.

### High level results

TODO percentage of turkers who completed the HIT (maybe?) <br> TODO anything else?

A total of 320 turkers, 49 registered users, and 15 anonymous users were part of this study.

Next we have average (median) stats, followed by aggregate (sum) stats.

| worker.type | labels.per.100m | feet.per.min | minutes.per.1k.ft | minutes\_audited |
|:------------|:----------------|:-------------|:------------------|:-----------------|
| anon        | 5.413           | 150.830      | 6.630             | 13.260           |
| reg         | 5.988           | 163.800      | 6.105             | 24.420           |
| turk        | 7.423           | 628.456      | 1.591             | 6.365            |

| worker.type | n.missions | distance.miles | n.labels | hours.audited |
|:------------|:-----------|:---------------|:---------|:--------------|
| anon        | 30         | 5.682          | 481      | 3.547         |
| reg         | 147        | 37.121         | 3626     | 21.518        |
| turk        | 177        | 42.803         | 5559     | 9.037         |
