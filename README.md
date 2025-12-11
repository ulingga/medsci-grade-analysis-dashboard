# SMS Grade Analysis Dashboard

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/ulingga/medsci-grade-analysis-dashboard)](https://github.com/ulingga/medsci-grade-analysis-dashboard/releases)
![R](https://img.shields.io/badge/R-%3E%3D%204.2-blue)
![Shiny](https://img.shields.io/badge/Shiny-app-blue)
![License](https://img.shields.io/github/license/ulingga/medsci-grade-analysis-dashboard)

This repository contains an **R Shiny + RMarkdown** workflow to support **Board of Examiners (BoE)** and **Board of Studies (BoS)** grade analysis for the School of Medical Sciences (SMS).

The tool provides:

- A **Shiny front-end** for academics to upload multiple Canvas grade exports and build a single canonical ETL table.
- Downloadable **UG/PG wide Excel tables** for further review in the meetings.
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
├─ LICENSE
├─ SMS Grade Analysis.Rproj
├─ shiny/
│  └─ boe_bos_shiny_app.Rmd         # Shiny app (ETL + UI + report trigger)
├─ reports/
│  └─ grade_analysis_report.Rmd     # Parameterised RMarkdown PDF report
├─ R/                               # (currently empty; reserved for future helpers)
├─ data/
│  └─ .gitignore                    # ensures real data is never committed
└─ docs/
   ├─ README_academic.md            # user guide for academics (local run)
   ├─ Developer_Guide.md            # architecture & dev notes
   └─ DEPLOYMENT.md                 # deployment options (Shiny server, Docker, etc.)
```

---

## 2. How to Use This (Quick Start – Local Run)

For academics or administrators comfortable with R/RStudio.

### 2.1. Install R Packages (once)

```r
install.packages(c(
  "shiny", "tidyverse", "readxl", "janitor", "stringr",
  "tools", "writexl", "dbplyr", "knitr", "tidyr",
  "ggplot2", "purrr", "tibble", "shinyjs", "rmarkdown"
))

# If you are a PC user, for PDF output (LaTeX)
install.packages("tinytex")
tinytex::install_tinytex()
```

### 2.2. Run the Shiny App

1. Open RStudio.
2. Click **File → Open Project…** and select `SMS Grade Analysis.Rproj`.
3. Once RStudio opens the project, copy and paste this into the Console and click **Enter**:

```r
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

Then:

- Upload Canvas grade exports one by one (`.csv`, `.xls`, `.xlsx`).  
- For each file, specify the `paper_code` using the prefix + suffix controls  
  (e.g. `MEDSCI` + `318` → `MEDSCI_318`).  
- Click **Combine into ETL** to build the master table.  
- Optionally remove papers using **Remove paper from ETL**.  

### Downloads available:

- **UG wide (Excel)**  
- **PG wide (Excel)**  
- **BoE/BoS PDF report**  
  via *Prepare analysis report → Download analysis report (PDF)*

More detailed, step-by-step instructions (written for non-technical academics) are in:

```text
docs/README_academic.md
```

---

## 3. For Developers & Maintainers

- Architecture, ETL functions, and report logic:  
  ```text
  docs/Developer_Guide.md
  ```

- Deployment instructions (UoA Shiny server, shinyapps.io, Docker, Dockerfile examples):  
  ```text
  docs/DEPLOYMENT.md
  ```

The `/R` folder is currently empty but reserved for future developers who may want to move ETL helper functions out of the Shiny document into standalone `.R` scripts.

---

## 4. Versioning & Releases

This repository is intended to follow **semantic versioning**:

- **v1.0.0** – first stable release for BoE/BoS (2025)  
- **v1.1.0** – minor enhancements (new plots, extra tables)  
- **v2.0.0** – major changes (new data model, additional analyses, year-on-year comparisons)

### Creating a Release

1. Ensure `main` is clean and working.  
2. On GitHub: **Releases → Draft a new release**.  
3. Tag the version (e.g. `v1.0.0`) and write release notes.  
4. Academics can then download the ZIP for that specific version.

---

## 5. License

This project is released under the **MIT License**.

See the `LICENSE` file for full terms. In summary, others are permitted to:

- Use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software  
- Provided that the copyright and permission notice are included in all copies or substantial portions of the Software

---

## 6. Citation

If used in academic reporting or internal documentation, please cite as:

> Li, E. (2025). *SMS Grade Analysis Dashboard: Shiny & RMarkdown workflow for BoE/BoS reporting* [Computer software]. GitHub.

---

## 7. Getting Support

At this stage, the SMS Grade Analysis Dashboard is not yet supported by the University IT.  
For unexpected behaviour, error messages, or questions about interpretation, please contact:

- **Emily Li** (Developer)
- **Jacqui Hannam** — *Associate Professor, SMS*

Please provide:
- A brief description of the issue  
- A screenshot (if possible)  
- The steps you took before the error appeared  
- The relevant Canvas grade export file (if appropriate)

This information helps us diagnose and reproduce the issue more easily.
