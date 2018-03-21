Accuracy Summary
================
Mikey Saugstad
October 10, 2017

Preliminary Notes
-----------------

-   For all graphs relating to accuracy, each data point is a single user (or a set of clustered users) over a single condition (i.e., 2-3 routes, or 2000/4000ft).
-   In the case of 5 meter and 10 meter segment granularity, raw accuracy and specificity get a huge boost from a large number of true negatives.
-   For NoSidewalk issues, these should only be looked at in the street-level, binary case. Both because there is not an established way to label, and because that is the level of granularity that users would actually care about.

Comparing Anon vs. Registered Volunteers vs. Turkers
----------------------------------------------------

*Takeaways*:

This section has a series of boxplots that compare performance of anonymous volunteers, registered volunteers, a single turker, and 5 turkers with majority vote. Here, a the single turker is the first turker to complete that set of routes.

For each granularity and label type, an ANOVA test was run, with the p-value being reported in the top-right corner of each boxplot. I appeneded a `**` if the p-value was less than 0.01, and just a `*` if the p-value was less than 0.05, to make finding significant results easier.

To make room for the p-value on each boxplot, I expanded the y-limit past 1.0, and then added a dotted line at the 1.0 mark so that we still have that reference point. However, it makes the graphs look a bit ugly, so LMK if you want to remove it.

#### Raw Accuracy

Defined as $\\frac{TP + TN}{TP + TN + FP + FN}$. Just the percentage of things they got correct.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/worker-types-raw-accuracy-1.png)

#### Recall

Defined as $\\frac{TP}{TP + FN}$. High recall means that they found most of the issues/features.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/worker-types-recall-1.png)

#### Precision

Defined as $\\frac{TP}{TP + FP}$. High precision means that they rarely placed a label when they shouldn't have.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/worker-types-precision-1.png)

#### F-measure

Defined as $2 \* \\frac{precision \* recall}{precision + recall}$. It is essentially a balance between recall and precision.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/worker-types-f-measure-1.png)

#### Specificity

Defined as $\\frac{TN}{TN + FP}$. Similar to precision, high specificity means that they rarely placed a label when they shouldn't have, but specificity gives more weight to true *negatives*, while precision gives more weight to true *positives*.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/worker-types-specificity-1.png)

Volunteer Data
--------------

In this section, there are a series of histograms that help to visualize the distribution of volunteers' accuracy. For each accuracy measure, there is a grid of histograms split by label type and granularity (street, 5 meter, 10 meter).

Note that these histograms have lines representing the *mean* of each group (not the *median*; lmk if you want to see median instead).

#### Raw accuracy

Defined as $\\frac{TP + TN}{TP + TN + FP + FN}$. Just the percentage of things they got correct.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/volunteer-raw-accuracy-1.png)

#### Recall

Defined as $\\frac{TP}{TP + FN}$. High recall means that they found most of the issues/features.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/volunteer-recall-1.png)

#### Precision

Defined as $\\frac{TP}{TP + FP}$. High precision means that they rarely placed a label when they shouldn't have.

*Note*: Very little confidence should be given to precision for the NoSidewalk label, since GT labelers only placed the label at intersections and at places where a sidewalk ends.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/volunteer-precision-1.png)

#### F-measure

Defined as $2 \* \\frac{precision \* recall}{precision + recall}$. It is essentially a balance between recall and precision.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/volunteer-f-measure-1.png)

#### Specificity

Defined as $\\frac{TN}{TN + FP}$. Similar to precision, high specificity means that they rarely placed a label when they shouldn't have, but specificity gives more weight to true *negatives*, while precision gives more weight to true *positives*.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/volunteer-specificity-1.png)

Turker Data
-----------

### Comparing effects of number of turkers and vote type on accuracy

In this section, there are a series of line graphs to help visualize how the number of turkers used and the method of voting affects the various accuracy measures. For each accuracy measure, there is a grid of line graphs, split by label type and granularity (same as above). However, each graph also has a line for each of the voting methods. You will notice that all voting methods are equivalent when looking at only one turker.

#### Raw Accuracy

Defined as $\\frac{TP + TN}{TP + TN + FP + FN}$. Just the percentage of things they got correct.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/vote-type-raw-accuracy-1.png)

#### Recall

Defined as $\\frac{TP}{TP + FN}$. High recall means that they found most of the issues/features.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/vote-type-recall-1.png)

#### Precision

Defined as $\\frac{TP}{TP + FP}$. High precision means that they rarely placed a label when they shouldn't have.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/vote-type-precision-1.png)

