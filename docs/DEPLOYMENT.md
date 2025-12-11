# Deployment Guide — SMS Grade Analysis Dashboard

This guide explains options for deploying the Shiny App and PDF report generator.

It covers:

-   Running locally (recommended for academics for now)
-   Deployment to UoA-hosted Shiny Server
-   Deployment via shinyapps.io
-   Deployment using Docker

------------------------------------------------------------------------

# 1. Local Execution (No Server Required)

Any academic with R + RStudio installed can run the full application offline.

### **Step 1 — Install dependencies**

``` r
install.packages(c(
  "shiny", "tidyverse", "readxl", "janitor", "stringr",
  "tools", "writexl", "dbplyr", "knitr", "tidyr",
  "ggplot2", "purrr", "tibble", "shinyjs"
))

install.packages("tinytex")
tinytex::install_tinytex()
```

### **Step 2 — Run Shiny**

``` r
rmarkdown::run("shiny/boe_bos_shiny_app.Rmd")
```

------------------------------------------------------------------------

# 2. Deployment on UoA-hosted Shiny Server

UoA maintains internal Shiny servers for secure academic tools.

### **Server requirements**

-   R ≥ 4.2
-   All packages listed above
-   Pandoc + LaTeX (TinyTeX recommended)
-   Shiny Server or Shiny Server Pro
-   HTTPS-enabled environment

### **Folder structure on server**

```         
/srv/shiny-server/sms-grade-dashboard/
│  ├─ app.R or boe_bos_shiny_app.Rmd
│  ├─ reports/
│  ├─ R/
│  └─ data/ (empty — for temp uploads only)
```

### **Deployment steps**

1.  Clone the GitHub repository:

``` bash
git clone https://github.com/ulingga/medsci-grade-analysis-dashboard.git
```

2.  Install R packages under server R library.

3.  Configure `/etc/shiny-server/shiny-server.conf`:

``` bash
location /sms-grade-dashboard {
    run_as shiny;
    app_dir /srv/shiny-server/sms-grade-dashboard;
}
```

4.  Restart server:

``` bash
sudo systemctl restart shiny-server
```

### **Security considerations**

-   Ensure uploaded files are not persisted indefinitely (GDPR-style practice).
-   Shiny Server Pro supports user authentication; open source version does not.

------------------------------------------------------------------------

# 3. Deployment via shinyapps.io

Good for external testing or ad-hoc demos.

### **Requirements**

``` r
install.packages("rsconnect")
rsconnect::setAccountInfo(name="xxx", token="xxx", secret="xxx")
```

### **Deploy**

``` r
rsconnect::deployDoc("shiny/boe_bos_shiny_app.Rmd")
```

### **Limitations**

-   File upload size limits\
-   No long-term data storage\
-   Must not upload real student data to external servers (policy!)

------------------------------------------------------------------------

# 4. Deployment via Docker

Suitable when UoA wants fully reproducible deployment.

### **Example Dockerfile**

``` dockerfile
FROM rocker/shiny:latest

RUN install2.r --error \
    shiny tidyverse readxl janitor stringr writexl dbplyr knitr tidyr ggplot2 purrr tibble shinyjs tinytex

COPY . /srv/shiny-server/app/

# TinyTeX for PDF reports
RUN tlmgr init-usertree
RUN /usr/local/bin/tinytex/install-unx.sh

EXPOSE 3838

CMD ["/usr/bin/shiny-server"]
```

### **Build & run**

``` bash
docker build -t sms-dashboard .
docker run -p 3838:3838 sms-dashboard
```

------------------------------------------------------------------------

# 5. Recommended Deployment Path for SMS

| Environment | Suitable for | Student data safe? | Notes |
|-----------------------|---------------|-------------------|---------------|
| **Local R + RStudio** | Individual academics | ✔️ Yes | Easiest setup |
| **UoA Shiny Server** | Programme-wide use | ✔️ Yes | Requires ITS deployment |
| **shinyapps.io** | Demo only | ❌ No | Do NOT upload real Canvas exports |
| **Docker container** | ITS-managed deployment | ✔️ Yes | Future-proof option |

------------------------------------------------------------------------

# 6. Maintenance Checklist

-   Update R packages annually\
-   Check TinyTeX installation (for PDF)
-   Run with example files after each Shiny update
-   Tag GitHub releases (`v1.x.x`) for reproducible BoE/BoS reporting
-   Keep `data/` empty and `.gitignore`d

------------------------------------------------------------------------

# 7. Contacts

Deployment questions:\
- **Jacqui Hannam** (Associate Professor, SMS)\
- **Emily Li** (Creator)
