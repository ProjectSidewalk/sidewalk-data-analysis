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
| 133698   | 17799      | 41095      | 20290    | 1193      | 1317  | 7665           | 223057 |

### Data characteristics

This is the start of filtering out users with low labeling frequency (also filtering out researchers).

TODO: anything else?

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 78363    | 7739       | 30308      | 17048    | 640       | 1008  | 4203           | 139309 |

There have been a total of 19081 audits by our users across 12799 streets, averaging 1.49 audits per street.

### User stats and tool usage

TODO: missions started vs missions completed (not sure we can do this; I expect that it would be difficult, without much benefit)

Below are the medians for a few metrics (followed by sums), split by user group. For all user groups, the minimum threshold to be included in this list was that they have completed at least one audit task and that their labeling threshold is above 3.75 labels per 100 meters.

| role       | n\_users | miles | missions | audits | minutes\_audited | minutes\_per\_1k\_ft | labels | labels\_per\_100m | sessions | mins\_per\_sess |
|:-----------|:---------|:------|:---------|:-------|:-----------------|:---------------------|:-------|:------------------|:---------|:----------------|
| Anonymous  | 247      | 0.087 | 0        | 2      | 9.880            | 16.144               | 10     | 6.815             | 2        | 5.470           |
| Turker     | 114      | 0.376 | 4        | 5      | 29.235           | 12.322               | 56     | 7.642             | 1        | 27.020          |
| Registered | 152      | 0.733 | 4        | 9      | 30.520           | 7.225                | 66     | 6.051             | 1        | 20.323          |

| role       | n\_users | miles    | coverage | missions | audits | hours\_audited | labels | &gt;1 sess |
|:-----------|:---------|:---------|:---------|:---------|:-------|:---------------|:-------|:-----------|
| Anonymous  | 247      | 76.586   | 7.1%     | 297      | 1109   | 65.259         | 7361   | 71%        |
| Turker     | 114      | 1015.264 | 94%      | 3060     | 13188  | 442.177        | 100732 | 25%        |
| Registered | 152      | 363.140  | 34%      | 1097     | 4784   | 140.594        | 31216  | 39%        |

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
