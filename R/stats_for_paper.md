Statistics for Paper
================
Mikey Saugstad
April 17, 2018

-   [Public Deployment](#public-deployment)
    -   [High-level results](#high-level-results)
        -   [Top-line numbers (no filtering)](#top-line-numbers-no-filtering)
        -   [Top-line numbers (with filtering)](#top-line-numbers-with-filtering)
        -   [Attribute counts by type](#attribute-counts-by-type)
        -   [Dataset 1st 100% vs full deployment](#dataset-1st-100-vs-full-deployment)
        -   [Data characteristics](#data-characteristics)
        -   [Data lost due to filtering](#data-lost-due-to-filtering)
        -   [User stats and tool usage](#user-stats-and-tool-usage)
    -   [Possible Stories](#possible-stories)
        -   [Data overlap and agreement between users](#data-overlap-and-agreement-between-users)
        -   [User dropoffs](#user-dropoffs)
-   [Turk Study](#turk-study)
    -   [High level results](#high-level-results-1)
        -   [Ground truth label counts](#ground-truth-label-counts)
        -   [Aggregate accuracy](#aggregate-accuracy)
        -   [Raw accuracy](#raw-accuracy)
        -   [Voting: Improved recall when at least one turker marks](#voting-improved-recall-when-at-least-one-turker-marks)
        -   [Descriptive stats for users](#descriptive-stats-for-users)
        -   [Giant table with accuracy](#giant-table-with-accuracy)
        -   [IRR](#irr)
    -   [Possible Stories](#possible-stories-1)
        -   [Granularity: Street-level vs 5 meter-level](#granularity-street-level-vs-5-meter-level)
        -   [Accuracy by user group](#accuracy-by-user-group)
        -   [Accuracy by label type](#accuracy-by-label-type)
        -   [Visual search time: Time to label by type](#visual-search-time-time-to-label-by-type)
        -   [Zone type: Land use effect on accuracy](#zone-type-land-use-effect-on-accuracy)
        -   [User behavior: Does auditing speed, etc influence accuracy](#user-behavior-does-auditing-speed-etc-influence-accuracy)
        -   [User group: Reg vs anon vs turk1 vs turk3 vs turk5](#user-group-reg-vs-anon-vs-turk1-vs-turk3-vs-turk5)
        -   [Low severity: Removing low severity effect on recall](#low-severity-removing-low-severity-effect-on-recall)
        -   [Binary vs ordinal issues per segment](#binary-vs-ordinal-issues-per-segment)
    -   [Rationale and description of mixed effect models](#rationale-and-description-of-mixed-effect-models)
        -   [Why we are using them](#why-we-are-using-them)
        -   [Example text](#example-text)
        -   [Assumptions](#assumptions)

Public Deployment
=================

NOTE: Public deployment data includes all data up through March 31st (and part of April 1st). This includes all data through the most recent deployment on mturk. We only consider someone a "user" if they have completed an audit of at least one street and have placed at least one label after the tutorial. Later on, we also filter out users with low labeling frequency.

High-level results
------------------

### Top-line numbers (no filtering)

The following are the label counts (not attribute counts) by user group and label type. There are a total of 251438 labels, 11347 are tutorial labels and 240091 are non tutorial labels. We consider only non tutorial labels throughout this document.

| label\_type    | Anon (384)   | Registered (243) | Turker (170)   | Researcher (28) | Total           |
|:---------------|:-------------|:-----------------|:---------------|:----------------|:----------------|
| CurbRamp       | 10631 (4.4%) | 27144 (11.3%)    | 88554 (36.9%)  | 18336 (7.6%)    | 144665 (60.3%)  |
| NoCurbRamp     | 1310 (0.5%)  | 3250 (1.4%)      | 13262 (5.5%)   | 1138 (0.5%)     | 18960 (7.9%)    |
| Obstacle       | 1105 (0.5%)  | 2827 (1.2%)      | 16154 (6.7%)   | 1498 (0.6%)     | 21584 (9.0%)    |
| SurfaceProblem | 765 (0.3%)   | 1896 (0.8%)      | 3216 (1.3%)    | 2591 (1.1%)     | 8468 (3.5%)     |
| NoSidewalk     | 1414 (0.6%)  | 6211 (2.6%)      | 28181 (11.7%)  | 7919 (3.3%)     | 43725 (18.2%)   |
| Occlusion      | 68 (0.0%)    | 310 (0.1%)       | 462 (0.2%)     | 438 (0.2%)      | 1278 (0.5%)     |
| Other          | 92 (0.0%)    | 148 (0.1%)       | 1137 (0.5%)    | 34 (0.0%)       | 1411 (0.6%)     |
| Total          | 15385 (6.4%) | 41786 (17.4%)    | 150966 (62.9%) | 31954 (13.3%)   | 240091 (100.0%) |

### Top-line numbers (with filtering)

| label\_type    | Anon (293)   | Registered (188) | Turker (122)   | Researcher (21) | Total           |
|:---------------|:-------------|:-----------------|:---------------|:----------------|:----------------|
| CurbRamp       | 7376 (4.1%)  | 23102 (12.8%)    | 54961 (30.4%)  | 17297 (9.6%)    | 102736 (56.8%)  |
| NoCurbRamp     | 836 (0.5%)   | 2741 (1.5%)      | 4931 (2.7%)    | 1066 (0.6%)     | 9574 (5.3%)     |
| Obstacle       | 824 (0.5%)   | 2506 (1.4%)      | 14726 (8.1%)   | 1461 (0.8%)     | 19517 (10.8%)   |
| SurfaceProblem | 522 (0.3%)   | 1577 (0.9%)      | 2594 (1.4%)    | 2525 (1.4%)     | 7218 (4.0%)     |
| NoSidewalk     | 994 (0.5%)   | 5581 (3.1%)      | 25352 (14.0%)  | 7686 (4.2%)     | 39613 (21.9%)   |
| Occlusion      | 41 (0.0%)    | 288 (0.2%)       | 356 (0.2%)     | 424 (0.2%)      | 1109 (0.6%)     |
| Other          | 42 (0.0%)    | 128 (0.1%)       | 900 (0.5%)     | 29 (0.0%)       | 1099 (0.6%)     |
| Total          | 10635 (5.9%) | 35923 (19.9%)    | 103820 (57.4%) | 30488 (16.9%)   | 180866 (100.0%) |

### Attribute counts by type

Here are the counts of attributes by attribute type after single and multi user clustering.

NOTE: Clustering only uses the filtered data! Only those who meet the labeling frequency threshold.

| attribute.type | count  | percentage |
|:---------------|:-------|:-----------|
| CurbRamp       | 51098  | 49.6%      |
| NoCurbRamp     | 7941   | 7.7%       |
| Obstacle       | 12993  | 12.6%      |
| SurfaceProblem | 5647   | 5.5%       |
| NoSidewalk     | 23468  | 22.8%      |
| Occlusion      | 953    | 0.9%       |
| Other          | 928    | 0.9%       |
| Problem        | 23603  | -          |
| Total          | 103028 | 100.0%     |

### Dataset 1st 100% vs full deployment

The first table describes the dataset at the point where we hit 100% completion. The second table describes the full dataset. Both include *all* users, not just "good" users. We define a user to be someone who both completed an audit of at least one street segment *and* placed at least one label after the tutorial.

| role       | n.users | label\_count | miles\_audited |
|:-----------|:--------|:-------------|:---------------|
| Researcher | 25      | 29107        | 224            |
| Anonymous  | 349     | 12240        | 150            |
| Turker     | 43      | 78153        | 1208           |
| Registered | 234     | 41353        | 533            |

| role       | n.users | label\_count | miles\_audited |
|:-----------|:--------|:-------------|:---------------|
| Researcher | 28      | 31954        | 239            |
| Anonymous  | 384     | 12950        | 156            |
| Turker     | 170     | 150847       | 2249           |
| Registered | 243     | 41588        | 536            |

### Data characteristics

This is the start of filtering out users with low labeling frequency.

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 102736   | 9574       | 39613      | 19517    | 1109      | 1099  | 7218           | 180866 |

There have been a total of 22276 audits by our "good" users across 14037 streets, averaging 1.59 audits per street. Of the 6053 streets that have been audited multiple times, there are an average of 2.36 audits per street.

### Data lost due to filtering

There were 825 users who placed 240091 labels pre-filtering. Those with low labeling frequency accounted for 201 of the users (24.4%) and 56341 of the labels (23.5%). This means that we are left with 624 of the users (75.6%) and 180866 of the labels (75.3%).

### User stats and tool usage

TODO: Missions started vs missions completed (not sure we can do this; I expect it to be difficult, without much benefit).

Below are the means/medians/sds for a few metrics (followed by sums), split by user group.

In the first 2 tables, we have the non-filtered data: this includes all users that audited at least one street and placed at least one label after the tutorial. The latter 2 tables include the filtered dataset: users were only included if they audited at least one street and had a labeling frequency of at least 3.75 labels per 100 meters.

NOTE: A "session" below is defined as a sequence of audit task interactions for a user where the minimum time between consecutive interactions is less than one hour.

Non filtered dataset:

| role       | n.users | audit\_count\_md | audit\_count\_mn | audit\_count\_sd | km\_md | km\_mn | km\_sd | label\_count\_md | label\_count\_mn | label\_count\_sd | labels\_per\_100m\_md | labels\_per\_100m\_mn | labels\_per\_100m\_sd | miles\_audited\_md | miles\_audited\_mn | miles\_audited\_sd | minutes\_audited\_md | minutes\_audited\_mn | minutes\_audited\_sd | minutes\_per\_session\_md | minutes\_per\_session\_mn | minutes\_per\_session\_sd | mission\_count\_md | mission\_count\_mn | mission\_count\_sd | m\_per\_min\_md | m\_per\_min\_mn | m\_per\_min\_sd | n\_sessions\_md | n\_sessions\_mn | n\_sessions\_sd | problem\_label\_count\_md | problem\_label\_count\_mn | problem\_label\_count\_sd | problem\_labels\_per\_100m\_md | problem\_labels\_per\_100m\_mn | problem\_labels\_per\_100m\_sd |
|:-----------|:--------|:-----------------|:-----------------|:-----------------|:-------|:-------|:-------|:-----------------|:-----------------|:-----------------|:----------------------|:----------------------|:----------------------|:-------------------|:-------------------|:-------------------|:---------------------|:---------------------|:---------------------|:--------------------------|:--------------------------|:--------------------------|:-------------------|:-------------------|:-------------------|:----------------|:----------------|:----------------|:----------------|:----------------|:----------------|:--------------------------|:--------------------------|:--------------------------|:-------------------------------|:-------------------------------|:-------------------------------|
| Anonymous  | 384     | 2.0              | 5.581            | 15.166           | 0.154  | 0.652  | 2.023  | 15.0             | 33.724           | 82.977           | 8.012                 | 18.152                | 34.329                | 0.096              | 0.405              | 1.257              | 10.345               | 18.294               | 26.087               | 5.673                     | 9.535                     | 12.888                    | 1.0                | 1.500              | 3.067              | 17.166          | 32.801          | 67.070          | 2               | 2.297           | 2.014           | 2.0                       | 6.872                     | 19.339                    | 0.830                          | 3.082                          | 6.321                          |
| Registered | 243     | 9.0              | 28.868           | 100.621          | 1.195  | 3.547  | 13.211 | 65.0             | 171.144          | 651.312          | 5.841                 | 9.135                 | 18.311                | 0.742              | 2.204              | 8.209              | 29.350               | 55.830               | 126.900              | 20.025                    | 28.333                    | 27.290                    | 3.0                | 5.786              | 20.348             | 46.120          | 51.679          | 35.371          | 1               | 2.037           | 3.272           | 12.0                      | 32.605                    | 135.250                   | 0.911                          | 2.304                          | 5.441                          |
| Turker     | 170     | 9.0              | 169.835          | 477.006          | 1.146  | 21.290 | 60.188 | 59.0             | 887.335          | 2671.343         | 7.081                 | 10.597                | 16.161                | 0.712              | 13.229             | 37.399             | 35.120               | 266.196              | 712.553              | 32.768                    | 51.187                    | 58.217                    | 4.0                | 35.394             | 94.915             | 30.752          | 45.060          | 41.123          | 1               | 3.229           | 6.231           | 12.0                      | 191.853                   | 883.811                   | 0.889                          | 2.573                          | 7.056                          |
| Researcher | 28      | 16.5             | 108.893          | 245.085          | 2.321  | 13.711 | 31.206 | 122.5            | 1141.214         | 3212.718         | 6.005                 | 6.487                 | 3.054                 | 1.442              | 8.519              | 19.391             | 36.500               | 195.805              | 438.638              | 7.585                     | 14.216                    | 13.265                    | 4.5                | 24.643             | 53.024             | 53.529          | 74.048          | 52.979          | 2               | 16.143          | 31.226          | 13.5                      | 186.679                   | 522.481                   | 0.679                          | 0.988                          | 1.095                          |

| role       | n.users | audits | hours\_audited | km   | labels | miles | missions | coverage | &gt;1 sess |
|:-----------|:--------|:-------|:---------------|:-----|:-------|:------|:---------|:---------|:-----------|
| Anonymous  | 384     | 2143   | 117            | 250  | 12950  | 156   | 576      | 14%      | 75%        |
| Registered | 243     | 7015   | 226            | 862  | 41588  | 536   | 1406     | 50%      | 38%        |
| Turker     | 170     | 28872  | 754            | 3619 | 150847 | 2249  | 6017     | 209%     | 29%        |
| Researcher | 28      | 3049   | 91             | 384  | 31954  | 239   | 690      | 22%      | 68%        |

Filtered dataset:

| role       | n.users | audit\_count\_md | audit\_count\_mn | audit\_count\_sd | km\_md | km\_mn | km\_sd | label\_count\_md | label\_count\_mn | label\_count\_sd | labels\_per\_100m\_md | labels\_per\_100m\_mn | labels\_per\_100m\_sd | miles\_audited\_md | miles\_audited\_mn | miles\_audited\_sd | minutes\_audited\_md | minutes\_audited\_mn | minutes\_audited\_sd | minutes\_per\_session\_md | minutes\_per\_session\_mn | minutes\_per\_session\_sd | mission\_count\_md | mission\_count\_mn | mission\_count\_sd | m\_per\_min\_md | m\_per\_min\_mn | m\_per\_min\_sd | n\_sessions\_md | n\_sessions\_mn | n\_sessions\_sd | problem\_label\_count\_md | problem\_label\_count\_mn | problem\_label\_count\_sd | problem\_labels\_per\_100m\_md | problem\_labels\_per\_100m\_mn | problem\_labels\_per\_100m\_sd |
|:-----------|:--------|:-----------------|:-----------------|:-----------------|:-------|:-------|:-------|:-----------------|:-----------------|:-----------------|:----------------------|:----------------------|:----------------------|:-------------------|:-------------------|:-------------------|:---------------------|:---------------------|:---------------------|:--------------------------|:--------------------------|:--------------------------|:-------------------|:-------------------|:-------------------|:----------------|:----------------|:----------------|:----------------|:----------------|:----------------|:--------------------------|:--------------------------|:--------------------------|:-------------------------------|:-------------------------------|:-------------------------------|
| Anonymous  | 293     | 2                | 4.031            | 12.690           | 0.133  | 0.439  | 1.685  | 17.0             | 36.724           | 89.948           | 10.508                | 23.164                | 37.934                | 0.083              | 0.273              | 1.047              | 10.640               | 17.589               | 25.105               | 6.120                     | 9.289                     | 10.913                    | 0                  | 1.078              | 2.554              | 13.660          | 19.980          | 21.169          | 2               | 2.232           | 1.902           | 2.0                       | 7.498                     | 20.903                    | 1.430                          | 3.892                          | 7.036                          |
| Registered | 188     | 8                | 27.505           | 111.245          | 0.869  | 3.352  | 14.658 | 70.5             | 191.080          | 731.413          | 6.800                 | 11.257                | 20.336                | 0.540              | 2.083              | 9.108              | 29.335               | 57.882               | 139.760              | 20.367                    | 28.329                    | 27.346                    | 3                  | 5.553              | 22.634             | 37.314          | 42.404          | 29.573          | 1               | 2.101           | 3.638           | 12.0                      | 36.298                    | 152.493                   | 1.253                          | 2.854                          | 6.078                          |
| Turker     | 122     | 5                | 108.254          | 419.687          | 0.586  | 13.408 | 52.259 | 59.0             | 850.984          | 2961.015         | 8.900                 | 13.962                | 17.996                | 0.364              | 8.331              | 32.472             | 26.940               | 225.221              | 757.025              | 26.370                    | 37.720                    | 34.596                    | 3                  | 24.205             | 87.261             | 23.623          | 30.571          | 25.034          | 1               | 3.082           | 6.756           | 11.5                      | 182.385                   | 996.002                   | 1.340                          | 3.368                          | 8.194                          |
| Researcher | 21      | 11               | 129.381          | 280.256          | 1.511  | 16.226 | 35.716 | 113.0            | 1451.810         | 3676.075         | 7.080                 | 7.530                 | 2.825                 | 0.939              | 10.083             | 22.193             | 33.560               | 233.844              | 501.810              | 7.550                     | 11.107                    | 8.967                     | 3                  | 28.762             | 60.685             | 50.913          | 68.873          | 56.350          | 2               | 18.810          | 34.797          | 8.0                       | 240.571                   | 596.770                   | 0.745                          | 1.111                          | 1.198                          |

| role       | n.users | audits | hours\_audited | km   | labels | miles | missions | coverage | &gt;1 sess |
|:-----------|:--------|:-------|:---------------|:-----|:-------|:------|:---------|:---------|:-----------|
| Anonymous  | 293     | 1181   | 86             | 129  | 10760  | 80    | 316      | 7.4%     | 72%        |
| Registered | 188     | 5171   | 181            | 630  | 35923  | 392   | 1044     | 36.4%    | 38%        |
| Turker     | 122     | 13207  | 458            | 1636 | 103820 | 1016  | 2953     | 94.5%    | 23%        |
| Researcher | 21      | 2717   | 82             | 341  | 30488  | 212   | 604      | 19.7%    | 71%        |

Below are trimmed down tables; non-filtered followed by filtered.

| role       | n.users | audits | labels | miles   | missions | &gt;1 sess | labels\_per\_100m\_md | miles\_per\_hr\_md | minutes\_audited\_md | problem\_labels\_per\_100m\_md |
|:-----------|:--------|:-------|:-------|:--------|:---------|:-----------|:----------------------|:-------------------|:---------------------|:-------------------------------|
| Anonymous  | 384     | 2143   | 12950  | 155.54  | 576      | 75%        | 8.01                  | 0.64               | 10.34                | 0.83                           |
| Registered | 243     | 7015   | 41588  | 535.61  | 1406     | 38%        | 5.84                  | 1.72               | 29.35                | 0.91                           |
| Turker     | 170     | 28872  | 150847 | 2248.91 | 6017     | 29%        | 7.08                  | 1.15               | 35.12                | 0.89                           |
| Researcher | 28      | 3049   | 31954  | 238.54  | 690      | 68%        | 6.00                  | 2.00               | 36.50                | 0.68                           |

| role       | n.users | audits | labels | miles   | missions | &gt;1 sess | labels\_per\_100m\_md | miles\_per\_hr\_md | minutes\_audited\_md | problem\_labels\_per\_100m\_md |
|:-----------|:--------|:-------|:-------|:--------|:---------|:-----------|:----------------------|:-------------------|:---------------------|:-------------------------------|
| Anonymous  | 293     | 1181   | 10760  | 79.89   | 316      | 72%        | 10.51                 | 0.51               | 10.64                | 1.43                           |
| Registered | 188     | 5171   | 35923  | 391.57  | 1044     | 38%        | 6.80                  | 1.39               | 29.34                | 1.25                           |
| Turker     | 122     | 13207  | 103820 | 1016.42 | 2953     | 23%        | 8.90                  | 0.88               | 26.94                | 1.34                           |
| Researcher | 21      | 2717   | 30488  | 211.73  | 604      | 71%        | 7.08                  | 1.90               | 33.56                | 0.75                           |

Possible Stories
----------------

### Data overlap and agreement between users

Among all the data collected in DC, how much of DC is labeled by multiple users and what is the disagreement among them? (see comment in Outline document for details on implementation)

A total of 43.1% of streets were audited by multiple users.

### User dropoffs

NOTE: there are graphs that combine the tutorial and post-tutorial dropoff graphs at the end of this section.

#### Tutorial dropoffs

Below we look at how users drop off in the tutorial. Because most volunteers going through the tutorial have not yet registered, we simply use IP address as a "user" for the purpose of analyzing the tutorial in particular; however, turkers are automatically logged in immediately, so we use turker id for them. We consider only users who clicked on the "Let's get started!" button on the tutorial page as having started the tutorial. To simplify the analysis, we are looking at only tutorial attempts after the small additions we made to the tutorial last summer (so the data starts July 10th, 2017).

The steps in the graph below are as follows:

1.  Clicked "Let's get started!"
2.  Placed first curb ramp label *and correctly rated severity*
3.  Placed second curb ramp label and correctly rated severity
4.  Placed missing curb ramp label and correctly rated severity
5.  Placed third curb ramp label and correctly rated severity
6.  Placed fourth curb ramp label and correctly rated severity
7.  Placed no sidewalk label
8.  Placed fifth curb ramp label and correctly rated severity
9.  Took step forward and clicked through all dialog boxes to finish tutorial

Of the 1110 people who started the tutorial, 825 finished the first step (74%), and 568 completed the tutorial (51%).

The first graph shows both volunteers and turkers together, and the second shows them split.

![](stats_for_paper_files/figure-markdown_github/public.deployment.tutorial.dropoff.plots-1.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.tutorial.dropoff.plots-2.png)

#### Post tutorial dropoffs

Below we look at how users drop off after finishing the tutorial. For consistency with the tutorial analysis, we use IP address to denote a volunteer user and turker id to differentiate between turkers (the 568 IP addresses and turker ids who finished the tutorial in the previous section are the same 568 IPs/IDs that we are looking at below). As such, we are looking at only those who finished the tutorial after July 10th, 2017.

We also only counted missions completed in the two hours after finishing the tutorial. There was not a sizeable difference when looking at 1 hour, 2 hours, or 48 hours after finishing the tutorial, so I think that 2 hours works fine.

The steps in the graph below are as follows:

1.  Finished tutorial
2.  Took a step
3.  Completed first mission
4.  Completed second mission
5.  And so on...

Of the 568 people who finished the tutorial, 479 took a step (84%), 328 finished at least one mission afterwards (58%), 196 completed at least two missions afterwards (35%), and 28 did at least 10 missions afterwards (5%).

The first graph shows both volunteers and turkers together, and the second shows them split.

![](stats_for_paper_files/figure-markdown_github/public.deployment.post.tutorial.dropoff.plot-1.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.post.tutorial.dropoff.plot-2.png)

#### Tutorial and post tutorial dropoffs

Below are simply concatenations of the graphs from each of the two sub-sections above. These are followed by versions that use percentage of total users remaining instead of counts.

![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-1.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-2.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-3.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-4.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-5.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-6.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-7.png)![](stats_for_paper_files/figure-markdown_github/public.deployment.user.dropoff.plot-8.png)

Turk Study
==========

Update: This is now all of the data. There used to be 19 anonymous user routes, but three of them actually had no labels placed by the anonymous user (we had forgotten to check beforehand), thus we have only 16.

Even though 5 turkers did each route, the high level results for individual turkers looks only at the first turker to complete each set of routes. This makes aggregate stats more even, and a fairer comparison across user groups. (but maybe we should actually use all turkers when not aggregating, actually...)

High level results
------------------

### Ground truth label counts

Below is a table showing number of ground truth labels by user group and by label type.

| worker.type | All    | Problem | CurbRamp | NoCurbRamp | Obstacle | SurfaceProb |
|:------------|:-------|:--------|:---------|:-----------|:---------|:------------|
| anon        | 775    | 297     | 478      | 19         | 59       | 219         |
| reg         | 3842   | 1108    | 2734     | 68         | 236      | 804         |
| total       | 4617   | 1405    | 3212     | 87         | 295      | 1023        |
| % of total  | 100.0% | 30.4%   | 69.6%    | 1.9%       | 6.4%     | 22.2%       |

A total of 330 turkers, 50 registered users, and 16 anonymous users were part of this study.

### Aggregate accuracy

Below are two tables (street level, then 5 meter level) showing mean accuracy across all users when aggregating over all label types, and for problem vs no problem. We see that the All/Problem accuracies are comparable at the street level, but accuracy is much higher for curb ramps than problems at the 5 meter level (which makes Problem have a much lower accuracy than All).

NOTE: In these two tables, the data is binary (not ordinal), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

Mean accuracy across all users - street level:

| label.type | recall | precision | f.measure |
|:-----------|:-------|:----------|:----------|
| All        | 0.631  | 0.707     | 0.647     |
| Problem    | 0.577  | 0.707     | 0.602     |

Mean accuracy across all users - 5 meter level:

| label.type | recall | precision | f.measure |
|:-----------|:-------|:----------|:----------|
| All        | 0.445  | 0.490     | 0.445     |
| Problem    | 0.181  | 0.285     | 0.223     |

### Raw accuracy

Mean/median/sd raw accuracy by user group and label type - street level:

| user.group.label.type | md    | mn    | sd    |
|:----------------------|:------|:------|:------|
| anon, All             | 0.692 | 0.673 | 0.164 |
| anon, Problem         | 0.500 | 0.494 | 0.256 |
| anon, CurbRamp        | 0.944 | 0.764 | 0.322 |
| anon, NoCurbRamp      | 0.764 | 0.774 | 0.197 |
| anon, Obstacle        | 0.667 | 0.697 | 0.203 |
| anon, SurfaceProb     | 0.400 | 0.456 | 0.323 |
| reg, All              | 0.750 | 0.737 | 0.112 |
| reg, Problem          | 0.615 | 0.606 | 0.173 |
| reg, CurbRamp         | 0.926 | 0.836 | 0.236 |
| reg, NoCurbRamp       | 0.760 | 0.771 | 0.187 |
| reg, Obstacle         | 0.793 | 0.748 | 0.186 |
| reg, SurfaceProb      | 0.612 | 0.595 | 0.242 |
| turk1, All            | 0.720 | 0.712 | 0.132 |
| turk1, Problem        | 0.667 | 0.646 | 0.198 |
| turk1, CurbRamp       | 0.909 | 0.854 | 0.201 |
| turk1, NoCurbRamp     | 0.750 | 0.685 | 0.240 |
| turk1, Obstacle       | 0.714 | 0.689 | 0.186 |
| turk1, SurfaceProb    | 0.667 | 0.620 | 0.225 |

Mean/median/sd raw accuracy by user group and label type - 5 meter level:

| user.group.label.type | md    | mn    | sd    |
|:----------------------|:------|:------|:------|
| anon, All             | 0.954 | 0.946 | 0.026 |
| anon, Problem         | 0.870 | 0.863 | 0.076 |
| anon, CurbRamp        | 0.948 | 0.936 | 0.053 |
| anon, NoCurbRamp      | 0.984 | 0.981 | 0.019 |
| anon, Obstacle        | 0.976 | 0.970 | 0.025 |
| anon, SurfaceProb     | 0.912 | 0.898 | 0.075 |
| reg, All              | 0.955 | 0.952 | 0.021 |
| reg, Problem          | 0.924 | 0.909 | 0.055 |
| reg, CurbRamp         | 0.918 | 0.911 | 0.046 |
| reg, NoCurbRamp       | 0.987 | 0.981 | 0.021 |
| reg, Obstacle         | 0.976 | 0.971 | 0.025 |
| reg, SurfaceProb      | 0.960 | 0.945 | 0.047 |
| turk1, All            | 0.951 | 0.941 | 0.033 |
| turk1, Problem        | 0.893 | 0.874 | 0.081 |
| turk1, CurbRamp       | 0.933 | 0.918 | 0.043 |
| turk1, NoCurbRamp     | 0.984 | 0.961 | 0.069 |
| turk1, Obstacle       | 0.970 | 0.958 | 0.038 |
| turk1, SurfaceProb    | 0.948 | 0.929 | 0.057 |

### Voting: Improved recall when at least one turker marks

Since dealing with false positives is pretty easy (relative to walking through GSV), the most important thing for us is to maximize recall. So how does recall look if we consider a label placed by at least one turker as a potential attribute (i.e., we use the "at least one" voting method)?

For reference, individual turkers tended to have the best performance among our user groups, and their recall for problem vs no problem was 0.66 and their precision was 0.71.

NOTE: In this section we are looking at *problem vs no problem*, the data are binary (not ordinal), the data are at the street level (not 5 meter level), and we are looking at 5 clustered turkers with the "at least one" voting method.

*Takeaways*:

-   The mean recall improves significantly when going from majority vote to the "at least one" voting method, accompanied by a much smaller decrease in precision. Since recall is much more important to us, this is the voting method we should likely use going forward.

| voting.method | recall | precision |
|:--------------|:-------|:----------|
| majority.vote | 0.425  | 0.819     |
| at.least.one  | 0.962  | 0.643     |

### Descriptive stats for users

Next we have some descriptive statistics of users, by user group. These are average (mean/median) stats.

NOTE: In this table, we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

| worker.type | n.users | labels.per.100m\_md | labels.per.100m\_mn | labels.per.100m\_sd | minutes\_audited\_md | minutes\_audited\_mn | minutes\_audited\_sd | m.p.min\_md | m.p.min\_mn | m.p.min\_sd | sec.p.label\_md | sec.p.label\_mn | sec.p.label\_sd |
|:------------|:--------|:--------------------|:--------------------|:--------------------|:---------------------|:---------------------|:---------------------|:------------|:------------|:------------|:----------------|:----------------|:----------------|
| anon        | 16      | 4.921               | 5.116               | 3.382               | 12.865               | 14.332               | 6.327                | 47.429      | 55.232      | 38.167      | 6.939           | 10.446          | 10.058          |
| reg         | 50      | 5.988               | 6.063               | 3.457               | 23.660               | 26.226               | 13.297               | 51.583      | 58.561      | 30.531      | 5.232           | 5.943           | 2.522           |
| turk1       | 66      | 7.669               | 8.320               | 4.109               | 33.695               | 37.717               | 16.108               | 30.825      | 31.597      | 11.112      | 8.416           | 9.599           | 4.560           |

Below, we have a table of aggregate (sum) stats by user group.

NOTE: In this table, we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

| worker.type | n.users | distance.km | distance.miles | hours.audited | n.labels | n.missions |
|:------------|:--------|:------------|:---------------|:--------------|:---------|:-----------|
| anon        | 16      | 9.754       | 6.061          | 3.822         | 499      | 32         |
| reg         | 50      | 60.960      | 37.879         | 21.855        | 3696     | 150        |
| turk1       | 66      | 70.714      | 43.939         | 41.489        | 5711     | 182        |

### Giant table with accuracy

| label.type.granularity.user.group     | prec\_md | prec\_mn | prec\_sd | prec\_se | raw.acc\_md | raw.acc\_mn | raw.acc\_sd | raw.acc\_se | rec\_md | rec\_mn | rec\_sd | rec\_se |
|:--------------------------------------|:---------|:---------|:---------|:---------|:------------|:------------|:------------|:------------|:--------|:--------|:--------|:--------|
| All, street, all                      | 0.714    | 0.707    | 0.172    | 0.015    | 0.729       | 0.717       | 0.130       | 0.011       | 0.651   | 0.631   | 0.216   | 0.019   |
| All, street, anon                     | 0.833    | 0.745    | 0.293    | 0.076    | 0.692       | 0.673       | 0.164       | 0.041       | 0.523   | 0.488   | 0.242   | 0.060   |
| All, street, reg                      | 0.728    | 0.722    | 0.131    | 0.019    | 0.750       | 0.737       | 0.112       | 0.016       | 0.651   | 0.614   | 0.215   | 0.030   |
| All, street, turk1                    | 0.700    | 0.688    | 0.164    | 0.020    | 0.720       | 0.712       | 0.132       | 0.016       | 0.707   | 0.678   | 0.194   | 0.024   |
| All, street, turk3.all                | 0.605    | 0.601    | 0.126    | 0.016    | 0.694       | 0.692       | 0.104       | 0.013       | 0.875   | 0.867   | 0.100   | 0.012   |
| All, street, turk3.maj.vote           | 0.815    | 0.810    | 0.155    | 0.019    | 0.778       | 0.759       | 0.119       | 0.015       | 0.620   | 0.621   | 0.153   | 0.019   |
| All, street, turk5.all                | 0.545    | 0.550    | 0.114    | 0.014    | 0.641       | 0.645       | 0.099       | 0.012       | 0.918   | 0.917   | 0.079   | 0.010   |
| All, street, turk5.maj.vote           | 0.913    | 0.874    | 0.139    | 0.017    | 0.798       | 0.777       | 0.111       | 0.014       | 0.571   | 0.595   | 0.140   | 0.017   |
| All, 5\_meter, all                    | 0.500    | 0.490    | 0.199    | 0.017    | 0.952       | 0.946       | 0.029       | 0.002       | 0.466   | 0.445   | 0.226   | 0.020   |
| All, 5\_meter, anon                   | 0.680    | 0.587    | 0.277    | 0.072    | 0.954       | 0.946       | 0.026       | 0.007       | 0.415   | 0.375   | 0.262   | 0.065   |
| All, 5\_meter, reg                    | 0.525    | 0.493    | 0.192    | 0.027    | 0.955       | 0.952       | 0.021       | 0.003       | 0.430   | 0.421   | 0.203   | 0.029   |
| All, 5\_meter, turk1                  | 0.489    | 0.466    | 0.180    | 0.022    | 0.951       | 0.941       | 0.033       | 0.004       | 0.490   | 0.481   | 0.231   | 0.028   |
| All, 5\_meter, turk3.all              | 0.376    | 0.376    | 0.134    | 0.016    | 0.933       | 0.922       | 0.039       | 0.005       | 0.656   | 0.656   | 0.159   | 0.020   |
| All, 5\_meter, turk3.maj.vote         | 0.667    | 0.646    | 0.177    | 0.022    | 0.963       | 0.959       | 0.024       | 0.003       | 0.509   | 0.502   | 0.195   | 0.024   |
| All, 5\_meter, turk5.all              | 0.292    | 0.300    | 0.114    | 0.014    | 0.906       | 0.895       | 0.046       | 0.006       | 0.701   | 0.697   | 0.141   | 0.017   |
| All, 5\_meter, turk5.maj.vote         | 0.756    | 0.714    | 0.176    | 0.022    | 0.969       | 0.963       | 0.022       | 0.003       | 0.504   | 0.507   | 0.172   | 0.021   |
| Problem, street, all                  | 0.750    | 0.707    | 0.286    | 0.025    | 0.625       | 0.612       | 0.201       | 0.018       | 0.600   | 0.577   | 0.312   | 0.027   |
| Problem, street, anon                 | 1.000    | 0.879    | 0.214    | 0.059    | 0.500       | 0.494       | 0.256       | 0.064       | 0.250   | 0.394   | 0.346   | 0.086   |
| Problem, street, reg                  | 0.714    | 0.661    | 0.289    | 0.041    | 0.615       | 0.606       | 0.173       | 0.024       | 0.500   | 0.529   | 0.293   | 0.042   |
| Problem, street, turk1                | 0.750    | 0.708    | 0.288    | 0.035    | 0.667       | 0.646       | 0.198       | 0.024       | 0.700   | 0.658   | 0.294   | 0.037   |
| Problem, street, turk3.all            | 0.683    | 0.673    | 0.240    | 0.030    | 0.714       | 0.696       | 0.191       | 0.024       | 1.000   | 0.910   | 0.131   | 0.016   |
| Problem, street, turk3.maj.vote       | 0.800    | 0.762    | 0.287    | 0.036    | 0.677       | 0.646       | 0.206       | 0.025       | 0.571   | 0.554   | 0.303   | 0.038   |
| Problem, street, turk5.all            | 0.667    | 0.643    | 0.239    | 0.029    | 0.714       | 0.677       | 0.205       | 0.025       | 1.000   | 0.962   | 0.096   | 0.012   |
| Problem, street, turk5.maj.vote       | 1.000    | 0.819    | 0.237    | 0.031    | 0.655       | 0.618       | 0.198       | 0.024       | 0.375   | 0.425   | 0.270   | 0.034   |
| Problem, 5\_meter, all                | 0.250    | 0.285    | 0.258    | 0.023    | 0.907       | 0.886       | 0.074       | 0.006       | 0.131   | 0.181   | 0.175   | 0.015   |
| Problem, 5\_meter, anon               | 0.500    | 0.514    | 0.377    | 0.105    | 0.870       | 0.863       | 0.076       | 0.019       | 0.079   | 0.104   | 0.099   | 0.025   |
| Problem, 5\_meter, reg                | 0.200    | 0.225    | 0.199    | 0.028    | 0.924       | 0.909       | 0.055       | 0.008       | 0.125   | 0.137   | 0.136   | 0.019   |
| Problem, 5\_meter, turk1              | 0.272    | 0.285    | 0.248    | 0.031    | 0.893       | 0.874       | 0.081       | 0.010       | 0.200   | 0.232   | 0.200   | 0.025   |
| Problem, 5\_meter, turk3.all          | 0.206    | 0.229    | 0.154    | 0.019    | 0.856       | 0.833       | 0.081       | 0.010       | 0.353   | 0.400   | 0.211   | 0.026   |
| Problem, 5\_meter, turk3.maj.vote     | 0.333    | 0.311    | 0.255    | 0.032    | 0.919       | 0.900       | 0.069       | 0.009       | 0.125   | 0.148   | 0.134   | 0.017   |
| Problem, 5\_meter, turk5.all          | 0.176    | 0.207    | 0.135    | 0.017    | 0.803       | 0.793       | 0.096       | 0.012       | 0.500   | 0.500   | 0.197   | 0.024   |
| Problem, 5\_meter, turk5.maj.vote     | 0.333    | 0.423    | 0.333    | 0.043    | 0.926       | 0.907       | 0.072       | 0.009       | 0.091   | 0.108   | 0.106   | 0.013   |
| CurbRamp, street, all                 | 1.000    | 0.954    | 0.075    | 0.007    | 0.909       | 0.837       | 0.231       | 0.020       | 1.000   | 0.860   | 0.257   | 0.022   |
| CurbRamp, street, anon                | 1.000    | 0.970    | 0.077    | 0.021    | 0.944       | 0.764       | 0.322       | 0.080       | 1.000   | 0.743   | 0.371   | 0.093   |
| CurbRamp, street, reg                 | 1.000    | 0.959    | 0.077    | 0.011    | 0.926       | 0.836       | 0.236       | 0.033       | 1.000   | 0.856   | 0.257   | 0.036   |
| CurbRamp, street, turk1               | 1.000    | 0.947    | 0.072    | 0.009    | 0.909       | 0.854       | 0.201       | 0.025       | 1.000   | 0.890   | 0.216   | 0.027   |
| CurbRamp, street, turk3.all           | 1.000    | 0.931    | 0.087    | 0.011    | 0.941       | 0.932       | 0.079       | 0.010       | 1.000   | 0.995   | 0.023   | 0.003   |
| CurbRamp, street, turk3.maj.vote      | 1.000    | 0.957    | 0.084    | 0.010    | 1.000       | 0.917       | 0.124       | 0.015       | 1.000   | 0.945   | 0.115   | 0.014   |
| CurbRamp, street, turk5.all           | 0.926    | 0.915    | 0.102    | 0.013    | 0.926       | 0.917       | 0.093       | 0.011       | 1.000   | 0.996   | 0.021   | 0.003   |
| CurbRamp, street, turk5.maj.vote      | 1.000    | 0.962    | 0.079    | 0.010    | 1.000       | 0.934       | 0.103       | 0.013       | 1.000   | 0.958   | 0.091   | 0.011   |
| CurbRamp, 5\_meter, all               | 0.684    | 0.646    | 0.218    | 0.019    | 0.928       | 0.918       | 0.046       | 0.004       | 0.722   | 0.645   | 0.272   | 0.024   |
| CurbRamp, 5\_meter, anon              | 0.813    | 0.822    | 0.133    | 0.036    | 0.948       | 0.936       | 0.053       | 0.013       | 0.670   | 0.599   | 0.368   | 0.092   |
| CurbRamp, 5\_meter, reg               | 0.645    | 0.603    | 0.235    | 0.033    | 0.918       | 0.911       | 0.046       | 0.006       | 0.681   | 0.616   | 0.242   | 0.034   |
| CurbRamp, 5\_meter, turk1             | 0.684    | 0.641    | 0.203    | 0.025    | 0.933       | 0.918       | 0.043       | 0.005       | 0.767   | 0.679   | 0.268   | 0.033   |
| CurbRamp, 5\_meter, turk3.all         | 0.566    | 0.566    | 0.171    | 0.021    | 0.904       | 0.905       | 0.045       | 0.006       | 0.925   | 0.907   | 0.086   | 0.011   |
| CurbRamp, 5\_meter, turk3.maj.vote    | 0.768    | 0.742    | 0.149    | 0.018    | 0.949       | 0.944       | 0.030       | 0.004       | 0.833   | 0.773   | 0.191   | 0.024   |
| CurbRamp, 5\_meter, turk5.all         | 0.489    | 0.483    | 0.152    | 0.019    | 0.871       | 0.874       | 0.053       | 0.007       | 0.946   | 0.932   | 0.069   | 0.009   |
| CurbRamp, 5\_meter, turk5.maj.vote    | 0.793    | 0.772    | 0.146    | 0.018    | 0.956       | 0.950       | 0.026       | 0.003       | 0.811   | 0.798   | 0.148   | 0.018   |
| NoCurbRamp, street, all               | 0.000    | 0.205    | 0.317    | 0.030    | 0.750       | 0.729       | 0.219       | 0.019       | 1.000   | 0.693   | 0.435   | 0.056   |
| NoCurbRamp, street, anon              | 0.000    | 0.265    | 0.376    | 0.119    | 0.764       | 0.774       | 0.197       | 0.049       | 0.667   | 0.556   | 0.502   | 0.205   |
| NoCurbRamp, street, reg               | 0.000    | 0.225    | 0.339    | 0.052    | 0.760       | 0.771       | 0.187       | 0.026       | 1.000   | 0.722   | 0.422   | 0.086   |
| NoCurbRamp, street, turk1             | 0.000    | 0.181    | 0.292    | 0.038    | 0.750       | 0.685       | 0.240       | 0.030       | 1.000   | 0.697   | 0.442   | 0.081   |
| NoCurbRamp, street, turk3.all         | 0.000    | 0.152    | 0.225    | 0.028    | 0.500       | 0.504       | 0.229       | 0.028       | 1.000   | 0.878   | 0.309   | 0.056   |
| NoCurbRamp, street, turk3.maj.vote    | 0.000    | 0.277    | 0.379    | 0.055    | 0.833       | 0.804       | 0.203       | 0.025       | 0.667   | 0.586   | 0.441   | 0.081   |
| NoCurbRamp, street, turk5.all         | 0.000    | 0.120    | 0.169    | 0.021    | 0.360       | 0.371       | 0.227       | 0.028       | 1.000   | 0.967   | 0.183   | 0.033   |
| NoCurbRamp, street, turk5.maj.vote    | 0.000    | 0.340    | 0.417    | 0.065    | 0.879       | 0.848       | 0.192       | 0.024       | 0.500   | 0.486   | 0.432   | 0.079   |
| NoCurbRamp, 5\_meter, all             | 0.000    | 0.135    | 0.271    | 0.026    | 0.984       | 0.971       | 0.052       | 0.005       | 0.500   | 0.498   | 0.441   | 0.057   |
| NoCurbRamp, 5\_meter, anon            | 0.000    | 0.233    | 0.353    | 0.112    | 0.984       | 0.981       | 0.019       | 0.005       | 0.417   | 0.383   | 0.336   | 0.137   |
| NoCurbRamp, 5\_meter, reg             | 0.000    | 0.158    | 0.300    | 0.046    | 0.987       | 0.981       | 0.021       | 0.003       | 0.633   | 0.558   | 0.446   | 0.091   |
| NoCurbRamp, 5\_meter, turk1           | 0.000    | 0.103    | 0.230    | 0.030    | 0.984       | 0.961       | 0.069       | 0.009       | 0.450   | 0.472   | 0.460   | 0.084   |
| NoCurbRamp, 5\_meter, turk3.all       | 0.000    | 0.077    | 0.139    | 0.017    | 0.952       | 0.931       | 0.079       | 0.010       | 1.000   | 0.708   | 0.420   | 0.077   |
| NoCurbRamp, 5\_meter, turk3.maj.vote  | 0.000    | 0.222    | 0.365    | 0.053    | 0.991       | 0.979       | 0.056       | 0.007       | 0.400   | 0.433   | 0.444   | 0.081   |
| NoCurbRamp, 5\_meter, turk5.all       | 0.000    | 0.051    | 0.091    | 0.011    | 0.928       | 0.888       | 0.105       | 0.013       | 1.000   | 0.838   | 0.346   | 0.063   |
| NoCurbRamp, 5\_meter, turk5.maj.vote  | 0.000    | 0.275    | 0.395    | 0.062    | 0.994       | 0.985       | 0.046       | 0.006       | 0.325   | 0.393   | 0.410   | 0.075   |
| Obstacle, street, all                 | 0.500    | 0.475    | 0.374    | 0.038    | 0.750       | 0.712       | 0.188       | 0.016       | 0.450   | 0.399   | 0.369   | 0.035   |
| Obstacle, street, anon                | 1.000    | 0.690    | 0.413    | 0.156    | 0.667       | 0.697       | 0.203       | 0.051       | 0.000   | 0.226   | 0.311   | 0.083   |
| Obstacle, street, reg                 | 0.500    | 0.478    | 0.388    | 0.064    | 0.793       | 0.748       | 0.186       | 0.026       | 0.333   | 0.380   | 0.374   | 0.057   |
| Obstacle, street, turk1               | 0.500    | 0.445    | 0.357    | 0.048    | 0.714       | 0.689       | 0.186       | 0.023       | 0.500   | 0.455   | 0.368   | 0.049   |
| Obstacle, street, turk3.all           | 0.400    | 0.413    | 0.304    | 0.037    | 0.667       | 0.624       | 0.211       | 0.026       | 1.000   | 0.804   | 0.265   | 0.035   |
| Obstacle, street, turk3.maj.vote      | 0.500    | 0.536    | 0.405    | 0.063    | 0.750       | 0.743       | 0.167       | 0.021       | 0.250   | 0.324   | 0.364   | 0.048   |
| Obstacle, street, turk5.all           | 0.333    | 0.381    | 0.260    | 0.032    | 0.640       | 0.597       | 0.204       | 0.025       | 1.000   | 0.885   | 0.201   | 0.027   |
| Obstacle, street, turk5.maj.vote      | 1.000    | 0.800    | 0.385    | 0.070    | 0.818       | 0.773       | 0.188       | 0.023       | 0.000   | 0.225   | 0.317   | 0.042   |
| Obstacle, 5\_meter, all               | 0.091    | 0.182    | 0.235    | 0.024    | 0.972       | 0.964       | 0.033       | 0.003       | 0.000   | 0.161   | 0.228   | 0.021   |
| Obstacle, 5\_meter, anon              | 0.250    | 0.274    | 0.356    | 0.135    | 0.976       | 0.970       | 0.025       | 0.006       | 0.000   | 0.073   | 0.146   | 0.039   |
| Obstacle, 5\_meter, reg               | 0.042    | 0.143    | 0.195    | 0.032    | 0.976       | 0.971       | 0.025       | 0.004       | 0.000   | 0.134   | 0.218   | 0.033   |
| Obstacle, 5\_meter, turk1             | 0.111    | 0.196    | 0.242    | 0.033    | 0.970       | 0.958       | 0.038       | 0.005       | 0.143   | 0.203   | 0.245   | 0.033   |
| Obstacle, 5\_meter, turk3.all         | 0.100    | 0.153    | 0.190    | 0.023    | 0.944       | 0.930       | 0.052       | 0.006       | 0.375   | 0.378   | 0.300   | 0.040   |
| Obstacle, 5\_meter, turk3.maj.vote    | 0.000    | 0.200    | 0.295    | 0.046    | 0.976       | 0.973       | 0.023       | 0.003       | 0.000   | 0.120   | 0.231   | 0.031   |
| Obstacle, 5\_meter, turk5.all         | 0.095    | 0.121    | 0.140    | 0.017    | 0.932       | 0.913       | 0.059       | 0.007       | 0.500   | 0.464   | 0.320   | 0.042   |
| Obstacle, 5\_meter, turk5.maj.vote    | 0.071    | 0.354    | 0.423    | 0.077    | 0.982       | 0.979       | 0.020       | 0.002       | 0.000   | 0.095   | 0.226   | 0.030   |
| SurfaceProb, street, all              | 1.000    | 0.726    | 0.354    | 0.039    | 0.625       | 0.590       | 0.248       | 0.022       | 0.200   | 0.271   | 0.305   | 0.027   |
| SurfaceProb, street, anon             | 1.000    | 0.914    | 0.227    | 0.086    | 0.400       | 0.456       | 0.323       | 0.081       | 0.000   | 0.188   | 0.248   | 0.062   |
| SurfaceProb, street, reg              | 0.667    | 0.658    | 0.378    | 0.069    | 0.612       | 0.595       | 0.242       | 0.034       | 0.100   | 0.187   | 0.225   | 0.032   |
| SurfaceProb, street, turk1            | 1.000    | 0.742    | 0.347    | 0.051    | 0.667       | 0.620       | 0.225       | 0.028       | 0.300   | 0.355   | 0.347   | 0.043   |
| SurfaceProb, street, turk3.all        | 0.750    | 0.729    | 0.286    | 0.036    | 0.742       | 0.706       | 0.176       | 0.022       | 0.600   | 0.635   | 0.290   | 0.036   |
| SurfaceProb, street, turk3.maj.vote   | 1.000    | 0.862    | 0.324    | 0.060    | 0.620       | 0.574       | 0.263       | 0.032       | 0.000   | 0.142   | 0.218   | 0.027   |
| SurfaceProb, street, turk5.all        | 0.667    | 0.666    | 0.296    | 0.037    | 0.703       | 0.693       | 0.208       | 0.026       | 0.800   | 0.782   | 0.231   | 0.029   |
| SurfaceProb, street, turk5.maj.vote   | 1.000    | 1.000    | 0.000    | 0.000    | 0.583       | 0.553       | 0.269       | 0.033       | 0.000   | 0.067   | 0.131   | 0.016   |
| SurfaceProb, 5\_meter, all            | 0.250    | 0.342    | 0.354    | 0.039    | 0.952       | 0.931       | 0.057       | 0.005       | 0.000   | 0.096   | 0.193   | 0.017   |
| SurfaceProb, 5\_meter, anon           | 0.250    | 0.369    | 0.451    | 0.170    | 0.912       | 0.898       | 0.075       | 0.019       | 0.000   | 0.047   | 0.097   | 0.024   |
| SurfaceProb, 5\_meter, reg            | 0.292    | 0.303    | 0.304    | 0.056    | 0.960       | 0.945       | 0.047       | 0.007       | 0.000   | 0.068   | 0.144   | 0.021   |
| SurfaceProb, 5\_meter, turk1          | 0.250    | 0.363    | 0.373    | 0.054    | 0.948       | 0.929       | 0.057       | 0.007       | 0.000   | 0.129   | 0.235   | 0.029   |
| SurfaceProb, 5\_meter, turk3.all      | 0.273    | 0.319    | 0.268    | 0.034    | 0.934       | 0.921       | 0.054       | 0.007       | 0.154   | 0.245   | 0.264   | 0.033   |
| SurfaceProb, 5\_meter, turk3.maj.vote | 1.000    | 0.651    | 0.431    | 0.080    | 0.960       | 0.938       | 0.055       | 0.007       | 0.000   | 0.049   | 0.083   | 0.010   |
| SurfaceProb, 5\_meter, turk5.all      | 0.231    | 0.287    | 0.252    | 0.031    | 0.902       | 0.906       | 0.056       | 0.007       | 0.250   | 0.297   | 0.253   | 0.031   |
| SurfaceProb, 5\_meter, turk5.maj.vote | 1.000    | 0.941    | 0.243    | 0.059    | 0.960       | 0.939       | 0.054       | 0.007       | 0.000   | 0.029   | 0.063   | 0.008   |

### IRR

NOTE: In these tables, the data is binary (not ordinal), and is at the street level (not 5 meter level).

Our average (mean) IRR (kripp alpha) over the 7 rounds, by label type, is in the first table below, followed by mean kripp alpha across label types for each round, followed by the IRR in each round by label type:

| label.type  | mean.kripp.alpha |
|:------------|:-----------------|
| CurbRamp    | 0.907            |
| NoCurbRamp  | 0.787            |
| Obstacle    | 0.342            |
| SurfaceProb | 0.477            |
| Problem     | 0.475            |

| round.code | mean.kripp.alpha |
|:-----------|:-----------------|
| 1          | 0.717            |
| 2          | 0.503            |
| 3          | 0.651            |
| 4          | 0.405            |
| 5          | 0.574            |
| 6          | 0.808            |
| 7          | 0.526            |

| label.type.round | kripp.alpha |
|:-----------------|:------------|
| CurbRamp, 1      | 1.000       |
| CurbRamp, 2      | 0.712       |
| CurbRamp, 3      | 1.000       |
| CurbRamp, 4      | 1.000       |
| CurbRamp, 5      | 0.636       |
| CurbRamp, 6      | 1.000       |
| CurbRamp, 7      | 1.000       |
| NoCurbRamp, 1    | 1.000       |
| NoCurbRamp, 2    | 0.000       |
| NoCurbRamp, 3    | 1.000       |
| NoCurbRamp, 4    | 0.769       |
| NoCurbRamp, 5    | 1.000       |
| NoCurbRamp, 6    | 1.000       |
| NoCurbRamp, 7    | 0.741       |
| Obstacle, 1      | 0.546       |
| Obstacle, 2      | 0.659       |
| Obstacle, 3      | 0.304       |
| Obstacle, 4      | 0.192       |
| Obstacle, 5      | 0.227       |
| Obstacle, 6      | 0.351       |
| Obstacle, 7      | 0.115       |
| SurfaceProb, 1   | 0.470       |
| SurfaceProb, 2   | 0.532       |
| SurfaceProb, 3   | 0.425       |
| SurfaceProb, 4   | 0.130       |
| SurfaceProb, 5   | 0.605       |
| SurfaceProb, 6   | 0.846       |
| SurfaceProb, 7   | 0.333       |
| Problem, 1       | 0.568       |
| Problem, 2       | 0.611       |
| Problem, 3       | 0.525       |
| Problem, 4       | -0.068      |
| Problem, 5       | 0.404       |
| Problem, 6       | 0.843       |
| Problem, 7       | 0.442       |

Possible Stories
----------------

### Granularity: Street-level vs 5 meter-level

Below we compare street vs 5 meter level recall and precision by label type.

NOTE: In this section, the data is binary (not ordinal), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

Below is a table showing label type accuracy at the two granularity levels, followed by a graph that gives a visual representation of the mean and standard error.

| accuracy.type | label.type  | granularity | mean.accuracy | median.accuracy | sd    | se    |
|:--------------|:------------|:------------|:--------------|:----------------|:------|:------|
| recall        | All         | street      | 0.631         | 0.651           | 0.216 | 0.019 |
| recall        | All         | 5\_meter    | 0.445         | 0.466           | 0.226 | 0.020 |
| recall        | Problem     | street      | 0.577         | 0.600           | 0.312 | 0.027 |
| recall        | Problem     | 5\_meter    | 0.181         | 0.131           | 0.175 | 0.015 |
| recall        | CurbRamp    | street      | 0.860         | 1.000           | 0.257 | 0.022 |
| recall        | CurbRamp    | 5\_meter    | 0.645         | 0.722           | 0.272 | 0.024 |
| recall        | NoCurbRamp  | street      | 0.693         | 1.000           | 0.435 | 0.056 |
| recall        | NoCurbRamp  | 5\_meter    | 0.498         | 0.500           | 0.441 | 0.057 |
| recall        | Obstacle    | street      | 0.399         | 0.450           | 0.369 | 0.035 |
| recall        | Obstacle    | 5\_meter    | 0.161         | 0.000           | 0.228 | 0.021 |
| recall        | SurfaceProb | street      | 0.271         | 0.200           | 0.305 | 0.027 |
| recall        | SurfaceProb | 5\_meter    | 0.096         | 0.000           | 0.193 | 0.017 |
| precision     | All         | street      | 0.707         | 0.714           | 0.172 | 0.015 |
| precision     | All         | 5\_meter    | 0.490         | 0.500           | 0.199 | 0.017 |
| precision     | Problem     | street      | 0.707         | 0.750           | 0.286 | 0.025 |
| precision     | Problem     | 5\_meter    | 0.285         | 0.250           | 0.258 | 0.023 |
| precision     | CurbRamp    | street      | 0.954         | 1.000           | 0.075 | 0.007 |
| precision     | CurbRamp    | 5\_meter    | 0.646         | 0.684           | 0.218 | 0.019 |
| precision     | NoCurbRamp  | street      | 0.205         | 0.000           | 0.317 | 0.030 |
| precision     | NoCurbRamp  | 5\_meter    | 0.135         | 0.000           | 0.271 | 0.026 |
| precision     | Obstacle    | street      | 0.475         | 0.500           | 0.374 | 0.038 |
| precision     | Obstacle    | 5\_meter    | 0.182         | 0.091           | 0.235 | 0.024 |
| precision     | SurfaceProb | street      | 0.726         | 1.000           | 0.354 | 0.039 |
| precision     | SurfaceProb | 5\_meter    | 0.342         | 0.250           | 0.354 | 0.039 |
| raw.accuracy  | All         | street      | 0.717         | 0.729           | 0.130 | 0.011 |
| raw.accuracy  | All         | 5\_meter    | 0.946         | 0.952           | 0.029 | 0.002 |
| raw.accuracy  | Problem     | street      | 0.612         | 0.625           | 0.201 | 0.018 |
| raw.accuracy  | Problem     | 5\_meter    | 0.886         | 0.907           | 0.074 | 0.006 |
| raw.accuracy  | CurbRamp    | street      | 0.837         | 0.909           | 0.231 | 0.020 |
| raw.accuracy  | CurbRamp    | 5\_meter    | 0.918         | 0.928           | 0.046 | 0.004 |
| raw.accuracy  | NoCurbRamp  | street      | 0.729         | 0.750           | 0.219 | 0.019 |
| raw.accuracy  | NoCurbRamp  | 5\_meter    | 0.971         | 0.984           | 0.052 | 0.005 |
| raw.accuracy  | Obstacle    | street      | 0.712         | 0.750           | 0.188 | 0.016 |
| raw.accuracy  | Obstacle    | 5\_meter    | 0.964         | 0.972           | 0.033 | 0.003 |
| raw.accuracy  | SurfaceProb | street      | 0.590         | 0.625           | 0.248 | 0.022 |
| raw.accuracy  | SurfaceProb | 5\_meter    | 0.931         | 0.952           | 0.057 | 0.005 |

![](stats_for_paper_files/figure-markdown_github/turk.granularity.analysis-1.png)

And here is the same graph as above for only "good users", i.e., those with a labeling frequency over 3.75 labels per 100 meters.

![](stats_for_paper_files/figure-markdown_github/turk.granularity.analysis.good.users-1.png)

*Takeaways*:

-   Analyzing at the 5 meter level shows higher raw accuracy and specificity, both because of the large number of true negatives that we get from splitting into 5 meter segments; there are very few street segments with no labels at all.

-   Analyzing at the street level shows higher recall, implying that there were relatively fewer false negatives at the street level. This may mean that users aren't finding *every* issue, but they are more likely to find *at least one* issue of that type when there are multiple that occur on the same street.

-   Analyzing at the street level shows higher precision, implying that there were relatively fewer false positives at the street level. I suspect that this is due to fundamental misunderstandings about how to label (implying both that labeling is complex and difficult and that our onboarding is insufficient) which are persistent/consistent and frequent (think: labeling driveways as curb ramps, labeling storm drains as missing curb ramps, and labeling fire hydrants or street signs that are not in the way as obstacles). In those cases where the mistake is made frequently (multiple times per street), relatively fewer false positives makes sense when moving to street level analysis.

-   Analyzing at the street level shows higher f-measure. This clearly comes from the higher recall and precision.

-   CurbRamp pretty much outperforms all other label types across the board, regardless of accuracy type of 5 meter vs. street level. This is likely because curb ramps are the easiest label type to understand and find in GSV (both because they are large and easy to see, and because you know where to expect them -- at intersections).

-   The SurfaceProblem label type seems to have the highest precision and lowest recall among the different types of issues (I'm excluding CurbRamp here). I guess that, relative to the other types of issues, there are just fewer cases of mistaking something of a surface problem and more cases of not finding a surface problem that was visible in GSV (so maybe surface problems require increased diligence from users, and the other issues require better treatment in onboarding).

-   The Problem type seems to perform better than the surface problem and obstacle label types (except for surface problem precision, mentioned in the previous bullet).

-   NoCurbRamp seems to have high recall and low precision. This fits my intuition; since users know to expect curb ramps at intersections, if they arrive at an intersection and a curb ramp is not there, they know to place a NoCurbRamp label. However, if there was no sidewalk at all, then we did not add the missing curb ramp labels to the ground truth dataset, and this is not something that we covered during onboarding. I suspect that this, paired with users marking storm drains as missing curb ramps, were the main reasons for the low recall. Both could be addressed through proper training.

### Accuracy by user group

NOTE: In these two tables, the data is binary (not ordinal) and these are mean/median accuracies aggregated across all label types (all.\*) and for the problem vs no problem type (prob.\*).

#### Summary stats

Mean/median/sd accuracy by user group - street level:

| user.type      | all.rec.mn | all.rec.md | all.rec.sd | all.rec.se | all.prec.mn | all.prec.md | all.prec.sd | all.prec.se | all.f.mn | all.f.md | all.f.sd | all.f.se | prob.rec.mn | prob.rec.md | prob.rec.sd | prob.rec.se | prob.prec.mn | prob.prec.md | prob.prec.sd | prob.prec.se | prob.f.mn | prob.f.md | prob.f.sd | prob.f.se |
|:---------------|:-----------|:-----------|:-----------|:-----------|:------------|:------------|:------------|:------------|:---------|:---------|:---------|:---------|:------------|:------------|:------------|:------------|:-------------|:-------------|:-------------|:-------------|:----------|:----------|:----------|:----------|
| all            | 0.631      | 0.651      | 0.216      | 0.019      | 0.707       | 0.714       | 0.172       | 0.015       | 0.647    | 0.667    | 0.149    | 0.013    | 0.577       | 0.600       | 0.312       | 0.027       | 0.707        | 0.750        | 0.286        | 0.025        | 0.602     | 0.600     | 0.231     | 0.021     |
| anon           | 0.488      | 0.523      | 0.242      | 0.060      | 0.745       | 0.833       | 0.293       | 0.076       | 0.642    | 0.686    | 0.156    | 0.042    | 0.394       | 0.250       | 0.346       | 0.086       | 0.879        | 1.000        | 0.214        | 0.059        | 0.551     | 0.500     | 0.240     | 0.067     |
| reg            | 0.614      | 0.651      | 0.215      | 0.030      | 0.722       | 0.728       | 0.131       | 0.019       | 0.635    | 0.677    | 0.161    | 0.023    | 0.529       | 0.500       | 0.293       | 0.042       | 0.661        | 0.714        | 0.289        | 0.041        | 0.560     | 0.571     | 0.221     | 0.032     |
| turk1          | 0.678      | 0.707      | 0.194      | 0.024      | 0.688       | 0.700       | 0.164       | 0.020       | 0.658    | 0.648    | 0.140    | 0.017    | 0.658       | 0.700       | 0.294       | 0.037       | 0.708        | 0.750        | 0.288        | 0.035        | 0.644     | 0.667     | 0.233     | 0.029     |
| turk3.all      | 0.867      | 0.875      | 0.100      | 0.012      | 0.601       | 0.605       | 0.126       | 0.016       | 0.698    | 0.689    | 0.092    | 0.011    | 0.910       | 1.000       | 0.131       | 0.016       | 0.673        | 0.683        | 0.240        | 0.030        | 0.754     | 0.800     | 0.178     | 0.022     |
| turk3.maj.vote | 0.621      | 0.620      | 0.153      | 0.019      | 0.810       | 0.815       | 0.155       | 0.019       | 0.686    | 0.700    | 0.112    | 0.014    | 0.554       | 0.571       | 0.303       | 0.038       | 0.762        | 0.800        | 0.287        | 0.036        | 0.641     | 0.667     | 0.206     | 0.027     |
| turk5.all      | 0.917      | 0.918      | 0.079      | 0.010      | 0.550       | 0.545       | 0.114       | 0.014       | 0.678    | 0.674    | 0.091    | 0.011    | 0.962       | 1.000       | 0.096       | 0.012       | 0.643        | 0.667        | 0.239        | 0.029        | 0.753     | 0.800     | 0.186     | 0.023     |
| turk5.maj.vote | 0.595      | 0.571      | 0.140      | 0.017      | 0.874       | 0.913       | 0.139       | 0.017       | 0.695    | 0.698    | 0.110    | 0.014    | 0.425       | 0.375       | 0.270       | 0.034       | 0.819        | 1.000        | 0.237        | 0.031        | 0.558     | 0.500     | 0.202     | 0.026     |

Mean/median/sd accuracy by user group - 5 meter level:

| user.type      | all.rec.mn | all.rec.md | all.rec.sd | all.rec.se | all.prec.mn | all.prec.md | all.prec.sd | all.prec.se | all.f.mn | all.f.md | all.f.sd | all.f.se | prob.rec.mn | prob.rec.md | prob.rec.sd | prob.rec.se | prob.prec.mn | prob.prec.md | prob.prec.sd | prob.prec.se | prob.f.mn | prob.f.md | prob.f.sd | prob.f.se |
|:---------------|:-----------|:-----------|:-----------|:-----------|:------------|:------------|:------------|:------------|:---------|:---------|:---------|:---------|:------------|:------------|:------------|:------------|:-------------|:-------------|:-------------|:-------------|:----------|:----------|:----------|:----------|
| all            | 0.445      | 0.466      | 0.226      | 0.020      | 0.490       | 0.500       | 0.199       | 0.017       | 0.445    | 0.463    | 0.180    | 0.016    | 0.181       | 0.131       | 0.175       | 0.015       | 0.285        | 0.250        | 0.258        | 0.023        | 0.223     | 0.200     | 0.121     | 0.012     |
| anon           | 0.375      | 0.415      | 0.262      | 0.065      | 0.587       | 0.680       | 0.277       | 0.072       | 0.491    | 0.568    | 0.229    | 0.061    | 0.104       | 0.079       | 0.099       | 0.025       | 0.514        | 0.500        | 0.377        | 0.105        | 0.192     | 0.156     | 0.109     | 0.031     |
| reg            | 0.421      | 0.430      | 0.203      | 0.029      | 0.493       | 0.525       | 0.192       | 0.027       | 0.434    | 0.455    | 0.167    | 0.024    | 0.137       | 0.125       | 0.136       | 0.019       | 0.225        | 0.200        | 0.199        | 0.028        | 0.214     | 0.186     | 0.101     | 0.017     |
| turk1          | 0.481      | 0.490      | 0.231      | 0.028      | 0.466       | 0.489       | 0.180       | 0.022       | 0.443    | 0.471    | 0.179    | 0.022    | 0.232       | 0.200       | 0.200       | 0.025       | 0.285        | 0.272        | 0.248        | 0.031        | 0.235     | 0.211     | 0.135     | 0.018     |
| turk3.all      | 0.656      | 0.656      | 0.159      | 0.020      | 0.376       | 0.376       | 0.134       | 0.016       | 0.460    | 0.478    | 0.121    | 0.015    | 0.400       | 0.353       | 0.211       | 0.026       | 0.229        | 0.206        | 0.154        | 0.019        | 0.254     | 0.258     | 0.111     | 0.014     |
| turk3.maj.vote | 0.502      | 0.509      | 0.195      | 0.024      | 0.646       | 0.667       | 0.177       | 0.022       | 0.541    | 0.559    | 0.161    | 0.020    | 0.148       | 0.125       | 0.134       | 0.017       | 0.311        | 0.333        | 0.255        | 0.032        | 0.225     | 0.203     | 0.116     | 0.016     |
| turk5.all      | 0.697      | 0.701      | 0.141      | 0.017      | 0.300       | 0.292       | 0.114       | 0.014       | 0.405    | 0.405    | 0.118    | 0.014    | 0.500       | 0.500       | 0.197       | 0.024       | 0.207        | 0.176        | 0.135        | 0.017        | 0.267     | 0.242     | 0.122     | 0.015     |
| turk5.maj.vote | 0.507      | 0.504      | 0.172      | 0.021      | 0.714       | 0.756       | 0.176       | 0.022       | 0.575    | 0.584    | 0.151    | 0.019    | 0.108       | 0.091       | 0.106       | 0.013       | 0.423        | 0.333        | 0.333        | 0.043        | 0.205     | 0.167     | 0.115     | 0.017     |

#### Statistical significance

NOTE: This is at the street level (not 5 meter level) and only looks at the aggregation of all label types ("All").

We created binomial mixed effects models to determine the relationship between user group and recall/precision. We had user group as the fixed effect and route id as the random effect. We modeled recall/precision/raw accuracy as binomial and used a logistic link function. You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

##### Comparing anon reg and turk1

Using likelihood ratio tests (LRTs), we found the contribution of the fixed effect (worker type) to have a statistically significant association with recall (likelihood ratio = 21.616, df = 2, n = 132, p &lt; 0.001) and precision (likelihood ratio = 7.1219, df = 2, n = 131, p = 0.028), but we did *not* find a statistically significant difference for raw accuracy (likelihood ratio = 1.6217, df = 2, n = 132, p = 0.444).

To test that the orderings of the user groups are statistically significant (e.g., that turk1 recall is significantly higher than registered user recall, etc), we do post-hoc Tukey's HSD tests. This essentially gives us a pairwise test between each user group, which lets us determine what parts of the ordering are significant. The results of which are shown in the tables below.

NOTE: `*` means less than 0.05, `**` means less than 0.01, and `***` means less than 0.001

Recall: likelihood ratio = 21.616, df = 2, n = 132, p &lt; 0.001.

| worker.type | test       | p.value           | z.value | recall |
|:------------|:-----------|:------------------|:--------|:-------|
| turk1       | -          | -                 | -       | 0.678  |
| reg         | &lt; turk1 | 0.009 \*\*        | 2.853   | 0.614  |
| anon        | &lt; reg   | 0.020 \*          | 2.321   | 0.488  |
| anon        | &lt; turk1 | &lt; 0.001 \*\*\* | 3.976   | 0.488  |

Precision: likelihood ratio = 7.1219, df = 2, n = 131, p = 0.028

| worker.type | test      | p.value | z.value | precision |
|:------------|:----------|:--------|:--------|:----------|
| anon        | -         | -       | -       | 0.745     |
| reg         | &lt; anon | 0.270   | 1.103   | 0.722     |
| turk1       | &lt; reg  | 0.134   | 1.937   | 0.688     |
| turk1       | &lt; anon | 0.134   | 2.007   | 0.688     |

##### Comparing different turker groups

Using likelihood ratio tests (LRTs), we found the contribution of the fixed effect (worker type) to have a statistically significant association with recall (likelihood ratio = 498.96, df = 4, n = 330, p &lt; 0.001), precision (likelihood ratio = 374.88, df = 4, n = 330, p &lt; 0.001), and raw accuracy (likelihood ratio = 195.99, df = 4, n = 330, p &lt; 0.001).

To test that the orderings of the user groups are statistically significant (e.g., that turk5 majority vote precision is significantly higher than turk3 majority vote precision, etc), we do post-hoc Tukey's HSD tests. This essentially gives us a pairwise test between each user group, which lets us determine what parts of the ordering are significant. The results of which are shown in the tables below.

NOTE: `*` means less than 0.05, `**` means less than 0.01, and `***` means less than 0.001

Recall: likelihood ratio = 498.96, df = 4, n = 330, p &lt; 0.001.

| worker.type    | test                | p.value           | z.value | recall |
|:---------------|:--------------------|:------------------|:--------|:-------|
| turk5.all      | -                   | -                 | -       | 0.917  |
| turk3.all      | &lt; turk5.all      | &lt; 0.001 \*\*\* | 3.7244  | 0.867  |
| turk1          | &lt; turk3.all      | &lt; 0.001 \*\*\* | 10.0798 | 0.678  |
| turk1          | &lt; turk5.all      | &lt; 0.001 \*\*\* | 12.9464 | 0.678  |
| turk3.maj.vote | &lt; turk1          | 0.015 \*          | 2.6652  | 0.621  |
| turk3.maj.vote | &lt; turk3.all      | &lt; 0.001 \*\*\* | 12.4353 | 0.621  |
| turk3.maj.vote | &lt; turk5.all      | &lt; 0.001 \*\*\* | 15.0197 | 0.621  |
| turk5.maj.vote | &lt; turk3.maj.vote | 0.326             | 0.9828  | 0.595  |
| turk5.maj.vote | &lt; turk1          | &lt; 0.001 \*\*\* | 3.6420  | 0.595  |
| turk5.maj.vote | &lt; turk3.all      | &lt; 0.001 \*\*\* | 13.2806 | 0.595  |
| turk5.maj.vote | &lt; turk5.all      | &lt; 0.001 \*\*\* | 15.7607 | 0.595  |

Precision: likelihood ratio = 374.88, df = 4, n = 330, p &lt; 0.001

| worker.type    | test                | p.value           | z.value | precision |
|:---------------|:--------------------|:------------------|:--------|:----------|
| turk5.maj.vote | -                   | -                 | -       | 0.874     |
| turk3.maj.vote | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 3.704   | 0.810     |
| turk1          | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 5.872   | 0.688     |
| turk1          | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 9.019   | 0.688     |
| turk3.all      | &lt; turk1          | &lt; 0.001 \*\*\* | 4.341   | 0.601     |
| turk3.all      | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 9.897   | 0.601     |
| turk3.all      | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 12.466  | 0.601     |
| turk5.all      | &lt; turk3.all      | 0.004 \*\*        | 2.913   | 0.550     |
| turk5.all      | &lt; turk1          | &lt; 0.001 \*\*\* | 6.986   | 0.550     |
| turk5.all      | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 12.150  | 0.550     |
| turk5.all      | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 14.336  | 0.550     |

Raw accuracy: likelihood ratio = 195.99, df = 4, n = 330, p &lt; 0.001

| worker.type    | test                | p.value           | z.value | raw.accuracy |
|:---------------|:--------------------|:------------------|:--------|:-------------|
| turk5.maj.vote | -                   | -                 | -       | 0.777        |
| turk3.maj.vote | &lt; turk5.maj.vote | 0.087             | 1.713   | 0.759        |
| turk1          | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 4.145   | 0.712        |
| turk1          | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 5.839   | 0.712        |
| turk3.all      | &lt; turk1          | 0.021 \*          | 2.564   | 0.692        |
| turk3.all      | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 6.682   | 0.692        |
| turk3.all      | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 8.352   | 0.692        |
| turk5.all      | &lt; turk3.all      | &lt; 0.001 \*\*\* | 3.786   | 0.645        |
| turk5.all      | &lt; turk1          | &lt; 0.001 \*\*\* | 6.331   | 0.645        |
| turk5.all      | &lt; turk3.maj.vote | &lt; 0.001 \*\*\* | 10.377  | 0.645        |
| turk5.all      | &lt; turk5.maj.vote | &lt; 0.001 \*\*\* | 12.002  | 0.645        |

#### Old statistical significance section

NOTE: This is at the street level (not 5 meter level).

We created binomial mixed effects models to determine the relationship between user group and recall/precision. We had user group as the fixed effect and route id as the random effect. We modeled recall/precision as binomial and used a logistic link function. You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

Using likelihood ratio tests (LRTs), we found the contribution of the fixed effect (worker type) to have a statistically significant association with both recall and precision for both the Problem type and all label types aggregated (results shown below).

To test that the orderings of the user groups are statistically significant (e.g., that turk1 recall is significantly lower than registered user recall for the Problem type, etc), we do post-hoc Tukey's HSD tests. This essentially gives us a pairwise test between each user group, which lets us determine what parts of the ordering are significant. The results of which are shown in the tables below.

NOTE: `*` means less than 0.05, `**` means less than 0.01, and `***` means less than 0.001

Recall, all label types: likelihood ratio = 23.057, df = 4, n = 264, p &lt; 0.001.

| worker.type | test       | p.value    | z.value | recall |
|:------------|:-----------|:-----------|:--------|:-------|
| turk1       | -          | -          | -       | 0.678  |
| turk3       | &lt; turk1 | 0.056      | 2.6493  | 0.621  |
| reg         | &lt; turk3 | 1.000      | 0.5899  | 0.614  |
| reg         | &lt; turk1 | 0.017 \*   | 3.0746  | 0.614  |
| turk5       | &lt; reg   | 1.000      | 0.3255  | 0.595  |
| turk5       | &lt; turk3 | 0.986      | 0.9770  | 0.595  |
| turk5       | &lt; turk1 | 0.003 \*\* | 3.6205  | 0.595  |
| anon        | &lt; turk5 | 0.283      | 1.8226  | 0.488  |
| anon        | &lt; reg   | 0.283      | 1.9071  | 0.488  |
| anon        | &lt; turk3 | 0.119      | 2.3305  | 0.488  |
| anon        | &lt; turk1 | 0.002 \*\* | 3.7237  | 0.488  |

Precision, all label types: likelihood ratio = 108.36, df = 4, n = 263, p &lt; 0.001

| worker.type | test       | p.value           | z.value | precision |
|:------------|:-----------|:------------------|:--------|:----------|
| turk5       | -          | -                 | -       | 0.874     |
| turk3       | &lt; turk5 | 0.001 \*\*        | 3.7143  | 0.810     |
| anon        | &lt; turk3 | 0.612             | 0.7921  | 0.745     |
| anon        | &lt; turk5 | 0.029 \*          | 2.7554  | 0.745     |
| reg         | &lt; anon  | 0.612             | 1.0234  | 0.722     |
| reg         | &lt; turk3 | &lt; 0.001 \*\*\* | 3.8108  | 0.722     |
| reg         | &lt; turk5 | &lt; 0.001 \*\*\* | 7.0635  | 0.722     |
| turk1       | &lt; reg   | 0.253             | 1.7959  | 0.688     |
| turk1       | &lt; anon  | 0.253             | 1.8573  | 0.688     |
| turk1       | &lt; turk3 | &lt; 0.001 \*\*\* | 6.0213  | 0.688     |
| turk1       | &lt; turk5 | &lt; 0.001 \*\*\* | 9.1436  | 0.688     |

Recall, Problem type: likelihood ratio = 47.022, df = 4, n = 260, p &lt; 0.001

| worker.type | test       | p.value           | z.value | recall |
|:------------|:-----------|:------------------|:--------|:-------|
| turk1       | -          | -                 | -       | 0.658  |
| turk3       | &lt; reg   | 0.481             | 1.120   | 0.554  |
| turk3       | &lt; turk1 | 0.006 \*\*        | 3.355   | 0.554  |
| reg         | &lt; turk1 | 0.123             | 2.043   | 0.529  |
| turk5       | &lt; reg   | 0.002 \*\*        | 3.684   | 0.425  |
| turk5       | &lt; turk3 | 0.028 \*          | 2.768   | 0.425  |
| turk5       | &lt; turk1 | &lt; 0.001 \*\*\* | 6.025   | 0.425  |
| anon        | &lt; turk5 | 0.481             | 1.174   | 0.394  |
| anon        | &lt; reg   | 0.012 \*          | 3.082   | 0.394  |
| anon        | &lt; turk3 | 0.035 \*          | 2.619   | 0.394  |
| anon        | &lt; turk1 | &lt; 0.001 \*\*\* | 4.345   | 0.394  |

Precision, Problem type: likelihood ratio = 8.3102, df = 4, n = 252, p = 0.081

NOTE: Although anon user have a higher average problem type precision than turk5, the model actually says that turk5 has higher precision (though it is not statistically significant). This is because there is just higher precision across the board on the anon user routes; the mixed effects model takes this into account! More on this below.

| worker.type | test       | p.value | z.value | precision |
|:------------|:-----------|:--------|:--------|:----------|
| turk5       | -          | -       | -       | 0.819     |
| anon        | &lt; turk3 | 1.000   | 0.19702 | 0.879     |
| anon        | &lt; turk5 | 1.000   | 0.66753 | 0.879     |
| turk3       | &lt; turk5 | 1.000   | 0.98643 | 0.762     |
| turk1       | &lt; turk3 | 0.865   | 1.60644 | 0.708     |
| turk1       | &lt; anon  | 1.000   | 0.45078 | 0.708     |
| turk1       | &lt; turk5 | 0.143   | 2.45078 | 0.708     |
| reg         | &lt; turk1 | 1.000   | 0.07277 | 0.661     |
| reg         | &lt; turk3 | 0.897   | 1.52165 | 0.661     |
| reg         | &lt; anon  | 1.000   | 0.46253 | 0.661     |
| reg         | &lt; turk5 | 0.175   | 2.33597 | 0.661     |

One interesting thing I am seeing is anon users have a much higher average precision for the Problem type than other user groups, but the difference is not statistically significant. It turns out that on routes audited by anonymous users, turkers *also* had much higher Problem type precision than for registered user routes. This can be seen in the following table:

| worker.type | problem.precision.on.anon.routes |
|:------------|:---------------------------------|
| anon        | 0.879                            |
| turk1       | 0.862                            |
| turk3       | 0.894                            |
| turk5       | 0.922                            |

### Accuracy by label type

NOTE: In the two tables below, the data are binary (not ordinal), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

#### Summary stats

Mean/median/sd accuracy by label type - street level:

| label.type  | f\_md | f\_mn | f\_sd | prec\_md | prec\_mn | prec\_sd | recall\_md | recall\_mn | recall\_sd |
|:------------|:------|:------|:------|:---------|:---------|:---------|:-----------|:-----------|:-----------|
| All         | 0.667 | 0.647 | 0.149 | 0.714    | 0.707    | 0.172    | 0.651      | 0.631      | 0.216      |
| Problem     | 0.600 | 0.602 | 0.231 | 0.750    | 0.707    | 0.286    | 0.600      | 0.577      | 0.312      |
| CurbRamp    | 0.952 | 0.892 | 0.177 | 1.000    | 0.954    | 0.075    | 1.000      | 0.860      | 0.257      |
| NoCurbRamp  | 0.500 | 0.587 | 0.256 | 0.000    | 0.205    | 0.317    | 1.000      | 0.693      | 0.435      |
| Obstacle    | 0.500 | 0.564 | 0.190 | 0.500    | 0.475    | 0.374    | 0.450      | 0.399      | 0.369      |
| SurfaceProb | 0.500 | 0.531 | 0.209 | 1.000    | 0.726    | 0.354    | 0.200      | 0.271      | 0.305      |
| NoSidewalk  | 0.667 | 0.729 | 0.198 | 1.000    | 0.739    | 0.356    | 0.500      | 0.474      | 0.412      |

Mean/median/sd accuracy by label type - 5 meter level:

| label.type  | f\_md | f\_mn | f\_sd | prec\_md | prec\_mn | prec\_sd | recall\_md | recall\_mn | recall\_sd |
|:------------|:------|:------|:------|:---------|:---------|:---------|:-----------|:-----------|:-----------|
| All         | 0.463 | 0.445 | 0.180 | 0.500    | 0.490    | 0.199    | 0.466      | 0.445      | 0.226      |
| Problem     | 0.200 | 0.223 | 0.121 | 0.250    | 0.285    | 0.258    | 0.131      | 0.181      | 0.175      |
| CurbRamp    | 0.667 | 0.627 | 0.204 | 0.684    | 0.646    | 0.218    | 0.722      | 0.645      | 0.272      |
| NoCurbRamp  | 0.400 | 0.463 | 0.288 | 0.000    | 0.135    | 0.271    | 0.500      | 0.498      | 0.441      |
| Obstacle    | 0.250 | 0.275 | 0.131 | 0.091    | 0.182    | 0.235    | 0.000      | 0.161      | 0.228      |
| SurfaceProb | 0.182 | 0.258 | 0.210 | 0.250    | 0.342    | 0.354    | 0.000      | 0.096      | 0.193      |

#### Statistical significance

NOTE: This is at the street level (not 5 meter level).

We created binomial mixed effects models to determine the relationship between label type and recall/precision. We had label type as the fixed effect and user id nested in route id as random effects. We modeled recall/precision as binomial and used a logistic link function. You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

Using likelihood ratio tests (LRTs), we found the contribution of the fixed effect (label type) to have a statistically significant association with recall (likelihood ratio = 750.31, df = 3, n = 436, p &lt; 0.001), precision (likelihood ratio = 874.12, df = 3, n = 423, p &lt; 0.001), and raw accuracy (likelihood ratio = 160.47, df = 3, n = 528, p &lt; 0.001.

To test that the ordering of the label types are statistically significant (e.g., that NoCurbRamp recall is significantly lower than CurbRamp recall, etc), we do post-hoc Tukey's HSD tests. This essentially gives us a pairwise test between each label type, which lets us determine what parts of the ordering are significant. The results of which are shown in a tables below (first recall, then precision).

NOTE: `*` means less than 0.05, `**` means less than 0.01, and `***` means less than 0.001

Recall: likelihood ratio = 750.31, df = 3, n = 436, p &lt; 0.001

| label.type  | test            | p.value           | z.value | recall |
|:------------|:----------------|:------------------|:--------|:-------|
| CurbRamp    | -               | -                 | -       | 0.860  |
| NoCurbRamp  | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 4.171   | 0.693  |
| Obstacle    | &lt; NoCurbRamp | &lt; 0.001 \*\*\* | 5.419   | 0.399  |
| Obstacle    | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 15.006  | 0.399  |
| SurfaceProb | &lt; Obstacle   | &lt; 0.001 \*\*\* | 5.234   | 0.271  |
| SurfaceProb | &lt; NoCurbRamp | &lt; 0.001 \*\*\* | 8.793   | 0.271  |
| SurfaceProb | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 21.014  | 0.271  |

Precision: likelihood ratio = 874.12, df = 3, n = 423, p &lt; 0.001

| label.type  | test             | p.value           | z.value | precision |
|:------------|:-----------------|:------------------|:--------|:----------|
| CurbRamp    | -                | -                 | -       | 0.954     |
| SurfaceProb | &lt; CurbRamp    | &lt; 0.001 \*\*\* | 10.621  | 0.726     |
| Obstacle    | &lt; SurfaceProb | &lt; 0.001 \*\*\* | 4.615   | 0.475     |
| Obstacle    | &lt; CurbRamp    | &lt; 0.001 \*\*\* | 16.150  | 0.475     |
| NoCurbRamp  | &lt; Obstacle    | &lt; 0.001 \*\*\* | 7.116   | 0.205     |
| NoCurbRamp  | &lt; SurfaceProb | &lt; 0.001 \*\*\* | 10.638  | 0.205     |
| NoCurbRamp  | &lt; CurbRamp    | &lt; 0.001 \*\*\* | 21.253  | 0.205     |

Raw accuracy: likelihood ratio = 160.47, df = 3, n = 528, p &lt; 0.001

| label.type  | test            | p.value           | z.value | raw.accuracy |
|:------------|:----------------|:------------------|:--------|:-------------|
| CurbRamp    | -               | -                 | -       | 0.837        |
| NoCurbRamp  | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 6.1894  | 0.729        |
| Obstacle    | &lt; NoCurbRamp | 0.641             | 0.4668  | 0.712        |
| Obstacle    | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 6.6374  | 0.712        |
| SurfaceProb | &lt; Obstacle   | &lt; 0.001 \*\*\* | 6.0165  | 0.590        |
| SurfaceProb | &lt; NoCurbRamp | &lt; 0.001 \*\*\* | 6.4709  | 0.590        |
| SurfaceProb | &lt; CurbRamp   | &lt; 0.001 \*\*\* | 12.2482 | 0.590        |

### Visual search time: Time to label by type

NOTE: In this section, the data are binary (not ordinal), and is at the street level (not 5 meter level), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

NOTE: We believe that a user's "typical" visual search time is more accurately represented by their median visual search time than mean due to the long right tail of the distribution of search times.

Below is a table that shows the average time to place a label by label type along with the average recall and precision. The results match my intuition for the most part: CurbRamp has the shortest labeling time, SurfaceProblem has the longest labeling time, and NoCurbRamp and Obstacle are somewhere in between. However, I do find it a bit surprising that NoCurbRamp took longer to label than Obstacle.

Time to place a label is defined as follows:

-   For the first label a user places on a specific panorama, the time that elapsed between stepping into the panorama and placing the label.
-   For subsequent labels on the same panorama, the time that elapsed between placing the previous label and placing the current label.

| label.type  | median.s.to.label | mean.sec.to.label | sd.s.to.label | mean\_recall | mean\_precision |
|:------------|:------------------|:------------------|:--------------|:-------------|:----------------|
| CurbRamp    | 6.89              | 8.14              | 5.29          | 0.86         | 0.95            |
| Obstacle    | 8.21              | 10.10             | 7.54          | 0.40         | 0.47            |
| NoCurbRamp  | 9.46              | 12.78             | 10.20         | 0.69         | 0.20            |
| SurfaceProb | 10.92             | 13.77             | 8.41          | 0.27         | 0.73            |

We created a linear mixed effects models to determine the relationship between label type and visual search time. We had label type as the fixed effect and user id nested in route id as random effects. We modeled the log of the visual search time as linear and used a logistic link function. You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

Quick aside to discuss why we are using log-transformed visual search time: Our two assumptions are normality and heteroscedasticity of the residuals of the fit. Using the Shapiro-Wilk test, we reject the null (p &lt; 0.001), meaning we have not proven that the residuals are normally distributed. The histogram on the left of the residuals looks relatively normal, but there are some outliers, and it has a long-ish right tail. More importantly, we really do not seem to meet the heteroscedasticity assumption, given how the variance seems much larger for longer labeling times in the qq plot on the right.

![](stats_for_paper_files/figure-markdown_github/turk.visual.search.time.lme.assumption.2-1.png)![](stats_for_paper_files/figure-markdown_github/turk.visual.search.time.lme.assumption.2-2.png)

To try and deal with not meeting the normality or heteroscedasticity assumptions, we perform a log transform on our outcome variable (visual search time). After the transformation, we still fail the Shapiro-Wilk test (p &lt; 0.001), but the histogram on the left looks clearly normal, and the residuals on the right seem to have nearly constant variance. Thus, we meet the assumptions necessary to use this model after the transformation.

![](stats_for_paper_files/figure-markdown_github/turk.visual.search.time.lme.test.1-1.png)![](stats_for_paper_files/figure-markdown_github/turk.visual.search.time.lme.test.1-2.png)

Using a likelihood ratio test, we find the contribution by the fixed effect (label type) to be statistically significant (likelihood ratio = 78.871, df = 3, n = 434, p &lt; 0.001), i.e., the association between label type and labeling time is statistically significant. To test that the ordering of the label types are statistically significant (e.g., that CurbRamp labeling time is significantly lower than Obstacle labeling time, etc), we do a post-hoc Tukey's HSD test. This essentially gives us a pairwise test between each label type, which lets us determine what parts of the ordering are significant. The results of which are shown in a table below.

NOTE: `*` means less than 0.05, `**` means less than 0.01, and `***` means less than 0.001

| label.type  | test            | p.value           | z.value | seconds.to.label |
|:------------|:----------------|:------------------|:--------|:-----------------|
| CurbRamp    | -               | -                 | -       | 8.139            |
| Obstacle    | &gt; CurbRamp   | &lt; 0.001 \*\*\* | 3.862   | 10.102           |
| NoCurbRamp  | &gt; Obstacle   | 0.007 \*\*        | 2.935   | 12.784           |
| NoCurbRamp  | &gt; CurbRamp   | &lt; 0.001 \*\*\* | 7.247   | 12.784           |
| SurfaceProb | &gt; NoCurbRamp | 0.048 \*          | 1.981   | 13.766           |
| SurfaceProb | &gt; Obstacle   | &lt; 0.001 \*\*\* | 4.577   | 13.766           |
| SurfaceProb | &gt; CurbRamp   | &lt; 0.001 \*\*\* | 8.458   | 13.766           |

### Zone type: Land use effect on accuracy

#### Definitions

First, we have our definitions of the different zone types with shortened names that we use in parens (these can also be found in doc/zone\_type\_descriptions.txt):

-   Downtown: High-density commercial and residential development
-   Mixed-Use: Moderate-density residential and non-residential buildings (e.g., retail, art use)
-   Neighborhood Mixed-Use (Nbhd Mixed-Use): Low-density mixed-use development with an emphasis on residential
-   Production, distribution, and repair (Industrial): Moderate-density commercial and production, distribution, and repair (with heavy machinery)
-   Residential: Predominantly residential with detached houses on medium-to-large lots
-   Residential Apartment (Residential Apt): Medium-to-high density residential including apartments and some rowhouses
-   Residential Flat: Low-to-moderate density residential rowhouses
-   Special Purpose: Includes Fort McNair Naval Facility (and nearby high-density residential), some moderate-density residential, and some undeveloped land.
-   Unzoned: Predominantly wooded areas (e.g., large parks, golf courses, and cemeteries) and a military base.

NOTE: For the Special Purpose zone, there are no public roads in the Naval Facility and very few roads in the undeveloped land. Thus, the majority of the *roads* with the Special Purpose zone tag are in high-density residential areas, particularly apartments.

However, we are going to look at two different groupings of these zone types, because a lot of these zones have very little data. The first grouping is done semantically, and the second is done based on density of development.

-   Semantic grouping
    -   Commercial = Downtown + Industrial
    -   Mixed-Use = Mixed-Use + Neighborhood Mixed-Use
    -   Residential Apt = Residential Apt + Special Purpose
    -   Residential House = Residential + Residential Flat
    -   Unzoned = Unzoned
-   Density grouping
    -   Low to Moderate Density = Neighborhood Mixed-Use + Residential + Residential Flat + Unzoned
    -   Medium to High Density = Downtown + Mixed-Use + Industrial + Residential Apt + Special Purpose

Here are the distribution of the streets across DC, registered user routes, and anonymous user routes for zone types and quadrant.

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.bar.graph.1-1.png) ![](stats_for_paper_files/figure-markdown_github/turk.zone.type.bar.graph.2-1.png)

#### Semantic grouping

As a reminder, the grouping is defined as follows:

-   Commercial = Downtown + Industrial
-   Mixed-Use = Mixed-Use + Neighborhood Mixed-Use
-   Residential Apt = Residential Apt + Special Purpose
-   Residential House = Residential + Residential Flat
-   Unzoned = Unzoned

##### Distribution

Here is the zone type distribution for the mturk study. We assigned each street in DC a zone type based on the zone in which one of its endpoints is located. We then assigned a zone type to each route as the plurality zone type among the streets on that route. The graph below compares the zone type distributions of the anon user routes, registered user routes, and all of DC for reference.

NOTE: For registered and anon user routes below, it is the percentage of *routes* marked as that zone type. But for All DC Streets, it is the percentage of *streets* in DC, since we don't have a set of "routes" that makes up DC.

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.distribution.semantic-1.png)

| zone.type         | n.routes | n.neighborhoods |
|:------------------|:---------|:----------------|
| Commercial        | 2        | 2               |
| Mixed-Use         | 8        | 8               |
| Residential Apt   | 11       | 10              |
| Residential House | 43       | 40              |
| Unzoned           | 2        | 2               |

Below, we look at the distribution of the label type densities in the ground truth, by zone type. Commercial seems to be the only zone that has a noticeably different distribution of label types. However, we have only two routes for that zone type so we can't draw anything of statistical significance from that.

![](stats_for_paper_files/figure-markdown_github/gt.label.dist.zone.plot.semantic-1.png)

##### Relationship with accuracy

The first graph shows all label types aggregated, the second shows the problem vs. no problem type.

NOTE: In this section, the data are binary (not ordinal), and is at the street level (not 5 meter level), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

NOTE: The red dots on the graphs are means.

NOTE: N in the graphs below means number of routes that are predominantly of that zone type. However, there are 3 users who completed each route, so there are actually n \* 3 data points.

The only noticeable difference I see right away is particularly high recall and low precision for Unzoned relative to the other zones. However, there were only 2 routes classified as Unzoned, so we don't have enough data to make any judgements on that. The 3 zone types with significant data (Mixed-Use, Residential Apt, and Residential House) all seem to have roughly equal accuracies.

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis.semantic-1.png)![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis.semantic-2.png)

#### Density grouping

As a reminder, the grouping is defined as follows:

-   Low to Moderate Density = Neighborhood Mixed-Use + Residential + Residential Flat + Unzoned
-   Medium to High Density = Downtown + Mixed-Use + Industrial + Residential Apt + Special Purpose

##### Distribution

Here is the zone type distribution for the mturk study. We assigned each street in DC a zone type based on the zone in which one of its endpoints is located. We then assigned a zone type to each route as the plurality zone type among the streets on that route. The graph below compares the zone type distributions of the anon user routes, registered user routes, and all of DC for reference.

NOTE: For registered and anon user routes below, it is the percentage of *routes* marked as that zone type. But for All DC Streets, it is the percentage of *streets* in DC, since we don't have a set of "routes" that makes up DC.

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.distribution.density-1.png)

| zone.type            | n.routes | n.neighborhoods |
|:---------------------|:---------|:----------------|
| Medium-High Density  | 23       | 22              |
| Low-Moderate Density | 43       | 40              |

Since there are many residential routes in each group, the number of residential routes is of interest. Of the 43 low-moderate density routes, 41 were residential (95.35%). And of the 23 medium-high density routes, 13 were residential (56.52%).

Below, we look at the distribution of the label type densities in the ground truth, by zone type. The types of density appear to have similar distributions of label types.

![](stats_for_paper_files/figure-markdown_github/gt.label.dist.zone.plot.density-1.png)

##### Relationship with accuracy

We first show a table with the mean/median/sd for accuracy in the two zones. This is followed by a pair of graphs where the first graph shows all label types aggregated, the second shows the problem vs. no problem type.

NOTE: In this section, the data are binary (not ordinal), and is at the street level (not 5 meter level), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

NOTE: The red dots on the graphs are means.

NOTE: N in the graphs below means number of routes that are predominantly of that zone type. However, there are 3 users who completed each route, so there are actually n \* 3 data points.

There does not appear to be a significant difference in accuracy between the densities.

| accuracy.type | label.type | zone.type            | mean.accuracy | median.accuracy | sd    |
|:--------------|:-----------|:---------------------|:--------------|:----------------|:------|
| recall        | All        | Medium-High Density  | 0.660         | 0.675           | 0.189 |
| recall        | All        | Low-Moderate Density | 0.615         | 0.636           | 0.228 |
| recall        | Problem    | Medium-High Density  | 0.662         | 0.683           | 0.287 |
| recall        | Problem    | Low-Moderate Density | 0.530         | 0.500           | 0.317 |
| precision     | All        | Medium-High Density  | 0.679         | 0.667           | 0.143 |
| precision     | All        | Low-Moderate Density | 0.723         | 0.750           | 0.185 |
| precision     | Problem    | Medium-High Density  | 0.719         | 0.750           | 0.276 |
| precision     | Problem    | Low-Moderate Density | 0.700         | 0.750           | 0.293 |

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis.density-1.png)![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis.density-2.png)

### User behavior: Does auditing speed, etc influence accuracy

Variables being investigated: labeling frequency, auditing speed, and visual search time association with recall and precision. I'm also taking a look at both the All and Problem (vs. NoProblem) label types; we had been planning to only look at the All type, but it was easy enough for me to add both, and we can see if there is anything interesting there.

NOTE: In this section, the data are binary (not ordinal), at the street level granularity (not 5 meter level) we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

First, let's take a look at the relationships between the variables.

![](stats_for_paper_files/figure-markdown_github/turk.user.behavior.graphs-1.png)![](stats_for_paper_files/figure-markdown_github/turk.user.behavior.graphs-2.png)![](stats_for_paper_files/figure-markdown_github/turk.user.behavior.graphs-3.png)

From these graphs, the potential associations I was seeing (before running the tests) were a possible positive association between labeling frequency and recall (both Problem and All types) and a possible negative association between auditing speed and recall (both Problem and All types). In fact, three of those four are cases where we find statistically significant results.

To test for the associations between the user behaviors and accuracy, we created 4 binomial mixed effect models (one for accuracy type, precision and recall; and label type, All and Problem). We had the 3 user behaviors as individual fixed effects (labeling frequency, audit speed, and visual search time), which we scaled and centered so that estimates and standard errors between the predictors is easier. We used user id nested in route id as the random effects. We modeled recall and precision as binomial and used the standard logistic link function. We performed likelihood ratio tests (LRTs) to determine the significance of the predictors. You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

Below is a table showing the summaries of the models and results of the LRTs. The estimate and standard error columns come from the models (along with the association column, which denotes direction of relationship), and the p value and LRT stat come from the likelihood ratio tests.

| accuracy.type | label.type | param           | association | estimate | std.err | p.value           | LRT    | df  | n   |
|:--------------|:-----------|:----------------|:------------|:---------|:--------|:------------------|:-------|:----|:----|
| recall        | All        | label.freq      | +           | 0.403    | 0.098   | &lt; 0.001 \*\*\* | 17.030 | 1   | 132 |
| recall        | All        | audit.speed     | -           | -0.272   | 0.091   | 0.002 \*\*        | 9.222  | 1   | 132 |
| recall        | All        | viz.search.time | -           | -0.346   | 0.116   | 0.002 \*\*        | 9.505  | 1   | 132 |
| recall        | Problem    | label.freq      | +           | 0.601    | 0.141   | &lt; 0.001 \*\*\* | 18.433 | 1   | 127 |
| recall        | Problem    | audit.speed     | NA          | -0.162   | 0.131   | 0.209             | 1.576  | 1   | 127 |
| recall        | Problem    | viz.search.time | NA          | -0.129   | 0.116   | 0.261             | 1.265  | 1   | 127 |
| precision     | All        | label.freq      | -           | -0.233   | 0.077   | 0.003 \*\*        | 8.545  | 1   | 131 |
| precision     | All        | audit.speed     | NA          | 0.014    | 0.095   | 0.882             | 0.022  | 1   | 131 |
| precision     | All        | viz.search.time | -           | -0.258   | 0.113   | 0.024 \*          | 5.122  | 1   | 131 |
| precision     | Problem    | label.freq      | NA          | 0.174    | 0.163   | 0.282             | 1.159  | 1   | 128 |
| precision     | Problem    | audit.speed     | +           | 0.417    | 0.205   | 0.034 \*          | 4.473  | 1   | 128 |
| precision     | Problem    | viz.search.time | NA          | 0.316    | 0.180   | 0.071             | 3.266  | 1   | 128 |

The positive association between labeling frequency and recall is expected, as someone who placed more labels probably correctly found more attributes. Similarly the negative association between labeling frequency and All type precision is expected (more labels means more incorrect labels).

The negative association between auditing speed and All type recall is also expected, as auditing more quickly will probably end up with fewer correct attributes being found. Similarly, the negative association b/w auditing speed and Problem type precision might make sense (auditing more quickly, less likely to over-label).

The associations that we do not have immediate explanations for are the negative association b/w visual search time and All type recall and the negative association b/w visual search time and Problem type precision.

### User group: Reg vs anon vs turk1 vs turk3 vs turk5

TODO: Make some graphs.

*Takeaways*:

### Low severity: Removing low severity effect on recall

NOTE: I did this analysis using both &gt;=3 and &gt;=4, and both produced significant results. The difference between low and high severity is larger for &gt;=3 compared to &gt;=4, and thus the p-value is smaller. However, we could use &gt;=4 if the story is more compelling.

NOTE: In this section, the data are binary (not ordinal), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

Below is a table showing the average recall across all users for labels that had severity &lt;=3 (in the ground truth) and labels that had severity &gt;=4, along with the number of labels that fall into each of those categories.

We also created a binomial mixed effects model to determine the relationship between severity and recall. We had severity (high or low) as the fixed effect and user id nested in route id as random effects. We modeled recall as binomial and used a logistic link function. Using a likelihood ratio test (LRT), we found the contribution of the fixed effect (severity) to be statistically significant (street-level: likelihood ratio = 9.3761, df = 1, n = 214, p = 0.002; 5 meter-level: likelihood ratio = 29.76, df = 1, n = 214, p &lt; 0.001). You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

<table style="width:100%;">
<colgroup>
<col width="5%" />
<col width="14%" />
<col width="9%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">sev</th>
<th align="left">gt.prob.labs</th>
<th align="left">n.users</th>
<th align="left">mn.rec.st</th>
<th align="left">md.rec.st</th>
<th align="left">sd.rec.st</th>
<th align="left">mn.rec.5m</th>
<th align="left">md.rec.5m</th>
<th align="left">sd.rec.5m</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">all</td>
<td align="left">1405</td>
<td align="left">130</td>
<td align="left">0.577</td>
<td align="left">0.6</td>
<td align="left">0.312</td>
<td align="left">0.181</td>
<td align="left">0.131</td>
<td align="left">0.175</td>
</tr>
<tr class="even">
<td align="left">&lt;=3</td>
<td align="left">1247</td>
<td align="left">130</td>
<td align="left">0.572</td>
<td align="left">0.6</td>
<td align="left">0.321</td>
<td align="left">0.167</td>
<td align="left">0.125</td>
<td align="left">0.178</td>
</tr>
<tr class="odd">
<td align="left">&gt;=4</td>
<td align="left">158</td>
<td align="left">84</td>
<td align="left">0.675</td>
<td align="left">1.0</td>
<td align="left">0.397</td>
<td align="left">0.305</td>
<td align="left">0.196</td>
<td align="left">0.364</td>
</tr>
</tbody>
</table>

Below is a table showing the average recall across all users for labels that had severity &lt;=2 (in the ground truth) and labels that had severity &gt;=3, along with the number of labels that fall into each of those categories.

We also created binomial mixed effects models to determine the relationship between severity and recall. We had severity (high or low) as the fixed effect and user id nested in route id as random effects. We modeled recall as binomial and used a logistic link function. Using a likelihood ratio test (LRT), we found the contribution of the fixed effect (severity) to be statistically significant (street-level: likelihood ratio = 10.596, df = 1, n = 246, p = 0.001; 5 meter-level: likelihood ratio = 41.89, df = 1, n = 246, p &lt; 0.001). You can check out some notes on mixed effects models at the end of this document: [here](#rationale-and-description-of-mixed-effect-models).

<table style="width:100%;">
<colgroup>
<col width="5%" />
<col width="14%" />
<col width="9%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
<col width="11%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">sev</th>
<th align="left">gt.prob.labs</th>
<th align="left">n.users</th>
<th align="left">mn.rec.st</th>
<th align="left">md.rec.st</th>
<th align="left">sd.rec.st</th>
<th align="left">mn.rec.5m</th>
<th align="left">md.rec.5m</th>
<th align="left">sd.rec.5m</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">all</td>
<td align="left">1405</td>
<td align="left">130</td>
<td align="left">0.577</td>
<td align="left">0.600</td>
<td align="left">0.312</td>
<td align="left">0.181</td>
<td align="left">0.131</td>
<td align="left">0.175</td>
</tr>
<tr class="even">
<td align="left">&lt;=2</td>
<td align="left">1053</td>
<td align="left">130</td>
<td align="left">0.570</td>
<td align="left">0.563</td>
<td align="left">0.323</td>
<td align="left">0.158</td>
<td align="left">0.120</td>
<td align="left">0.180</td>
</tr>
<tr class="odd">
<td align="left">&gt;=3</td>
<td align="left">352</td>
<td align="left">116</td>
<td align="left">0.698</td>
<td align="left">0.833</td>
<td align="left">0.355</td>
<td align="left">0.290</td>
<td align="left">0.199</td>
<td align="left">0.322</td>
</tr>
</tbody>
</table>

### Binary vs ordinal issues per segment

For simplicity, the first graph looks at the 5 meter level, and the second looks at street level. All user groups are also combined (the groups being: registered volunteers, anonymous volunteers, and individual turkers).

NOTE: In this section, the data is at the street level (not 5 meter level), we are only considering single users auditing (i.e., no multi-user clustering or majority vote), and we only consider the first turker to audit each route.

NOTE: The red dots on the graphs are means.

*Takeaways*:

-   5 meter level (first graph): Considering multiple issues per segment results in *very slightly* lower accuracy for pretty much every type of label and type of accuracy (except precision). I suspect that this comes mostly from our method of clustering, which makes it unlikely that users end up with multiple labels per 5 meter segment. We do not have this restriction in the ground truth, so those few cases where we have more than one label per 5 meter segment in the GT usually results in an additional false negative when moving to ordinal analysis. However, the difference here is very small, so our clustering method seems fine to me.

-   Street level (second graph) recall: If we do this analysis at the street level, the decreases in accuracy are more pronounced. At this level, the clustering shouldn't have much effect. The decrease in recall suggests that users are finding *some* of the problems, but not *all* of them (meaning an increase in false negatives when we move to ordinal analysis).

-   Street level (second graph) recall: I suspect that the reason for the decrease in precision when moving to ordinal analysis at the street level is the same reason as why 5 meter level has lower precision than street level (seen in the previous section). That is, users' misunderstandings of how to label certain common things (driveways as curb ramps, etc.); since these mistakes are common, they may happen many times on a single street edge, which means that you start racking up the false positives when you move to ordinal analysis.

![](stats_for_paper_files/figure-markdown_github/turk.issues.per.seg.analysis-1.png)![](stats_for_paper_files/figure-markdown_github/turk.issues.per.seg.analysis-2.png)

Rationale and description of mixed effect models
------------------------------------------------

For some background on linear mixed-effect models, the reference I found most helpful was this one: <https://stats.idre.ucla.edu/other/mult-pkg/introduction-to-linear-mixed-models/>.

#### Why we are using them

A common question we are asking throughout this document is if some categorical variable (label type, user group, etc) has an effect on accuracy (or some other numeric variable, like visual search time), and if so, which categories have higher/lower accuracies than the others. Note that these are two different questions: is there a statistically significant effect overall, and what parts of the ordering of the categories is statistically significant.

The default analysis we would do in this situation is to do an ANOVA to test the overall significance, followed by Tukey's HSD post-hoc analysis to see if ordering is significant. Although we do fail most of the assumptions of these tests, that is not the main reason that we look to mixed effect models. We fail in different ways in different situations, so describing the failing of the assumptions is something that I will omit here.

The main reason that we want to use mixed effect models is that it allows us to take into account differences between users, the differences between routes, and the fact that the individual only audited a single route. Which route we are looking at has a significant influence on the accuracy of the users who audited that route, but the route ID is not a variable that is interesting to look at in relation to accuracy (the *type* of route is interesting to look at, but not the route ID). As such, we need a way to account for the route ID in our analyses.

A similar problem exists for individual users. When we are looking at how accuracy is different for different label types, each user has an accuracy number for each label type. But users have different "baseline" accuracy levels, similar to how routes have "baseline" accuracy levels. We would like to account for those baseline levels in our analyses, and only look at how label type, for example, causes a *change from the baseline*.

#### Example text

We created a &lt;linear|binomial|poisson - depending on outcome variable&gt; mixed effects models to determine the relationship between &lt;independent variable&gt; and &lt;outcome variable&gt;. We had &lt;independent variable&gt; as the fixed effect and user id nested in route id as random effects. We modeled &lt;outcome variable&gt; as &lt;linear|binomial|poisson&gt; and used a logistic link function.

Using a likelihood ratio test (LRT), we found the contribution of the fixed effect (&lt;independent variable&gt;) to have a statistically significant association with &lt;outcome variable&gt; (likelihood ratio = A, df = B, n = C, p &lt; 0.001).

To test that the ordering of the &lt;categorical independent variable&gt; is statistically significant (e.g., that NoCurbRamp recall is significantly lower than CurbRamp recall, etc), we do post-hoc Tukey's HSD tests. This essentially gives us a pairwise test between each &lt;categorical independent variable&gt;, which lets us determine what parts of the ordering are significant. The results of which are shown in a table below.

#### Assumptions

We won't need to go into the assumptions in the paper (people don't generally go into the assumptions for ANOVA, and we are already doing something better than that). But for those who are wondering, here is some text on the assumptions.

Our two assumptions are that the residuals of the fit are normally distributed (normality) and have constant variance across the range of fitted values (heteroscedasticity). To check for normality, we first use the Shapiro-Wilk test. In this test, the null hypothesis is that the residuals are normally distributed; if we fail to reject the null, then we meet our assumption that the residuals are normally distributed. This test has a high type 1 error rate (often says that data are not normally distributed when they really are), but it is good to check the test because it is a quick and easy way to say the data are normal if the test succeeds. If we fail the test, we check a histogram of the residuals to see if they are normally distributed. For heteroscedasticity, we make a scatter plot with the standardized residuals on the y-axis, and fitted visual search times on the x-axis. If the variance in the residuals (y-axis) is constant across the fitted values (x-axis), this constitutes "constant variance", and so we would meet the heteroscedasticity assumption.

Often we can deal with not meeting these assumptions by transforming our data. In fact, I think in at one of our analyses we do a log-transform which helps with failing to meet the heteroscedasticity assumption.
