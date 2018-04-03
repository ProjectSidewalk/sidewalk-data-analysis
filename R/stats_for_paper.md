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

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:------|
| 39786    | 4118       | 12142      | 3783     | 555       | 178   | 3582           | 64144 |

### Data characteristics

This is the start of filtering out users with low labeling frequency (also filtering out researchers).

TODO: anything else?

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:------|
| 20097    | 2373       | 4595       | 2186     | 225       | 105   | 1328           | 30909 |

There have been a total of 4802 audits by our users across 4111 streets, averaging 1.17 audits per street.

### User stats and tool usage

TODO: missions started vs missions completed (not sure we can do this; I expect that it would be difficult, without much benefit)

Below are the medians for a few metrics (followed by sums), split by user group. For all user groups, the minimum threshold to be included in this list was that they have completed at least one audit task and that their labeling threshold is above 3.75 labels per 100 meters.

| role       | n\_users | miles | missions | audits | minutes\_audited | minutes\_per\_1k\_ft | labels | labels\_per\_100m | sessions | mins\_per\_sess |
|:-----------|:---------|:------|:---------|:-------|:-----------------|:---------------------|:-------|:------------------|:---------|:----------------|
| Anonymous  | 132      | 0.084 | 0        | 2      | 10.685           | 16.507               | 10     | 6.348             | 2        | 6.969           |
| Registered | 123      | 0.801 | 4        | 12     | 37.470           | 6.719                | 78     | 5.792             | 1        | 23.013          |

| role       | n\_users | miles   | coverage | missions | audits | hours\_audited | labels | &gt;1 sess |
|:-----------|:---------|:--------|:---------|:---------|:-------|:---------------|:-------|:-----------|
| Anonymous  | 132      | 41.781  | 3.9%     | 168      | 611    | 35.250         | 3751   | 63%        |
| Registered | 123      | 316.462 | 29%      | 878      | 4191   | 116.209        | 27158  | 39%        |

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