#### F-measure

Defined as $2 \* \\frac{precision \* recall}{precision + recall}$. It is essentially a balance between recall and precision.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/vote-type-f-measure-1.png)

#### Specificity

Defined as $\\frac{TN}{TN + FP}$. Similar to precision, high specificity means that they rarely placed a label when they shouldn't have, but specificity gives more weight to true *negatives*, while precision gives more weight to true *positives*.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/vote-type-specificity-1.png)

Removing Low Severity
---------------------

In this section, we are looking at how accuracy is effected when we remove low severity labels from the GT set. The idea is that higher severity problems (as defined by the GT labelers) will be easier for crowd workers to find, resulting in higher recall. This would be a nice result to have, to say that crowd workers can at least find the most severe problems. The two expected outcomes here: recall goes up, and for precision to go down. I've included all accuracy types for now in case we see anything interesting.

*Note*: Low severity labels were removed from the *ground truth data only*. This is why we expect precision to go down, as legitimate problems have been removed from the ground truth, that crowd workers may have seen. Removing only crowd worker labels, but not GT labels, would likely result in higher precision and lower recall. Removing labels from both GT and crowd workers would probably result in lower precision *and* recall, due to the variability in how people label severity. Discrepency between GT and crowd worker severity ratings will be addressed in a separate section.

### How many low severity problems are there?

Below is a graph that shows the number of conditions (sets of routes) containing at least one label (split by label type), and how that is affected by removing low severity labels. On that first graph, the horizontal line shows the number of conditions that is in the current data set (some still need to be added once I fix some bugs and such).

The second graph shows the GT label counts, and how they are affected by removing low severity labels.

    ## Warning: Removed 1 rows containing missing values (geom_point).

    ## Warning: Removed 9 rows containing missing values (geom_hline).

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-label-counts-1.png)

    ## Warning: Removed 1 rows containing missing values (geom_point).

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-label-counts-2.png)

#### Raw Accuracy

Defined as $\\frac{TP + TN}{TP + TN + FP + FN}$. Just the percentage of things they got correct.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-raw-accuracy-1.png)

#### Recall

Defined as $\\frac{TP}{TP + FN}$. High recall means that they found most of the issues/features.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-recall-1.png)

#### Precision

Defined as $\\frac{TP}{TP + FP}$. High precision means that they rarely placed a label when they shouldn't have.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-precision-1.png)

#### F-measure

Defined as $2 \* \\frac{precision \* recall}{precision + recall}$. It is essentially a balance between recall and precision.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-f-measure-1.png)

#### Specificity

Defined as $\\frac{TN}{TN + FP}$. Similar to precision, high specificity means that they rarely placed a label when they shouldn't have, but specificity gives more weight to true *negatives*, while precision gives more weight to true *positives*.

![](accuracy_analysis-pre_single_user_clustering_files/figure-markdown_github-ascii_identifiers/low-severity-specificity-1.png)

Incorporating single-user clustering
------------------------------------

#### Expected effects

-   There will be *significantly fewer* ramp labels, and *slightly* fewer labels from the other label types: since ramp labels often come in pairs, although a much lower distance threshold is being used for the clustering of ramp labels, we can expect a fair number of legitimate ramp labels to be excluded.
-   For larger numbers of turkers being clustered, the *reduction in labels might be more pronounced*: (something to do with labels that have just enough votes without the single-user clustering).
-   Precision will *improve slightly* for label types: in situations where a user labeled the same problem/feature multiple times, clustering should remove the duplicate label, thus improving precision.
-   Recall will be *slightly worse* for all non-ramp label types: there are some cases where there are, in fact, multiple distinct issues in close proximity to one another. In such cases, clustering will remove one of the labels from the volunteer's data, thus lowering recall.
-   Recall with be *significantly worse* for CurbRamp and NoCurbRamp labels: due to there being significantly fewer ramp labels.
-   Confidence intervals will get *slightly larger* for turkers with majority vote: fewer individual labels -&gt; fewer labels that pass majority vote -&gt; more conditions where turkers couldn't agree on any labels
-   Confidence intervals will be *slightly larger* for precision and f-measure: fewer labels =&gt; fewer true/false positives =&gt; more users with no true or false positives =&gt; more users with null precision (since denominator is TP + FP), and f-measure (since precision is used to computer f-meausure) =&gt; larger confidence intervals for those accuracy types (b/c smaller n).
-   Specificity will *improve slightly* across the board: fewer labels =&gt; more true negatives and fewer false positives =&gt; higher specificity.

#### Observed effects
