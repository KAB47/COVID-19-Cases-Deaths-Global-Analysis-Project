# COVID-19 Cases & Deaths Global Analysis

SQL-driven global analysis of COVID-19 deaths, infection rates, and vaccination data across continents and countries — queried in SQL Server, results exported to Tableau for an interactive insights dashboard.

## TL;DR

SQL used to explore and extract four key result sets from raw global COVID-19 death and vaccination datasets, fed into a Tableau dashboard. Key findings:

1. 150.6M total cases and 3.18M deaths globally — a 2.11% global death rate
2. Europe had the highest death toll by continent — 1,016,750 deaths
3. North America second highest — 847,942, followed by South America at 672,415
4. Asia recorded 520,269 deaths; Africa 121,784; Oceania 1,046
5. Andorra had the highest infection rate globally — 17.13% of its population infected
6. Montenegro second at 15.51%, Czechia third at 15.23%, San Marino fourth at 14.93%
7. Time-series data tracked infection rate progression by country and date, enabling trend analysis over the course of the pandemic

## Key Results

| Metric | Value |
|--------|-------|
| Total global cases | 150,574,977 |
| Total global deaths | 3,180,206 |
| Global death percentage | 2.11% |
| Highest deaths by continent | Europe — 1,016,750 |
| Highest infection rate | Andorra — 17.13% of population |
| Second highest infection rate | Montenegro — 15.51% |
| Third highest infection rate | Czechia — 15.23% |

**Deaths by Continent:**
| Continent | Total Deaths |
|-----------|-------------|
| Europe | 1,016,750 |
| North America | 847,942 |
| South America | 672,415 |
| Asia | 520,269 |
| Africa | 121,784 |
| Oceania | 1,046 |

## Project Structure

```
COVID Cases & Deaths Analysis Project/
├── Dataset 1 (Covid Deaths).xlsx              # Raw deaths dataset
├── Dataset 2 (Covid Vaccinations).xlsx        # Raw vaccinations dataset
├── Inital Data Exploration.sql                # Exploratory SQL queries
├── SQL Queries Used For Tableau Dashboard.sql # Final queries for Tableau
├── SQL Query Result & Tableau Table 1.xlsx    # Global totals: cases, deaths, death %
├── SQL Query Result & Tableau Table 2.xlsx    # Total deaths by continent
├── SQL Query Result & Tableau Table 3.xlsx    # Infection rate by country (static)
├── SQL Query Result & Tableau Table 4.xlsx    # Infection rate by country over time
├── Tableau Dashboard.png                      # PNG File Type of Tableau Dashboard
├──Dashboard (Tableau File).twb                # Tableau workbook
└── README.md
```

## SQL Queries

Four queries were written to extract the key result sets fed into Tableau:

**Query 1 — Global death percentage:**
```sql
SELECT SUM(new_cases) AS total_cases,
       SUM(CAST(new_deaths AS INT)) AS total_deaths,
       SUM(CAST(new_deaths AS INT)) / SUM(new_cases) * 100 AS DeathPercentage
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1, 2
```

**Query 2 — Total deaths by continent:**
```sql
SELECT location, SUM(CAST(new_deaths AS INT)) AS TotalDeathCount
FROM PortfolioProject..CovidDeaths
WHERE continent IS NULL
AND location NOT IN ('World', 'European Union', 'International')
GROUP BY location
ORDER BY TotalDeathCount DESC
```

**Query 3 — Highest infection rate by country (static):**
```sql
SELECT Location, Population,
       MAX(total_cases) AS HighestInfectionCount,
       MAX((total_cases / population)) * 100 AS PercentPopulationInfected
FROM PortfolioProject..CovidDeaths
GROUP BY Location, Population
ORDER BY PercentPopulationInfected DESC
```

**Query 4 — Infection rate by country over time:**
```sql
SELECT Location, Population, date,
       MAX(total_cases) AS HighestInfectionCount,
       MAX((total_cases / population)) * 100 AS PercentPopulationInfected
FROM PortfolioProject..CovidDeaths
GROUP BY Location, Population, date
ORDER BY PercentPopulationInfected DESC
```

## Dashboard

The Tableau workbook (`Insights Dashboard of Analysis.twb`) visualises all four query result sets:

- Global KPI — total cases, deaths, and death percentage
- Deaths by continent — bar chart
- Infection rate by country — choropleth map
- Infection rate over time — time-series line chart by country

## Prerequisites

- **SQL Server** (or compatible RDBMS) to run the `.sql` files
- **Microsoft Excel** to view the raw datasets and query result tables
- **Tableau Desktop** to open the `.twb` dashboard file

## Setup

1. Import `Dataset 1 (CovidDeaths).xlsx` and `Dataset 2 (CovidVaccinations).xlsx` into your SQL Server instance as `CovidDeaths` and `CovidVaccinations` tables under a `PortfolioProject` database
2. Run `Inital Data Exploration.sql` to explore the raw data
3. Run `SQL Queries Used For Tableau Dashboard.sql` to generate the four result sets
4. Export each result as an Excel file matching the naming convention above
5. Open `Insights Dashboard of Analysis.twb` in Tableau Desktop and connect to the exported Excel files

## Tools Used

- **SQL Server** — data exploration, aggregation, and extraction
- **Microsoft Excel** — raw datasets and query result staging
- **Tableau** — interactive insights dashboard
