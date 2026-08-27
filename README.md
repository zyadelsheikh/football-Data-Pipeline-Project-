# Football Analytics Platform

A complete **Data Engineering & Analytics platform** for analyzing player and team performance across the **Top 5 European Football Leagues** from **2017 to 2026**.

The project transforms football data through an ETL workflow into a **Star Schema data warehouse**, then delivers interactive analytics through a **Streamlit dashboard** with support for **Azure Synapse Analytics**.

![Architecture Diagram](images/architecture.png)

---

##  Project Overview

The platform provides an end-to-end workflow for transforming raw football statistics into structured analytical data and interactive insights.

The main objectives are to:

- Build a scalable analytical data model.
- Transform and organize football statistics using ETL.
- Analyze player and team performance across seasons.
- Enable cross-league and cross-season comparisons.
- Provide an interactive analytics dashboard.
- Support both local data and cloud-based Azure Synapse storage.

---

##  Tech Stack

| Category | Technologies |
|---|---|
| **Language** | Python |
| **Data Processing** | Pandas |
| **ETL & Analysis** | Jupyter Notebook |
| **Data Warehouse** | Star Schema |
| **Local Storage** | CSV |
| **Cloud Data Warehouse** | Azure Synapse Analytics |
| **Visualization** | Streamlit |
| **Database Access** | SQL / Python |
| **Version Control** | Git & GitHub |

---

##  Data Pipeline Architecture

The project follows a structured analytical data workflow:

```text
                Football Data
                     │
                     ▼
             ETL & Transformation
                     │
                     ▼
              Star Schema Model
                     │
            ┌────────┴────────┐
            ▼                 ▼
       Local CSV        Azure Synapse
            │                 │
            └────────┬────────┘
                     ▼
            Streamlit Dashboard
                     │
                     ▼
              Football Insights
```

The pipeline separates the **data transformation layer**, **storage layer**, and **analytics/serving layer**, making the application easier to maintain and extend.

---

##  Data Warehouse

The analytical model is based on a **Star Schema** designed for efficient analytical queries.

![Star Schema](images/star_schema.png)

### Fact Table

**Fact_Player_Stats**

Contains player performance metrics across different:

- Players
- Teams
- Leagues
- Seasons

Including statistics such as:

- Goals
- Assists
- Minutes
- Matches
- xG / xA
- Shots
- Passing metrics
- Tackles
- Interceptions
- Blocks
- Touches
- Carries
- Progressive actions

### Dimension Tables

**Dim_Player**

Player attributes such as name, nationality, position, date of birth, and player ID.

**Dim_Team**

Team information and identifiers.

**Dim_League**

League information and identifiers.

**Dim_Season**

Season information used for temporal analysis.

This dimensional model enables analytical queries across multiple dimensions while keeping the fact table focused on measurable player performance.

---

## Streamlit Analytics Dashboard

The final serving layer is an interactive **Streamlit dashboard** designed to explore the processed football data.

###  Home

Provides a league-wide overview including:

- Player and team counts
- Top scorers
- Top assisters
- Goals by team
- General statistics
- Data export

![Home Dashboard](images/home.png)

### Player Season

Provides detailed player-level analysis:

- Player profile
- Key performance metrics
- Percentile-based performance analysis
- Radar charts
- Goals & Assists per 90
- Passing & shooting accuracy
- Player comparisons
- Cross-league comparisons
- Season-by-season trends
- Complete season statistics

![Player Analysis](images/player.png)

###  Team Season

Provides team-level analysis:

- Squad summary
- Advanced team statistics
- Top scorers
- Team comparisons
- Cross-league comparisons
- Season trends
- Full squad statistics

![Team Analysis](images/team.png)

### 📊 League Ranking

Ranks players according to different performance categories:

- Attacking
- Passing
- Dribbling & Carrying
- Defending
- Creation

Additional filters include:

- League
- Season
- Position
- Minimum minutes

![League Ranking](images/ranking.png)

---

