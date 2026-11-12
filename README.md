# Global Energy, Growth \& Emissions Analysis

**A multi-notebook data science project on how energy access, the energy mix and efficiency relate to economic growth and CO₂ emissions across 176 countries (2000–2020).**

!\[Python](https://img.shields.io/badge/Python-3.10%2B-1F4E79?logo=python\&logoColor=white)
!\[pandas](https://img.shields.io/badge/pandas-data%20cleaning-150458?logo=pandas\&logoColor=white)
!\[Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter\&logoColor=white)
!\[Status](https://img.shields.io/badge/Status-Notebook%2001%20complete-2E86C1)

📓 [**Read Notebook 01 on nbviewer**](https://nbviewer.org/github/GregoryOnokwai/global-energy-growth-emissions/blob/main/notebooks/01_introduction_and_data_cleaning.ipynb) (best formatting) · [View on GitHub](notebooks/01_introduction_and_data_cleaning.ipynb)

\---

## Table of Contents

1. [The Business Problem](#the-business-problem)
2. [Project Roadmap](#project-roadmap)
3. [Dataset](#dataset)
4. [Notebook 01: Introduction \& Data Cleaning](#notebook-01-introduction--data-cleaning)
5. [Key Results from Cleaning](#key-results-from-cleaning)
6. [Repository Structure](#repository-structure)
7. [How to Run](#how-to-run)
8. [Limitations](#limitations)
9. [Author](#author)

\---

## The Business Problem

Governments, development banks and energy investors must decide **where to put limited capital and policy effort** to:

* **expand energy access**,
* **cut emissions**, and
* **keep economies growing**.

These goals often seem to conflict, and the evidence is spread across many indicators and 170+ countries. This project brings it together around **four questions**:

|#|Question|What we want to know|
|-|-|-|
|1|**Where is the gap?**|Which countries are furthest from universal electricity and clean-cooking access (UN SDG 7), and which are off-track for 2030?|
|2|**What moves the needle?**|Do more energy use, cleaner electricity or better efficiency go with higher income and lower emissions, and how strong is that link?|
|3|**Can growth be decoupled from emissions?**|Which countries grew their economies while holding emissions flat, and what do they have in common?|
|4|**Where is it heading?**|If current trends continue, where will renewable share, access and CO₂ be in 2030?|

\---

## Project Roadmap

|Notebook|Focus|Status|
|-|-|-|
|**01**|Introduction \& data cleaning|✅ Complete|
|02|Exploratory data analysis|⏳ In progress|
|03|Modelling \& forecasting|⏳ Planned|

\---

## Dataset

**Source:**\*\*Source:\*\* \[Global Data on Sustainable Energy (2000–2020)](https://www.kaggle.com/datasets/anshtanwar/global-data-on-sustainable-energy) by Ansh Tanwar, Kaggle.



\*\*Licence:\*\* \[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The original file is included unchanged in `data/raw/`. The file in `data/processed/` is a \*\*modified version\*\*: columns were renamed, data types fixed, some gaps filled, and flag columns added, as documented in Notebook 01.

|Property|Value|
|-|-|
|Rows × columns|**3,649 × 21**|
|Countries|**176**|
|Years|**2000–2020** (21 years)|
|Unit of analysis|Country-year|
|Overall missing cells|**\~9.1%**|

The dataset covers electricity and clean-cooking access, renewable capacity and share, electricity generation by source (fossil, nuclear, renewables), primary energy use, energy intensity, CO₂ emissions, GDP and geography.

\---

## Notebook 01: Introduction \& Data Cleaning

This notebook turns the raw file into a reliable, documented dataset for the analysis notebooks. Every decision is explained in the notebook.

|Step|What was done|
|-|-|
|**Profiling**|Shape, data types, descriptive statistics, and missing values by column and by year|
|**Skewness analysis**|Found heavily right-skewed volume variables (means 3–24× their medians) and flagged them for log transforms|
|**Standardised names**|Renamed long headers to short `snake\_case` names with unit suffixes (`\_pct`, `\_twh`, `\_kt`), and fixed a hidden newline in `Density\\n(P/Km2)`|
|**Fixed data types**|Converted density from text (e.g. `"1,265"`) to numbers and trimmed spaces from country names|
|**Panel check**|Confirmed no duplicate country-years; 172 of 176 countries have all 21 years|
|**Validity rules**|Clipped percentages to 0–100, checked for negative quantities, and treated impossible zeros (Tuvalu's primary energy) as missing|
|**Missing-value audit**|Identified *why* each column has gaps (not published, not applicable, or patchy reporting) and chose a treatment for each|
|**Fact-based filling**|Filled gaps from definitions, not guesses: static geography copied within each country, and nuclear output computed from the low-carbon identity|
|**Interpolation**|Within-country, between observed years only; no extrapolation, so 2020 CO₂ stays empty|
|**Outlier flagging**|Robust z-score (median and MAD) on GDP growth; flagged, not removed|
|**Coverage score**|Scored each country by how much of its core data was observed, so results can be checked with and without low-coverage countries|

<p align="center">
  <img src="images/missing\_by\_year.png" alt="Heatmap of missing values by column and year" width="700">
</p>

\---

## Key Results from Cleaning

* **105 missing nuclear values recovered** exactly from the low-carbon accounting identity, and **21 set to 0** for countries that have never produced nuclear power.
* **CO₂ data ends in 2019.** 2020 is entirely missing in the source, so it is left empty rather than guessed (COVID-era emissions fell, so carrying 2019 forward would overstate them).
* **51 extreme GDP-growth years flagged across 33 countries**, such as Libya in 2011 (−62%) and Iraq in 2003 (−37%). They are kept, because these shocks are real signals.
* **12 countries have low data coverage** (<70% of core cells observed). This is driven mostly by country-name mismatches in the source and by overseas territories.
* **Output:** a clean dataset of **3,649 rows × 39 columns**, including flag columns that record every value that was originally missing.

\---

## Repository Structure

```text
.
├── notebooks/
│   └── 01\_introduction\_and\_data\_cleaning.ipynb
├── data/
│   ├── raw/
│   │   └── global\_sustainable\_energy.csv
│   └── processed/
│       └── df\_clean.csv
├── images/
│   ├── missing\_by\_column.png
│   ├── missing\_by\_year.png
│   └── skewness\_by\_column.png
├── requirements.txt
└── README.md
```

\---

## How to Run

```bash
git clone https://github.com/GregoryOnokwai/global-energy-growth-emissions.git
cd global-energy-growth-emissions
pip install -r requirements.txt
jupyter notebook notebooks/01\_introduction\_and\_data\_cleaning.ipynb
```

Run all cells. The notebook reads from `data/raw/` and writes the clean file to `data/processed/` and the charts to `images/`.

\---

## Limitations

* **Associations, not proof.** Energy use and GDP rise together partly because richer countries can afford more of both.
* **Country-level data only.** Results suit macro-level strategy and prioritisation, not project- or utility-level decisions.
* **Uneven coverage.** The least reliable numbers are often for the least-developed countries, the ones most in need. That is why the coverage score is kept.
* **Subset variables.** Financial flows and renewable share of primary energy cover only some countries, so they are used only in analyses restricted to those countries.

\---

## Author

**Gregory Onokwai**: Data Analyst | M.Eng. Chemical Engineering

* Portfolio: [gregoryonokwai.github.io](https://gregoryonokwai.github.io)
* LinkedIn: [linkedin.com/in/gregory-onokwai](https://www.linkedin.com/in/gregory-onokwai)
* GitHub: [@GregoryOnokwai](https://github.com/GregoryOnokwai)

