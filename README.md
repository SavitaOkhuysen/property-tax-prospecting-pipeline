Property Tax Prospecting Pipeline
An automated data pipeline that built a $1B+ prospecting dataset of 1,500+ high-value commercial properties across all 231 New Hampshire municipalities. Built for a real property tax consulting firm — not a portfolio exercise.
Business Impact
This pipeline directly enabled $100K–$200K in additional annual revenue by expanding the firm's tax abatement filings from approximately 80 to 150+ applications per cycle. For a small firm previously generating $500K–$1M annually, the long-term revenue potential through scaled filings and multi-year negotiations at 35% contingency is $4M–$8M per cycle — a 4–8x revenue multiplier delivered through data automation.

The firm offered a business partnership based on the results of this work.
The Problem
Property tax abatement consulting depends on identifying the right properties to pursue. The best prospects combine three factors: high assessed value (bigger dollar abatement), low equalization ratio (property is underassessed, so appeals are more likely to succeed), and post-revaluation timing (owners haven't appealed yet). Before this pipeline, prospecting was manual — limited to towns the firm already knew, with no systematic way to scan the entire state.
What the Pipeline Does
Scrapes commercial and industrial property data from three different municipal assessment systems (Vision Government Solutions, Patriot Properties, Avitar Associates) covering all 231 NH municipalities
Filters for properties with assessed values over $3M
Calculates full market value using real NH DRA equalization ratios
Incorporates the NH DRA 2023–2027 assessment review schedule to identify post-revaluation windows
Scores and ranks each property using a weighted model combining assessed value, equalization ratio, and appeal timing
Exports tiered prospecting lists ready for owner outreach
Scoring Model
The scoring model (v3) uses a geometric mean approach so that both high assessment AND low ratio must be present for a property to score well. A high assessment in a high-ratio town scores low. A low assessment in a low-ratio town scores low. Only the combination produces a top prospect.

Factor
Weight
Logic
Combined value + ratio score
50 points
Geometric mean of normalized assessment value and inverse ratio score
Assessment review timing
30 points
Post-revaluation years get highest scores (appeal window is freshest)
Raw abatement opportunity
20 points
Dollar size of the gap between assessed and full market value


Properties are then tiered: A (Top Priority), B (High Value), C (Monitor), D (Low Priority).
Key Results
Metric
Value
Municipalities covered
231 (all of NH)
Properties analyzed
1,200+ (live) / 561 (simulation)
Total assessed value
$5.2B
Tier A prospects
30
Tier A abatement opportunity
$47M

Data Sources
Source
Coverage
System
Vision Government Solutions
36 towns
gis.vgsi.com
Patriot Properties
33 towns
patriotproperties.com
Avitar Associates
49 towns
data.avitarassociates.com
NH DRA Equalization Ratios
All 231 towns
revenue.nh.gov
NH DRA Assessment Review Schedule
85 towns
revenue.nh.gov

Architecture
Each NH town uses one of three assessment platforms. The pipeline routes each town to the correct scraper automatically, handles authentication (including Avitar's text-based CAPTCHA), manages rate limiting, and consolidates results into a single standardized dataset.

NH DRA Ratios + Tax Rates (231 towns)

         │

         ▼

┌─────────────────────────┐

│  Town Router             │

│  (maps town → scraper)   │

└────┬────────┬────────┬───┘

     │        │        │

     ▼        ▼        ▼

  Vision   Patriot   Avitar

 (36 towns)(33 towns)(49 towns)

     │        │        │

     └────────┴────────┘

              │

              ▼

     Scoring Model v3

     (value × ratio × timing)

              │

              ▼

     Tiered Export (A/B/C/D)
Output Files
File
Contents
Use
nh_commercial_prospects_full.csv
All properties scored and ranked
Full pipeline review
nh_commercial_prospects_tier_a.csv
Top 30 immediate prospects
Priority outreach
nh_commercial_prospects_tier_ab.csv
Top 151 active prospects
Full outreach campaign
prospecting_analysis.png
4-panel analysis dashboard
Executive summary


The export includes blank enrichment columns (owner name, phone, email, contact status, notes) designed for the firm's manual outreach workflow.
Small Business Implementation
This was built for a firm operating within Microsoft 365 Business Standard — zero additional infrastructure cost:

Python + Selenium — statewide scraping across three assessment platforms
SharePoint — central data access point
Excel on SharePoint — prospecting dataset for the team
Power Automate — automated workflows for client communications and filing tracking
Synology NAS (RAID 1) — on-premise backup with automatic SharePoint sync
Power BI — connected to SharePoint Excel for reporting
Pipeline Modes
The notebook defaults to simulation mode, which demonstrates the full pipeline logic with synthetic NH data structured exactly like real municipal records. Set test_mode=False in the pipeline orchestrator cell to run live scraping across all 231 municipalities.
Tech Stack
Python (Selenium, Pandas, NumPy, Matplotlib, BeautifulSoup)
Selenium with headless Chrome for JavaScript-rendered municipal sites
Google Colab (execution environment)
Real NH DRA data (equalization ratios, tax rates, assessment review schedule)
What This Project Demonstrates
Building a complete data pipeline from scratch for a real business with real revenue impact
Scraping and consolidating data from multiple heterogeneous sources into a single standardized dataset
Designing a scoring model based on domain knowledge, not just statistical patterns
Thinking about data as a product — structured output, tiered prioritization, enrichment columns for downstream workflow
Delivering a tool that changed how a business operates, not just a report someone reads once
