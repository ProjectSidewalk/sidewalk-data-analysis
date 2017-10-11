# sidewalk-data-analysis
Holds all offline data analysis scripts for Project Sidewalk.

As a temporary setup, if you want to run the code directly,
1. Create a `data` directory in the root directory.
2. Ask @misaugstad for the two CSVs that belong in there (They haven't been included yet; waiting to make sure that all data is ethically sound to share first).
3. Change the filepath in the `R/accuracy_analysis.Rmd` file, line 31, to point to the root directory for this project.
4. Knit the `R/accuracy_analysis.Rmd` file. You can do this using [RStudio](https://www.rstudio.com/), _the_ amazing, free, open-source, cross-platform R IDE. Once you open the file in RStudio, the shortcut is `ctrl-shift-K` (probably `cmd-shift-K` on Mac).
