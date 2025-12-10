# SMS Grade Analysis Dashboard

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/ulingga/medsci-grade-analysis-dashboard)](https://github.com/ulingga/medsci-grade-analysis-dashboard/releases)
![R](https://img.shields.io/badge/R-%3E%3D%204.2-blue)
![Shiny](https://img.shields.io/badge/Shiny-app-blue)
![License](https://img.shields.io/github/license/ulingga/medsci-grade-analysis-dashboard)

This repository contains an **R Shiny + RMarkdown** workflow to support **Board of Examiners (BoE)** and **Board of Studies (BoS)** grade analysis for the School of Medical Sciences (SMS).

The tool provides:

- A **Shiny front-end** for academics to upload multiple Canvas grade exports and build a single canonical ETL table.
- Downloadable **UG/PG wide Excel tables** for further analysis.
- An automated **BoE/BoS PDF report** with:
  - Enrolment and unique student counts by stage  
  - Per-paper summary statistics (mean, median, SD, fail %)  
  - Stage-level summaries  
  - DNC/DNS tables  
  - Distribution plots and paired t-test comparisons between papers

> ⚠️ **Privacy note:** No real student data is included in this repository. The `data/` directory is `.gitignore`d and is intended only for local Canvas exports or synthetic example data.

---

## 1. Repository Structure

```text
.
├─ README.md
├─ .gitignore
<<<<<<< HEAD
=======
├─ SMS Grade Analysis.Rproj
>>>>>>> 9b56533 (Rewrote the README.md)
├─ shiny/
│  └─ boe_bos_shiny_app.Rmd         # Shiny app (ETL + UI + report trigger)
├─ reports/
<<<<<<< HEAD
│  └─ grade_analysis_report.Rmd   # PDF analysis report
└─ data/
   └─ .gitignore                  # DO NOT COMMIT real data
=======
│  └─ grade_analysis_report.Rmd     # Parameterised RMarkdown PDF report
├─ R/                               # (optional) shared helpers (ETL, plotting, etc.)
├─ data/
│  └─ .gitignore                    # ensures real data is never committed
└─ docs/
   ├─ README_academic.md            # user guide for academics
   ├─ Developer_Guide.md            # architecture & dev notes
   └─ DEPLOYMENT.md                 # instructions for deployment
```

---

## 2. How to Use This (Quick Start – Local Run)

For academics or administrators comfortable with R/RStudio.

### 2.1. Install R Packages (once)

```r
install.packages(c(
  "shiny", "tidyverse", "readxl", "janitor", "stringr",
  "tools", "writexl", "dbplyr", "knitr", "tidyr",
  "ggplot2", "purrr", "tibble", "shinyjs"
))

# If you are a PC user, for PDF output (LaTeX)
install.packages("tinytex")
tinytex::install_tinytex()
```

### 2.2. Run the Shiny App

From the project root in RStudio:

```r
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

Then:

- Upload one or more Canvas grade exports (`.csv`, `.xls`, `.xlsx`).  
- For each file, specify the `paper_code` using the prefix + suffix controls  
  (e.g. `MEDSCI` + `318` → `MEDSCI_318`).  
- Click **Combine into ETL** to build the master table.  
- Optionally remove papers using **Remove paper from ETL**.  

### Downloads available:

- **UG wide (Excel)**  
- **PG wide (Excel)**  
- **BoE/BoS PDF report**  
  via *Prepare analysis report → Download analysis report (PDF)*

More details are available in:

```
docs/README_academic.md
```

---

## 3. For Developers & Maintainers

- Architecture, ETL functions, and report logic:  
  ```
  docs/Developer_Guide.md
  ```

- Deployment instructions (UoA Shiny server, shinyapps.io, Docker):  
  ```
  docs/DEPLOYMENT.md
  ```

---

## 4. Versioning & Releases

This repository follows **semantic versioning**:

- **v1.0.0** – first stable release for BoE/BoS (2025)  
- **v1.1.0** – minor enhancements (new plots, extra tables)  
- **v2.0.0** – major changes (new data model, new analysis modules)

### Creating a Release

1. Ensure `main` is clean and working.  
2. On GitHub: **Releases → Draft a new release**  
3. Tag version (e.g., `v1.0.0`) and write release notes.  
4. Academics may download the ZIP for that specific version.

---

## 5. License

Specify the appropriate license (MIT, GPL-3, or institutional) in the repository.

---

## 6. Citation

If used in academic reporting or internal documentation, cite as:

> Li, E. (2025). *SMS Grade Analysis Dashboard: Shiny & RMarkdown workflow for BoE/BoS reporting* [Computer software]. GitHub.
>>>>>>> 9b56533 (Rewrote the README.md)

