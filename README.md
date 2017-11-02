# sidewalk-data-analysis
Holds all offline data analysis scripts for Project Sidewalk.

As a temporary setup, if you want to run the code directly,
1. Create a `data` directory in the root directory.
2. Ask @misaugstad for the two CSVs that belong in there (They haven't been included yet; waiting to make sure that all data is ethically sound to share first).
3. Download and install [RStudio](https://www.rstudio.com/), _the_ amazing, free, open-source, cross-platform R IDE, if you haven't already.
4. Click File->Open Project and choose the `sidewalk-data-analysis.Rproj` file to open the project.
5. Open the `R/accuracy_analysis.Rmd` file.
5. Knit the `R/accuracy_analysis.Rmd` file. The shortcut is `ctrl-shift-k` (probably `cmd-shift-K` on Mac).
6. If it complains that packages aren't installed, look at the list of imports in the first code chunk, and run `install.packages(c('package1', 'package2', ...))` in the R console.
