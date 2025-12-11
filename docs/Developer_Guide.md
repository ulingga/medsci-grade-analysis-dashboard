---
editor_options: 
  markdown: 
    wrap: sentence
---

# Developer Guide — SMS Grade Analysis Dashboard

This document provides a technical overview of the SMS Grade Analysis Dashboard, including architecture, ETL logic, the Shiny app structure, and developer conventions.\
It is intended for developers, maintainers, and future contributors.

------------------------------------------------------------------------

## 1. Project Architecture

```         
.
├─ shiny/
│  └─ boe_bos_shiny_app.Rmd      # Shiny UI + server + ETL integration
├─ reports/
│  └─ grade_analysis_report.Rmd  # Parameterised PDF report
├─ R/                            # Optional: shared helper functions
├─ data/                         # Ignored (contains Canvas grade exports)
├─ docs/
│  ├─ Developer_Guide.md
│  ├─ README_academic.md
│  └─ DEPLOYMENT.md
└─ README.md
```

The system contains three major components:

### **1.1 ETL (Extract–Transform–Load)**

Located inside `boe_bos_shiny_app.Rmd`.

Functions include: - `read_canvas_file()` — reads CSV/XLSX Canvas exports.
- `clean_canvas_grades()` — standardises columns and cleans rows.
- `process_one_file()` — wrapper for reading + cleaning a single paper.
- `make_grades_long_unique()` — collapses multiple attempts into one row.
- `make_ug_pg_wide()` — creates UG and PG "wide" format sheets.

### **1.2 Shiny Application**

The RMarkdown Shiny document contains:

-   **UI components**
    -   File upload
    -   paper_prefix + paper_suffix input
    -   Combine into ETL
    -   Remove paper from ETL
    -   Download buttons
    -   PDF report generation
-   **Server logic**
    -   Maintains a reactive long-format ETL table
    -   Validates file types, handles errors safely
    -   Generates UG/PG Excel files
    -   Saves an `.rds` for the PDF report

### **1.3 Parameterised RMarkdown Report**

`grade_analysis_report.Rmd` uses:

-   `params$data_path` (passed from Shiny)
-   Reads ETL via `readRDS()`
-   Computes:
    -   Unique counts per stage
    -   Paper-level statistics
    -   Stage summaries
    -   DNC/DNS lists
    -   Boxplots + density plots
    -   Paired t-tests (UG + PG)

------------------------------------------------------------------------

## 2. ETL Logic Overview

### **2.1 Canonical schema (long format)**

```         
paper_code
student_name
student_id
unposted_final_score
unposted_final_grade
```

This structure is consistent across all Canvas files and becomes the foundation for long-to-wide transformations and statistical reporting.

### **2.2 Handling duplicates and multiple attempts**

`make_grades_long_unique()` applies:

-   Highest score wins\
-   First non-missing grade used

### **2.3 UG/PG splitting**

Logic:

-   `paper_num < 700` → Undergraduate
-   `>= 700` → Postgraduate

### **2.4 Wide-format naming convention**

```         
MEDSCI_301_score
MEDSCI_301_grade
MEDSCI_302_score
MEDSCI_302_grade
...
```

Sorted numerically by `paper_num`.

------------------------------------------------------------------------

## 3. Shiny Architecture

### **3.1 Reactive Data Store**

`all_grades <- reactiveVal(empty tibble)`

Updated via: - Add paper - Remove paper

### **3.2 Error Handling**

User-friendly modal dialogs catch:

-   Invalid file types
-   Missing columns
-   Duplicate uploads
-   Empty ETL

### **3.3 File Downloads**

Excel files generated with `writexl::write_xlsx()`.

### **3.4 PDF Report Workflow**

1.  Save ETL as temp `.rds`
2.  Render `grade_analysis_report.Rmd`
3.  Provide PDF to user

------------------------------------------------------------------------

## 4. Coding Conventions

### **4.1 Style**

-   `tidyverse` grammar
-   `snake_case` for column names
-   One function per task
-   Avoid side effects inside functions

### **4.2 File organisation**

Future developers may move ETL helpers into `/R/etl_functions.R`.

------------------------------------------------------------------------

## 5. Testing

### **5.1 Local testing**

Run Shiny using:

``` r
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

### **5.2 Synthetic example data**

Place examples (NOT real student data) into:

```         
data/example_canvas_files/
```

------------------------------------------------------------------------

## 6. Future Development Ideas

-   Add unit tests (`testthat`)
-   Add year-on-year comparison module
-   Integrate Te Whare Tapa Whā reporting
-   Add cluster analysis or predictive modelling
-   Add user authentication (Shiny Server Pro)

------------------------------------------------------------------------

## 7. Contact

For development questions:\
- **Emily Li** (Creator)\