##  Cross-League Analysis

The platform supports comparisons between players and teams across different leagues and seasons.

Player comparisons use **percentile-based metrics**, where players are evaluated relative to their own peer groups.

This makes statistical comparisons more meaningful when analyzing competitions with different statistical distributions.

> Cross-league comparisons represent statistical performance comparisons and should not automatically be interpreted as direct comparisons of overall player quality.

---

##  Data Coverage

The platform covers the **Top 5 European Leagues**:

- 🇬🇧 Premier League
- 🇪🇸 La Liga
- 🇮🇹 Serie A
- 🇩🇪 Bundesliga
- 🇫🇷 Ligue 1

### Dataset

- **Seasons:** 2017–2026
- **Leagues:** 5
- **Player-season records:** 25,000+
- **Data Model:** Star Schema

---

##  Getting Started

### Prerequisites

- Python 3.x
- Git
- pip

### Installation

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>

python -m venv venv
```

Activate the environment.

**Windows**

```bash
venv\Scripts\activate
```

**Linux / macOS**

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

##  Data Setup

The dashboard expects the following processed datasets:

```text
processed/
├── dim_player.csv
├── dim_team.csv
├── dim_league.csv
├── dim_season.csv
└── fact_player_stats.csv
```

These files are generated by the ETL workflow in:

```text
prj_fixed.ipynb
```

---

##  Run the Dashboard

Start the Streamlit application:

```bash
streamlit run app.py
```

The dashboard will be available at:

```text
http://localhost:8501
```

---

##  Azure Synapse Analytics

The platform supports two data sources:

```text
Local CSV
   │
   ▼
Streamlit

Azure Synapse
   │
   ▼
Streamlit
```

The data source can be switched through:

```text
.streamlit/secrets.toml
```

### Local Mode

```toml
data_source = "local"
```

### Azure Mode

```toml
data_source = "azure"
```

The database layer handles the differences between the local data model and the Azure Synapse schema, allowing the Streamlit pages to use the same internal structure regardless of the underlying data source.

###  Security

Azure credentials are stored in:

```text
.streamlit/secrets.toml
```

Sensitive credentials should **never be hardcoded or committed to GitHub**.

---

##  Project Structure

```text
Football-Analytics/
│
├── app.py
├── data.py
├── db.py
├── prj_fixed.ipynb
├── requirements.txt
│
├── processed/
│   ├── dim_player.csv
│   ├── dim_team.csv
│   ├── dim_league.csv
│   ├── dim_season.csv
│   └── fact_player_stats.csv
│
├── sql/
│   ├── create_tables.sql
│   └── load_to_azure.py
│
├── .streamlit/
│   └── secrets.toml.example
│
└── images/
    ├── architecture.png
    ├── star_schema.png
    ├── home.png
    ├── player.png
    ├── team.png
    └── ranking.png
```

---

## Data Limitations

The **2025–2026** source dataset contains only basic counting statistics:

- Goals
- Assists
- Minutes
- Matches

Several advanced metrics are unavailable for this season because they are not present in the original source data.

The dashboard automatically detects unavailable metrics and displays a warning rather than presenting misleading visualizations.

---

##  Future Improvements

Potential improvements include:

- Automated data ingestion
- Incremental ETL pipelines
- Scheduled data refresh
- Automated data quality validation
- Cloud-based object storage
- Additional football performance metrics
- Machine learning-based player performance prediction
- Production deployment

---

##  Key Project Highlights

This project demonstrates practical experience with:

- **ETL & Data Transformation**
- **Dimensional Data Modeling**
- **Star Schema Design**
- **Analytical Data Warehousing**
- **Python & Pandas**
- **SQL**
- **Azure Synapse Analytics**
- **Interactive Data Visualization**
- **Cross-League Statistical Analysis**
- **Cloud & Local Data Architecture**

---

##  License

This project is available under the license specified in the repository.

---

**Football Analytics Platform — Turning Football Data into Actionable Insights. **
