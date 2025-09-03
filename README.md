# ISSAPP - Interactive Suicide and Self-harm Analysis Platform for Public Policy

![ISSAPP Logo](www/logo.png)

## Overview

ISSAPP is an interactive web application built with R Shiny that provides comprehensive visualization and analysis of suicide, suicide attempts, acute pesticide poisoning, and depression data in Colombia from 2009-2018. This application serves as complementary material for the doctoral thesis "Relationship between pesticide exposure and suicide attempts and suicide in Colombia during 2009-2018".

## Features

- **Multi-level Geographic Analysis**: Departmental and municipal level data visualization
- **Interactive Maps**: Dynamic choropleth maps using `tmap` and `leaflet`
- **Temporal Analysis**: Data exploration across different time periods (2009-2018)
- **Population Segmentation**: Analysis by total population, gender (men/women)
- **Multiple Health Indicators**:
  - Suicide mortality rates
  - Suicide attempt incidence rates  
  - Acute pesticide poisoning incidence rates
  - Depression and mood disorders prevalence rates
- **Cluster Analysis**: Spatial clustering visualization for different health events
- **Responsive Design**: Mobile-friendly interface with modern UI components

## Architecture

### Project Structure
```
issapp/
├── app.R                    # Main Shiny application
├── code/
│   ├── libraries.R          # Package loading and dependencies
│   ├── plotting-maps.R      # Core mapping functions
│   └── plotting-maps-cluster.R  # Cluster analysis functions
├── data/
│   ├── db-app-dep.xlsx      # Departmental level data
│   ├── db-app-mun.xlsx      # Municipal level data
│   ├── db-app-cluster.xlsx  # Cluster analysis data
│   └── shape/               # Shapefiles for geographic boundaries
├── docs/
│   ├── home_info.md         # Home page content
│   └── about.md             # Project information
├── www/                     # Static web assets
├── renv/                    # R environment management
└── renv.lock               # Dependency lock file
```

### Technology Stack

- **Backend**: R with Shiny framework
- **Frontend**: HTML, CSS, Bootstrap (via shinythemes)
- **Mapping**: tmap, leaflet, sf (spatial data)
- **Data Processing**: tidyverse, dplyr, readxl
- **UI Components**: shinycssloaders, shinythemes
- **Deployment**: Can be deployed on Shiny Server, RStudio Connect, or shinyapps.io

### Key Dependencies

```r
# Core Shiny
shiny, shinythemes, shinycssloaders

# Data manipulation
tidyverse, dplyr, readxl

# Spatial analysis and mapping  
sf, tmap, tmaptools, leaflet, raster

# Package management
pacman, renv
```

## Installation

### Prerequisites

- R (>= 4.0.0)
- RStudio (recommended)

### Option 1: Using renv (Recommended)

The project uses `renv` for reproducible package management.

```bash
# Clone the repository
git clone <repository-url>
cd issapp

# Open R/RStudio and restore the environment
R
```

```r
# Install renv if not already installed
if (!requireNamespace("renv", quietly = TRUE)) {
  install.packages("renv")
}

# Restore the project environment
renv::restore()
```

### Option 2: Manual Installation

```r
# Install required packages
required_packages <- c(
  "shiny", "shinythemes", "shinycssloaders", "pacman",
  "tidyverse", "dplyr", "readxl", "sf", "tmap", 
  "tmaptools", "leaflet", "raster", "stringr", "markdown"
)

install.packages(required_packages)
```

### Data Setup

The application expects Excel files in the `data/` directory:
- `db-app-dep.xlsx`: Departmental level data
- `db-app-mun.xlsx`: Municipal level data  
- `db-app-cluster.xlsx`: Cluster analysis data
- Shapefiles in `data/shape/` directory

**Note**: Due to data privacy considerations, the actual data files are included in `.gitignore`. Contact the authors for access to the datasets.

## Usage

### Running the Application

```r
# Set working directory to project root
setwd("path/to/issapp")

# Run the application
shiny::runApp()
```

The application will be available at `http://localhost:3838` (or the port specified by Shiny).

### Navigation

1. **Home**: Introduction and project overview
2. **About**: Detailed project information, data sources, and methodology
3. **Results**: Interactive maps with filtering options:
   - Geographic level (Departmental/Municipal)
   - Health indicator (Suicide, Suicide attempts, Poisoning, Depression)  
   - Population group (Total, Men, Women)
   - Time period (2009-2018, individual years)
4. **Cluster**: Spatial cluster analysis visualization

### Data Sources

- **Population Data**: DANE (National Administrative Department of Statistics)
- **Suicide Data**: DANE vital statistics
- **Suicide Attempts**: RIPS (Individual Health Service Provision Records)
- **Pesticide Poisoning**: SIVIGILA (Public Health Surveillance System)
- **Depression**: RIPS with ICD-10 codes F313-F341

## Development

### Code Organization

- **`app.R`**: Main application file with UI and server logic
- **`code/libraries.R`**: Centralized package loading
- **`code/plotting-maps.R`**: Core mapping functions and data processing
- **`code/plotting-maps-cluster.R`**: Cluster analysis specific functions

### Key Functions

- `map_function()`: Main mapping function for results visualization
- `map_function_cluster()`: Cluster analysis mapping
- `data_by_event()`: Data filtering and processing
- `data_to_map()`: Spatial data joining
- `data_breaks()`: Dynamic legend creation

### Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

## Deployment

### Local Deployment
```r
# Run locally
shiny::runApp()
```

### Server Deployment

For production deployment, consider:
- **Shiny Server**: Free, open-source option
- **RStudio Connect**: Commercial solution with advanced features
- **shinyapps.io**: Cloud hosting service

#### Example Dockerfile
```dockerfile
FROM rocker/r-base:4.0.0

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libssl-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libgdal-dev \
    libudunits2-dev

# Copy application files
COPY . /srv/shiny-server/issapp/

# Install R dependencies
RUN R -e "install.packages('renv')"
RUN R -e "renv::restore()"

# Expose port
EXPOSE 3838

# Run app
CMD ["R", "-e", "shiny::runApp('/srv/shiny-server/issapp', host='0.0.0.0', port=3838)"]
```

## Performance Considerations

- Data files are loaded on application start
- Map rendering can be intensive for municipal-level data
- Consider data caching for production deployments
- Optimize shapefiles for web use (simplify geometries if needed)

## Security Notes

- The `.Rprofile` file is correctly excluded from version control
- Data files containing sensitive information are gitignored
- Consider implementing authentication for production deployments

## Data Access and Licensing

This project is part of academic research. Please contact the authors for licensing and usage terms.

- **Source Code**: MIT License
- **Research Data**: Access restricted - contact authors for permission
- **Geographic Data**: Check individual shapefile licenses

## Citation

When using this application or its methods, please cite:

```
Castro-Cely, Y. (2023). Relación entre la exposición a plaguicidas con el intento 
suicida y suicidio en Colombia durante el periodo 2009-2018. [Doctoral thesis]. 
Universidad Nacional de Colombia.
```

## Authors

- **Yesenia Castro-Cely** - Principal Investigator
- **Gonzalo Novoa-Fernández** - Application Design and Development

## Contact

For questions, data access, or collaboration opportunities, please contact the authors through the Universidad Nacional de Colombia.

## Acknowledgments

- Universidad Nacional de Colombia - Doctoral Program in Public Health
- Dr. María Erley Orjuela Ramírez - Thesis Director
- Dr. Álvaro Javier Idrovo Velandia - Thesis Co-director
- DANE, SISPRO, and SIVIGILA for providing the data sources

---