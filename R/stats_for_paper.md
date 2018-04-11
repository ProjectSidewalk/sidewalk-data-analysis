Statistics for Paper
================
Mikey Saugstad
April 6, 2018

-   [Public Deployment](#public-deployment)
    -   [High-level results](#high-level-results)
        -   [Top-line numbers (no filtering)](#top-line-numbers-no-filtering)
        -   [Data characteristics](#data-characteristics)
        -   [User stats and tool usage](#user-stats-and-tool-usage)
    -   [Possible Stories](#possible-stories)
        -   [Data overlap and agreement between users](#data-overlap-and-agreement-between-users)
        -   [Stickyness of tool: user dropoffs](#stickyness-of-tool-user-dropoffs)
-   [Turk Study](#turk-study)
    -   [High level results](#high-level-results-1)
    -   [Possible Stories](#possible-stories-1)
        -   [Street-level vs 5 meter-level](#street-level-vs-5-meter-level)
        -   [Zone type (land use) effect on accuracy](#zone-type-land-use-effect-on-accuracy)
        -   [Reg vs anon vs turker vs turk3 vs turk5](#reg-vs-anon-vs-turker-vs-turk3-vs-turk5)
        -   [Does removing low severity brings higher recall](#does-removing-low-severity-brings-higher-recall)
        -   [Binary vs ordinal issues per segment](#binary-vs-ordinal-issues-per-segment)

Public Deployment
=================

NOTE: Public deployment data includes all data up through March 31st (and part of April 1st). This includes all data through the most recent deployment on mturk.

High-level results
------------------

### Top-line numbers (no filtering)

TODO: Decide how to cluster production labels

The following are the label counts (not issue counts) by user group and label type.

| label\_type    | Anon         | Registered    | Turker         | Researcher    | Total           |
|:---------------|:-------------|:--------------|:---------------|:--------------|:----------------|
| CurbRamp       | 16557 (6.6%) | 28085 (11.2%) | 89766 (35.7%)  | 18686 (7.4%)  | 153094 (60.8%)  |
| NoCurbRamp     | 2510 (1.0%)  | 3447 (1.4%)   | 13509 (5.4%)   | 1198 (0.5%)   | 20664 (8.2%)    |
| Obstacle       | 1139 (0.5%)  | 2829 (1.1%)   | 16154 (6.4%)   | 1517 (0.6%)   | 21639 (8.6%)    |
| SurfaceProblem | 804 (0.3%)   | 1898 (0.8%)   | 3216 (1.3%)    | 2603 (1.0%)   | 8521 (3.4%)     |
| NoSidewalk     | 2370 (0.9%)  | 6357 (2.5%)   | 28408 (11.3%)  | 8031 (3.2%)   | 45166 (17.9%)   |
| Occlusion      | 69 (0.0%)    | 310 (0.1%)    | 462 (0.2%)     | 453 (0.2%)    | 1294 (0.5%)     |
| Other          | 93 (0.0%)    | 148 (0.1%)    | 1137 (0.5%)    | 39 (0.0%)     | 1417 (0.6%)     |
| Total          | 23542 (9.3%) | 43074 (17.1%) | 152652 (60.6%) | 32527 (12.9%) | 251795 (100.0%) |

### Data characteristics

This is the start of filtering out users with low labeling frequency (also filtering out researchers).

| CurbRamp | NoCurbRamp | NoSidewalk | Obstacle | Occlusion | Other | SurfaceProblem | Total  |
|:---------|:-----------|:-----------|:---------|:----------|:------|:---------------|:-------|
| 89293    | 9256       | 32560      | 18174    | 685       | 1074  | 4722           | 155764 |

There have been a total of 19768 audits by our users across 13045 streets, averaging 1.52 audits per street.

### User stats and tool usage

TODO: Missions started vs missions completed (not sure we can do this; I expect it to be difficult, without much benefit). TODO: Add table of label counts, split by label type for rows and by user group for columns. Include totals and percentages.

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

Possible Stories
----------------

### Data overlap and agreement between users

Amongst all the data collected in DC, how much of DC is labeled by multiple users and what is the disagreement amongst them? (see comment in Outline document for details on implementation)

### Stickyness of tool: user dropoffs

We want a bar chart here showing, after a user clicks start mapping, what percentage finish the tutorial, what percentage finish a mission, etc.

Turk Study
==========

Update: This is now all of the data. There used to be 19 anonymous user routes, but three of them actually had no labels placed by the anonymous user (we had forgotten to check beforehand), thus we have only 16.

Even though 5 turkers did each route, the high level results for individual turkers looks only at the first turker to complete each set of routes. This makes aggregate stats more even, and a fairer comparison across user groups. (but maybe we should actually use all turkers when not aggregating, actually...)

High level results
------------------

TODO: Come up with our own zone type descriptions, possibly aggregating as well. <br> TODO: Add "n" to a bunch of graphs. <br> TODO: Percentage of turkers who completed the HIT (maybe?).

Below is a table showing number of ground truth labels by user group and by label type.

| worker.type | All    | Problem | CurbRamp | NoCurbRamp | Obstacle | SurfaceProb |
|:------------|:-------|:--------|:---------|:-----------|:---------|:------------|
| anon        | 775    | 297     | 478      | 19         | 59       | 219         |
| reg         | 3842   | 1108    | 2734     | 68         | 236      | 804         |
| total       | 4617   | 1405    | 3212     | 87         | 295      | 1023        |
| % of total  | 100.0% | 30.4%   | 69.6%    | 1.9%       | 6.4%     | 22.2%       |

A total of 330 turkers, 50 registered users, and 16 anonymous users were part of this study.

Below are two tables (street level, then 5 meter level) showing average (median) accuracy across all users when aggregating over all label types, and when aggregating over just the problem label types (missing curb ramp, surface problem, and obstacle). We show all the different accuracy types. We see that the first set of numbers is much higher, because curb ramp accuracy is very high in general. When we remove curb ramps in the second set of numbers, they become very low.

Median accuracy across all users - street level:

| label.type | recall | precision | f.measure |
|:-----------|:-------|:----------|:----------|
| All        | 0.714  | 0.674     | 0.667     |
| Problem    | 0.714  | 0.714     | 0.667     |

Median accuracy across all users - 5 meter level:

| label.type | recall | precision | f.measure |
|:-----------|:-------|:----------|:----------|
| All        | 0.543  | 0.472     | 0.468     |
| Problem    | 0.200  | 0.250     | 0.222     |

Then we show the above accuracy measures (but for only precision, recall, and f-measure), as an average (median) per user group. Also, I threw in issues/features (by that I just mean post-clustering) per 100m, because this is the only table right now that has all five user groups.

Median accuracy by user group - street level:

| user.type | all.recall | all.prec | all.f.meas | prob.recall | prob.prec | prob.f.meas |
|:----------|:-----------|:---------|:-----------|:------------|:----------|:------------|
| anon      | 0.466      | 0.665    | 0.611      | 0.182       | 0.755     | 0.267       |
| reg       | 0.796      | 0.492    | 0.553      | 0.512       | 0.474     | 0.463       |
| turk1     | 0.615      | 0.674    | 0.610      | 0.396       | 0.500     | 0.392       |
| turk3     | 0.585      | 0.809    | 0.676      | 0.265       | 0.667     | 0.344       |
| turk5     | 0.599      | 0.885    | 0.703      | 0.138       | 0.750     | 0.235       |

Median accuracy by user group - 5 meter level:

| user.type | all.recall | all.prec | all.f.meas | prob.recall | prob.prec | prob.f.meas |
|:----------|:-----------|:---------|:-----------|:------------|:----------|:------------|
| anon      | 0.356      | 0.574    | 0.474      | 0.056       | 0.148     | 0.090       |
| reg       | 0.620      | 0.378    | 0.434      | 0.230       | 0.195     | 0.198       |
| turk1     | 0.498      | 0.508    | 0.495      | 0.198       | 0.244     | 0.175       |
| turk3     | 0.502      | 0.694    | 0.575      | 0.111       | 0.250     | 0.154       |
| turk5     | 0.510      | 0.761    | 0.582      | 0.057       | 0.314     | 0.103       |

Next we have some descriptive statistics of users, by user group. These are average (median) stats.

| worker.type | labels.per.100m | feet.per.min | minutes.per.1k.ft | minutes\_audited |
|:------------|:----------------|:-------------|:------------------|:-----------------|
| anon        | 4.921           | 150.830      | 6.630             | 13.260           |
| reg         | 5.988           | 163.800      | 6.105             | 24.420           |
| turk1       | 6.808           | 628.456      | 1.591             | 6.365            |

Below, we have a table of aggregate (sum) stats by user group.

| worker.type | n.missions | distance.miles | n.labels | hours.audited |
|:------------|:-----------|:---------------|:---------|:--------------|
| anon        | 32         | 6.061          | 481      | 3.547         |
| reg         | 150        | 37.879         | 3626     | 21.518        |
| turk1       | 182        | 43.939         | 5559     | 9.037         |

Our average (mean) IRR over the 7 rounds, by label type, is in the table below:

| label.type  | mean.kripp.alpha |
|:------------|:-----------------|
| CurbRamp    | 0.907            |
| NoCurbRamp  | 0.787            |
| Obstacle    | 0.342            |
| SurfaceProb | 0.477            |
| Problem     | 0.475            |

Here is the zone type distribution for the mturk study. This shows the distribution of zone type for the routes that we took from anonymous and registered users and compare it to the distribution across all of DC. There are three zone types where anonymous users have no data, but registered users do. So the second graph shows the distribution when we remove the sets of routes from registered users the contain data from those three zone types. We will likely use the second set of data for comparison between the user groups. This removes 13 of the 50 sets of routes from registered users. There is still 16 sets of routes from anonymous users.

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.distribution-1.png)![](stats_for_paper_files/figure-markdown_github/turk.zone.type.distribution-2.png)

Possible Stories
----------------

### Street-level vs 5 meter-level

For simplicity, the graphs below count only one true/false positie/negative per segment, instead of counting the number of labels in that segment (i.e., binary instead of ordinal). All user groups are also combined (the groups being: registered volunteers, anonymous volunteers, and individual turkers).

NOTE: The red dots on the graphs are means.

NOTE: The "n" labels in this graph mark the number of *users* (since one user = one observation in this graph).

*Takeaways*:

-   Analyzing at the 5 meter level shows higher raw accuracy and specificity, both because of the large number of true negatives that we get from splitting into 5 meter segments; there are very few street segments with no labels at all.

-   Analyzing at the street level shows higher recall, implying that there were relatively fewer false negatives at the street level. This may mean that users aren't finding *every* issue, but they are more likely to find *at least one* issue of that type when there are multiple that occur on the same street.

-   Analyzing at the street level shows higher precision, implying that there were relatively fewer false positives at the street level. I suspect that this is due to fundamental misunderstandings about how to label (implying both that labeling is complex and difficult and that our onboarding is insufficient) which are persistent/consistent and frequent (think: labeling driveways as curb ramps, labeling storm drains as missing curb ramps, and labeling fire hydrants or street signs that are not in the way as obstacles). In those cases where the mistake is made frequently (multiple times per street), relatively fewer false positives makes sense when moving to street level analysis.

-   Analyzing at the street level shows higher f-measure. This clearly comes from the higher recall and precision.

-   CurbRamp pretty much outperforms all other lable types across the board, regardless of accuracy type of 5 meter vs. street level. This is likely because curb ramps are the easiest label type to understand and find in GSV (both because they are large and easy to see, and because you know where to expect them -- at intersections).

-   The SurfaceProblem label type seems to have the highest precision and lowest recall among the different types of issues (I'm excluding CurbRamp here). I guess that, relative to the other types of issues, there are just fewer cases of mistaking something of a surface problem and more cases of not finding a surface problem that was vsisible in GSV (so maybe surface problems require increased diligence from users, and the other issues require better treatment in onboarding).

-   The Problem type seems to perform better than the surface problem and obstacle label types (except for surface problem precision, mentioned in the previous bullet).

-   NoCurbRamp seems to have high recall and low precision. This fits my intuition; since users know to expect curb ramps at intersections, if they arrive at an intersection and a curb ramp is not there, they know to place a NoCurbRamp label. However, if there was no sidewalk at all, then we did not add the missing curb ramp labels to the ground truth dataset, and this is not something that we covered during onboarding. I suspect that this, paired with users marking storm drains as missing curb ramps, were the main reasons for the low recall. Both could be addressed through proper training.

![](stats_for_paper_files/figure-markdown_github/turk.granularity.analysis-1.png)

### Zone type (land use) effect on accuracy

The first graph shows all label types aggregated, the second shows the problem vs. no problem type.

Note: The red dots on the graphs are means.

*Takeaways*:

![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis-1.png)![](stats_for_paper_files/figure-markdown_github/turk.zone.type.analysis-2.png)

### Reg vs anon vs turker vs turk3 vs turk5

TODO: Make some graphs.

*Takeaways*:

### Does removing low severity brings higher recall

Below is a table with counts of ground truth labels in the dataset at different severity thresholds. This is followed by a trellis plot that shows how recall changes when we remove low severity problems from the ground truth dataset. This is shown by label type and user group.

*Takeaways*:

-   Only 17% of surface problems and 35% of obstacles had severity of 3 or higher. Most missing curb ramps are high severity (not surprising).

-   The general trend is a decent improvement in recall when we remove high severity labels from the ground truth.

-   Note that it *is* possible for the recall to get worse when removing high severity labels. This is because certain users may have found more of the low severity problems than the high severity ones.

| low.severity.thresh | Problem | NoCurbRamp | Obstacle | SurfaceProb |
|:--------------------|:--------|:-----------|:---------|:------------|
| baseline            | 1405    | 87         | 295      | 1023        |
| &gt;=3              | 352     | 74         | 106      | 172         |
| &gt;=4              | 158     | 42         | 54       | 62          |

![](stats_for_paper_files/figure-markdown_github/turk.high.severity.analysis-1.png)

### Binary vs ordinal issues per segment

For simplicity, the first graph looks at the 5 meter level, and the second looks at street level. All user groups are also combined (the groups being: registered volunteers, anonymous volunteers, and individual turkers).

Note: The red dots on the graphs are means.

*Takeaways*:

-   5 meter level (first graph): Considering multiple issues per segment results in *very slightly* lower accuracy for pretty much every type of label and type of accuracy (except precision). I suspect that this comes mostly from our method of clustering, which makes it unlikely that users end up with multiple labels per 5 meter segment. We do not have this restriction in the ground truth, so those few cases where we have more than one label per 5 meter segment in the GT usually results in an additional false negative when moving to ordinal analysis. However, the difference here is very small, so our clustering method seems fine to me.

-   Street level (second graph) recall: If we do this analysis at the street level, the decreases in accuracy are more pronounced. At this level, the clustering shouldn't have much effect. The decrease in recall suggests that users are finding *some* of the problems, but not *all* of them (meaning an increase in false negatives when we move to ordinal analysis).

-   Street level (second graph) recall: I suspect that the reason for the decrease in precision when moving to ordinal analysis at the street level is the same reason as why 5 meter level has lower precision than street level (seen in the previous section). That is, users' misunderstandings of how to label certain common things (driveways as curb ramps, etc.); since these mistakes are common, they may happen many times on a single street edge, which means that you start racking up the false positives when you move to ordinal analysis.

![](stats_for_paper_files/figure-markdown_github/turk.issues.per.seg.analysis-1.png)![](stats_for_paper_files/figure-markdown_github/turk.issues.per.seg.analysis-2.png)
