# IMDB Data Analysis and Stream Processing - Final Project

## Project Team
**Sarah SHAHIN** | **Najlaa ALLIOUI** | **Hafsa REDOUANE**

---

## Overview

This project combines batch data analysis on IMDB datasets with real-time stream processing for Wikipedia events monitoring, demonstrating distributed data processing capabilities using Apache Spark and PySpark.

**Components:** Batch analytics (IMDB movies, actors, ratings) + Stream processing (Wikipedia event tracking)

---

## Technologies
- **Apache Spark 3.x** + **PySpark** - Distributed processing
- **Databricks** - Cloud platform
- **Unity Catalog** + **Delta Lake** - Data governance and storage

---

## Data Sources

**IMDB Datasets** (https://datasets.imdbws.com/):
- name.basics (~13M), title.basics (~10M), title.ratings (~1.4M), title.crew (~10M), title.akas (~37M), title.episode (~8M), title.principals (~60M)

**Wikimedia Events**: Wikipedia page edits (simulated using Spark rate source)

---

## Project Structure

**Cell 1: Data Loading** - Loads 7 IMDB datasets with custom safe casting for null handling  
**Cell 2: Batch Analysis** - Answers 14 questions on demographics, titles, ratings, and crew  
**Cell 3: Stream Processing** - Real-time monitoring with metrics and alerts  
**Cell 4: Verification** - Displays streaming results from Unity Catalog tables

---

## Stream Processing

### Tracked Entities
1. Best Comedy Movie (tconst)
2. Director (nconst)
3. Genre_Action
4. User_Type_Bot
5. Custom entity

### Metrics
- **Edit Count** (2-min windows) → `workspace.imdb_project.stream_metrics_output`
- **Bytes Changed** per entity

### Alerts
**Type:** LARGE_NON_BOT_EDIT  
**Triggers:** Entity edited + ≥1000 bytes + human editor  
**Output:** `workspace.imdb_project.stream_alerts_output`

### Storage
- **Checkpoints:** `dbfs:/Volumes/workspace/imdb_project/raw_imdb_files/stream_checkpoints_*`
- **Format:** Delta Lake (ACID + time travel)

---

## Setup

**Prerequisites:** Python 3.8+, Spark 3.x, Databricks account

**Steps:**
1. Download datasets from https://datasets.imdbws.com/
2. Upload to `workspace.imdb_project.raw_imdb_files`
3. Run notebook cells sequentially

**Requirements:** 16GB RAM minimum, 32GB recommended, Standard_DS3_v2+ cluster

---

## Results

**Batch:** 100M+ records processed, 28 genres cataloged, demographic profiles identified  
**Stream:** 5 entities tracked, 2-min window metrics, alert detection, Delta storage

---

## Technical Highlights

- Safe casting for IMDB's `\N` nulls and invalid entries
- Explicit schemas for large file handling
- Rate source simulation for Wikimedia events
- Unity Catalog integration with proper checkpointing

---

## Repository
```
├── README.md
├── project_BD.ipynb
└── data/ (>10GB, not in repo)
```

---

## Troubleshooting
- **Large files:** Increase driver memory or manual load
- **Permissions:** Grant Unity Catalog access
- **Checkpoints:** Clear if corrupted

---

## Acknowledgments
IMDB datasets | Apache Spark | Databricks | Wikimedia | Course Instructor (joe@adaltas.com)

**License:** Educational use only
