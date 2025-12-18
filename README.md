# IMDB Data Analysis and Stream Processing - Final Project

## Project Team
- **Sarah SHAHIN**
- **Najlaa ALLIOUI**
- **Hafsa REDOUANE**

---

## Overview

This project performs comprehensive data analysis on IMDB datasets and implements stream processing for monitoring Wikipedia events. It demonstrates proficiency in large-scale distributed data processing using Apache Spark and PySpark, combining batch analytics with real-time stream processing.

**Key Components:**
1. **Batch Data Analysis**: Analyzing IMDB movies, actors, genres, and ratings
2. **Stream Processing**: Real-time monitoring of Wikipedia events for selected IMDB entities

---

## Technologies Used

- **Apache Spark 3.x** - Distributed data processing
- **PySpark** - Python API for Spark
- **Databricks** - Cloud-based Spark platform
- **Unity Catalog** - Data governance and management
- **Delta Lake** - Storage layer for streaming and batch data

---

## Data Sources

### IMDB Datasets
All datasets sourced from [IMDB Non-Commercial Datasets](https://datasets.imdbws.com/):

- **name.basics** (~13M records) - Person information
- **title.basics** (~10M records) - Title metadata
- **title.ratings** (~1.4M records) - Rating data
- **title.crew** (~10M records) - Crew assignments
- **title.akas** (~37M records) - Alternative titles
- **title.episode** (~8M records) - Episode data
- **title.principals** (~60M records) - Cast/crew roles

### Wikimedia Event Streams
Real-time Wikipedia page edit events tracked for selected entities. In this implementation, a simulated stream using Spark's rate source is used for demonstration.

---

## Project Structure

### Cell 1: Data Loading and Cleaning
Initializes Spark session and loads all IMDB datasets with universal cleaning. Implements custom functions for safe type casting and null handling to manage IMDB's `\N` notation and problematic data types.

### Cell 2: Batch Analysis Questions
Answers all 14 project questions covering:
- People demographics (birth years, career spans)
- Title characteristics (runtime, genres, release years)
- Ratings analysis (highest-rated content by criteria)
- Crew identification (directors, alternate titles)

### Cell 3: Stream Processing Setup
Implements real-time monitoring system with:
- Entity selection from IMDB analysis results
- Stream source configuration
- Metrics calculation pipeline (2-minute windows)
- Alert detection logic for specific event types
- Output sink configuration to Unity Catalog tables

### Cell 4: Stream Verification
Verifies streaming outputs by reading from Unity Catalog tables and displaying aggregated metrics and triggered alerts.

---


## Stream Processing Implementation

### Tracked Entities
Five entities selected from IMDB analysis:
1. Best Comedy Movie (tconst)
2. Director of best comedy (nconst)
3. Genre_Action (abstract concept)
4. User_Type_Bot (bot activity tracking)
5. Additional custom entity

### Metrics Calculated

**Edit Count by Entity (2-Minute Windows)**
- Counts edits per entity in tumbling windows
- Tracks both frequency and magnitude of changes
- Output stored in `workspace.imdb_project.stream_metrics_output`

**Total Bytes Changed**
- Aggregates byte changes per entity
- Identifies magnitude of content modifications

### Alert System

**Alert Type: LARGE_NON_BOT_EDIT**

Triggers when:
- Specific tracked entity is edited
- Byte change ≥ 1000 bytes
- Editor is human (non-bot)

**Use Cases:**
- Detecting significant manual edits
- Identifying potential vandalism
- Triggering content review notifications

Alerts routed to separate table: `workspace.imdb_project.stream_alerts_output`

### Storage Configuration

**Checkpoints:**
- Metrics: `dbfs:/Volumes/workspace/imdb_project/raw_imdb_files/stream_checkpoints_metrics_final/cp`
- Alerts: `dbfs:/Volumes/workspace/imdb_project/raw_imdb_files/stream_checkpoints_alerts_final/cp`

**Output Tables:**
- Metrics table (Complete mode) - All aggregations updated
- Alerts table (Append mode) - New alerts only

**Format:** Delta Lake for ACID transactions and time travel capabilities

---

## Installation & Setup

### Prerequisites
- Python 3.8+
- Apache Spark 3.x
- Databricks account

### Quick Start

1. **Download IMDB Datasets** from https://datasets.imdbws.com/
2. **Upload to Databricks** volume: `workspace.imdb_project.raw_imdb_files`
3. **Run Notebook** cells sequentially


### Memory Requirements
- Minimum: 16GB RAM
- Recommended: 32GB RAM
- Databricks: Standard_DS3_v2 or larger cluster

---

## Key Results

### Batch Analysis
- Successfully processed 100M+ records across 7 datasets
- Identified complete demographic profile of entertainment professionals
- Documented film history timeline from 1800s to present
- Cataloged 28 distinct genres
- Found highest-rated content with statistical significance

### Stream Processing
-  Tracked 5 distinct IMDB entities
-  Calculated real-time metrics in 2-minute windows
-  Implemented alert detection with custom logic
-  Stored results in separate Delta tables
-  Demonstrated exactly-once processing with checkpointing

---

## Key Technical Decisions

### 1. Data Type Handling
Implemented safe casting functions to handle IMDB's `\N` null notation and invalid numeric entries, preventing schema inference errors.

### 2. Large File Processing
Used explicit schema definitions and error handling for datasets exceeding Spark's default limits, with fallback to manual loading when necessary.

### 3. Stream Simulation
Used Spark's rate source instead of direct Wikimedia connection due to Databricks network constraints, maintaining identical logical structure.

### 4. Unity Catalog Integration
Implemented fully qualified table names and proper checkpoint configuration for modern Databricks requirements.

---

## Repository Structure

```
imdb-big-data-project/
├── README.md                    # This file
├── project_BD.ipynb            # Main notebook
└── data/                       # IMDB datasets (not in repo)
```

**Note:** IMDB datasets (>10GB) are not included in repository and must be downloaded separately.

---

## Troubleshooting

**File Too Large Error:** Increase driver memory or load manually via Databricks UI

**Schema Inference Failures:** Handled automatically by safe casting functions in Cell 1

**Permission Errors:** Grant necessary Unity Catalog permissions for table creation

**Checkpoint Issues:** Clear checkpoint directories if corruption occurs

---


## Acknowledgments

- **IMDB** for open datasets
- **Apache Spark Community** for distributed computing framework
- **Databricks** for cloud analytics platform
- **Wikimedia Foundation** for EventStreams documentation
- **Course Instructor** (joe@adaltas.com) for project guidance

---

**License:** Educational purposes only. IMDB datasets subject to non-commercial use licensing.