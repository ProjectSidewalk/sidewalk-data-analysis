Statistics for Paper
================
Mikey Saugstad
March 19, 2018

Public Deployment
-----------------

NOTE: The public deployment dataset being used right now is not all that recent, so do not draw conclusions from what is below right now.

### How much data was collected?

Labels, labels by label type, anything else? (this is the one section before filtering low quality work)

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 95907    | 13676      | 23772      | 16252    | 813       | 546   | 5960           | 157000 |

### Data characteristics

TODO: Number of labels (only high quality users)<br> TODO: labels by label type<br> TODO: number of audits<br> TODO: anything else?

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
