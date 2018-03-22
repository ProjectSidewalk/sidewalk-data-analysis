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
| 95976    | 13678      | 23772      | 16255    | 813       | 546   | 5960           | 157000 |

### Data characteristics

This is the start of filtering out users with low labeling frequency (also filtering out researchers). TODO: number of audits<br> TODO: anything else?

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:------|
| 52841    | 5054       | 16141      | 13788    | 399       | 330   | 3086           | 91639 |

There have been a total of 13455 audits by our users across 9822 streets, averaging 1.37 audits per street.

### User stats and tool usage

TODO: coverage for each user group<br> TODO: how long users stay on tool (use same method as computing total time spent, just with bigger time difference)<br> TODO: missions started vs missions completed (not sure we can do this; I expect that it would be difficult, without much benefit)

Below are the medians for a few metrics (followed by sums), split by user group. For all user groups, the minimum threshold to be included in this list was that they have completed at least one audit task and that their labeling threshold is above 3.75 labels per 100 meters.

| role       | n\_users | miles | missions | audits | minutes\_audited | minutes\_per\_1k\_ft | labels | labels\_per\_100m |
|:-----------|:---------|:------|:---------|:-------|:-----------------|:---------------------|:-------|:------------------|
| Anonymous  | 224      | 0.088 | 0        | 2      | 9.83             | 15.519               | 10.5   | 6.838             |
| Turker     | 33       | 0.537 | 5        | 8      | 34.33            | 10.917               | 86.0   | 7.682             |
| Registered | 149      | 0.737 | 4        | 9      | 31.17            | 6.821                | 69.0   | 5.909             |

| role       | n\_users | miles   | missions | audits | hours\_audited | labels |
|:-----------|:---------|:--------|:---------|:-------|:---------------|:-------|
| Anonymous  | 224      | 72.250  | 278      | 1046   | 58.236         | 6919   |
| Turker     | 33       | 600.387 | 1669     | 7635   | 247.206        | 53543  |
| Registered | 149      | 362.728 | 1091     | 4774   | 139.700        | 31177  |

### Possible Story 1: Data overlap and agreement between users

Amongst all the data collected in DC, how much of DC is labeled by multiple users and what is the disagreement amongst them? (see comment in Outline document for details on implementation)

Turk Study
----------
