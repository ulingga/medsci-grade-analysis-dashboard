# SMS Grade Analysis – Academic User Guide (Local RStudio Version)

This guide explains how academic staff can run the **SMS Grade Analysis Tool** locally to:

-   Upload Canvas grade exports\
-   Combine multiple papers into a single dataset\
-   Generate UG/PG wide summary tables\
-   Produce a formatted PDF report for BoE / BoS meetings

No coding skills are required once the app is launched.

------------------------------------------------------------------------

## 1. System Requirements

To run the tool locally:

-   **R** (version 4.2 or higher)\
    Download: <https://cran.r-project.org>\
-   **RStudio Desktop**\
    Download: <https://posit.co/download/rstudio-desktop/>\
-   Internet connection (first-time package installation)
-   Canvas grade export files for each paper (CSV or Excel)

------------------------------------------------------------------------

## 2. Downloading the Project

### Option A: Download ZIP from GitHub

1.  Go to the repository page<https://github.com/ulingga/medsci-grade-analysis-dashboard/tree/main/>.\
2.  Click **Code → Download ZIP**.\
3.  Unzip the folder.\
4.  Open the unzipped project folder.

### Option B: Receive a ZIP from School IT

Unzip the folder and proceed.

Inside the project you will see:

```         
SMS Grade Analysis/
 ├─ shiny/
 │   └─ boe_bos_shiny_app.Rmd
 ├─ reports/
 │   └─ grade_analysis_report.Rmd
 ├─ data/
 ├─ README.md
 └─ SMS Grade Analysis.Rproj
```

------------------------------------------------------------------------

## 3. Opening the Project in RStudio

1.  Double-click the file ending with `.Rproj`.\
2.  This opens RStudio with the correct working directory.

Verify you are in the correct folder:

``` r
getwd()
list.files()
```

You should see folders such as `shiny/`, `reports/`, `data/`.

------------------------------------------------------------------------

## 4. Installing Required R Packages

Run the following **once** in R console (if you have never downloaded these packages):

``` r
install.packages(c(
  "shiny", "shinyjs", "tidyverse", "readxl", "janitor", "stringr",
  "tools", "writexl", "dbplyr", "knitr", "tidyr",
  "ggplot2", "purrr", "tibble", "rmarkdown"
))
```

### if you use a *PC* instead of a Mac, install TinyTeX for PDF report generation:

``` r
install.packages("tinytex")
tinytex::install_tinytex()
```

------------------------------------------------------------------------

## 5. Launch the Grade Analysis Tool

Run in Console:

``` r
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

This opens the Shiny interface in your browser or RStudio Viewer.

------------------------------------------------------------------------

## 6. Uploading Canvas Grade Files

You may upload **one or multiple papers** and combine them into a single dataset.

Steps:\
1. Click **Browse** and select a Canvas grade export (`.csv`, `.xls`, `.xlsx`).\
2. Choose the **Paper category (prefix)**\
e.g., `MEDSCI`, `MEDIMAGE`, `CLINIMAG`, `PHARMACOL`, `PHYSIOL`, `BIOMED`\
3. Enter the **Paper code (suffix)**\
e.g., `142`, `201`, `318`, `303AB`, `729`\
4. Click **Combine into ETL**.

Repeat these steps to add more papers.

------------------------------------------------------------------------

## 7. Reviewing Uploaded Papers

Go to **Uploaded Papers** tab:

-   **Current Paper List** shows all papers added to the ETL.\
-   **Master dataset preview** shows the last 10 rows of the combined dataset.

Confirm: \
- All expected papers appear\
- Codes are correct\
- No accidental duplicates

------------------------------------------------------------------------

## 8. Removing a Paper (If Uploaded Incorrectly)

If a paper was uploaded incorrectly:

1.  Select it from **Remove a paper from ETL** dropdown.\
2.  Click **Remove selected paper from ETL**.

The master dataset updates immediately.

------------------------------------------------------------------------

## 9. Downloading Outputs (Excel Tables)

Under **Downloads & Reports**, you may download:

### UG wide (Excel)

-   One row per student\
-   Grade and score columns for each undergraduate paper

### PG wide (Excel)

-   Same structure for postgraduate papers (`paper_num ≥ 700`)

These tables can be used for: - Additional analysis\
- Excel summaries\
- Custom statistical models

------------------------------------------------------------------------

## 10. Generating the PDF Report (BoE / BoS)

In **Downloads & Reports**:

1.  Click **Prepare analysis report**.\
2.  When complete, click **Download analysis report (PDF)**.

The report includes:

### Summary Statistics

-   Unique student counts\
-   Enrolments by stage\
-   Mean, median, SD, fail % per paper\
-   Stage-level summaries

### DNC / DNS Tables

-   Students with DNC/DNS outcomes\
-   Table for students with ≥2 DNC/DNS outcomes (contact me if you'd like to change this)

### Visualisations

-   Boxplots of score distributions\
-   Density plots by paper and stage

### Paired Paper Comparisons

-   Paired t-tests for students taking both papers\
-   Mean differences\
-   Confidence intervals\
-   Significance flags

------------------------------------------------------------------------

## 11. Interpreting Key Outputs

### Fail grades include:

```         
D+, D, D-, F, DNC, DNS
```

### Paired Comparisons:

-   Positive values → higher scores in Paper 1\
-   Negative values → higher scores in Paper 2\
-   “Significant” means p \< 0.05

### Outliers:

-   Identified using the 1.5 × IQR rule

------------------------------------------------------------------------

## 12. Troubleshooting

### “Cannot produce PDF”

Run:

``` r
tinytex::install_tinytex()
tinytex::tlmgr_update()
```

### “File not found”

Ensure you opened the project using the `.Rproj` file.

### Shiny does not open in Viewer

Force browser mode:

``` r
options(shiny.launch.browser = TRUE)
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

------------------------------------------------------------------------

## 13. Getting Support

## 13. Getting Support

At this stage, the SMS Grade Analysis Dashboard is not yet supported by University IT.  
For any unexpected behaviour, error messages, or questions about interpretation, please contact:

- **Emily Li** (Developer) 
- **Jacqui Hannam** — *Associate Professor, School of Medical Sciences*

Please provide:\
- A brief description of the issue  
- A screenshot (if possible)  
- The steps you took before the error appeared  
- The relevant Canvas grade export file (if appropriate)

This information helps us diagnose and reproduce the issue quickly.

------------------------------------------------------------------------

## End of Guide

This tool is designed to streamline BoE/BoS preparation and provide consistent, reproducible analysis across MEDSCI and related programmes.
