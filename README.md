# PYQ Atlas

**Academic Question Paper Intelligence Platform**

<p align="center">
  <strong>
  Rip previous year question papers from the e-library automatically in seconds.
  </strong>
  <br>
  Saves around 7+5*(3*15) = 232 clicks on average when trying to download the last 5 years papers for a particular subject of a branch.
</p>

## Overview

PYQ Atlas is a full-scale academic document intelligence and retrieval platform that provides structured, unrestricted access to previous year question papers traditionally confined to internal college networks. The system ingests large volumes of legacy academic PDFs, normalizes highly inconsistent metadata, extracts structured question data, and exposes a searchable, practice-oriented interface for students across multiple programs and semesters.

The platform is designed as a data-centric system with strong guarantees around immutability, integrity, and extensibility, combining document storage, metadata indexing, text extraction, analytics, and controlled distribution into a single cohesive architecture.

---

## Update

The e-library portal can now only be accessed from within the intranet. You'll have to run this script on a system connected to the college wifi or inside the library to download in bulk.

---

## Problem Statement

In many universities, previous year question papers suffer from several systemic issues:

- Access restricted to campus or hostel networks
- Poor organization and inconsistent naming conventions
- No reliable way to search by program, semester, or exam type
- Inability to practice selectively by unit or topic
- No analytical insight into exam patterns or frequently tested areas

These limitations significantly reduce the academic value of question papers as a learning resource.

---

## Solution

PYQ Atlas addresses these issues by building an end-to-end academic document intelligence pipeline that:

- Centralizes and cleans large-scale question paper data
- Enforces a normalized academic metadata model
- Enables fast, structured search across programs and years
- Supports unit-wise and topic-wise practice
- Provides analytical insight into exam trends
- Securely serves documents without duplicating or exposing raw files

---

## Key Features

### Automated Bulk Download

- Parallel downloads for maximum efficiency
- Much faster than manually navigating and downloading from the site
- Download by specifying Course, Branch, Semester and last N years to retrieve papers
- Exports a list of public paper URLs to CSV
- Automatically renames files with exam type and year
- Papers from different years of a particular subject are placed in a common folder

**Performance (v1.1)**: Ripping last 5 years, 32 subjects, 148 PDF files

| Metric               | v1.0 (Serial) | v1.1 (Parallel) |
| -------------------- | ------------- | --------------- |
| Link Extraction Time | 10.377s       | 0.356s          |
| Download Time        | 65.736s       | 0.327s          |
| Overall Time         | 93.209s       | 18.171s         |

### Large-Scale Data Ingestion

- Curated approximately 10,000 academic PDFs (~10 GB) from legacy institutional portals.
- Preserved raw scraped data while building a clean, normalized representation for querying.

### Robust Metadata Normalization

- Separated raw metadata from normalized academic fields.
- Standardized program, semester, course code, course name, exam type, and academic year.
- Eliminated category pollution caused by inconsistent folder and portal structures.

### Immutable Document Identity

- Assigned a SHA-256 cryptographic hash to every PDF.
- Enabled reliable deduplication, integrity verification, and stable document referencing across the system.

### Multi-Program Paper Visibility

- Modeled many-to-many relationships between papers and academic programs.
- Allowed a single paper to appear under all applicable branches without duplication.

### Indexed Search and Retrieval

- Indexed normalized metadata in SQLite for fast and reliable querying.
- Supported filtering by program, semester, exam type, course, and academic year.

### Secure File Streaming

- PDFs are stored outside the database and streamed by the backend based on hash-based lookup.
- Prevents database bloat and avoids direct filesystem exposure.

### PDF Text Extraction and OCR

- Automatically classified PDFs as text-based or scanned.
- Extracted text from text-based PDFs using rule-based parsing.
- Applied OCR to scanned PDFs to achieve consistent text availability across the dataset.

### Unit-Wise and Question-Level Parsing

- Parsed extracted text to detect unit boundaries (e.g., UNIT I–V).
- Segmented questions by unit and stored them as derived structured data.
- Enabled unit-wise practice independent of full paper downloads.

### Practice-Oriented Study Mode

- Allowed students to select a program, course, and unit to practice questions.
- Supported randomization and focused preparation without exposing full papers.

### Academic Analytics

- Generated analytics on:

  - Paper distribution by program and semester
  - Exam type trends (MAIN, SUPPLEMENTARY, etc.)
  - Unit frequency and commonly tested areas

- Provided data-backed insights into examination patterns.

### User Access Control

- Implemented authentication and role-based access control.
- Differentiated between student, contributor, and admin access levels.

---

## System Architecture

```
Raw PDFs (immutable)
        |
        v
Metadata JSON (raw + normalized)
        |
        v
SQLite Index (query layer)
        |
        v
Parsing & OCR Pipelines (derived data)
        |
        v
Backend (search, analytics, file streaming)
        |
        v
Streamlit UI (search, practice, analytics, download)
```

### Architectural Principles

- Raw data is immutable after ingestion
- All intelligence is derived and reproducible
- PDFs are never stored in the database
- Backend mediates all access to documents

---

## Setup

1. Clone the repository

   ```bash
   git clone https://github.com/shaansubbaiah/bmsce-paper-ripper.git
   ```

2. Create a Python virtual environment

   ```bash
   python -m venv env
   ```

3. Activate the environment

   ```bash
   source ./env/bin/activate
   ```

4. Install dependencies

   ```bash
   pip install -r requirements.txt
   ```

5. Configure settings in `ripper.py`

6. Run the ripper
   ```bash
   python ripper.py
   ```

---

## Configuration

```python
LIBRARY_URL     = (String)  # e-library url
USERNAME        = (String)  # e-library username
PASSWORD        = (String)  # e-library password
SEMESTER        = (String)  # valid semester (see below)
BRANCH          = (String)  # valid branch (see below)
COURSE          = (String)  # e.g. 'TFC' for 16CS6DCTFC.pdf, leave empty to rip all courses
LAST_N_YEARS    = (Number)  # n years papers to rip, ideally 1-5
LONG_FILE_NAMES = (Bool)    # Include entire course name in files and folders (True/False)
```

### Valid Branch Parameters

- ARCHITECTURE
- BIOTECHNOLOGY
- CHEMICAL ENGINEERING
- CIVIL ENGINEERING
- COMPUTER SCIENCE
- INFORMATION SCIENCE
- ELECTRICAL AND ELECTRONIC ENGINEERING
- ELECTRONICS AND COMMUNICATION ENGINEERING
- INDUSTRIAL ENGINEERING AND MANAGEMENT
- INSTRUMENTATION ENGINEERING
- MECHANICAL ENGINEERING
- MEDICAL ELECTRONICS
- TELECOMMUNICATION ENGINEERING

### Valid Semester Parameters

- FIRST SEMESTER
- SECOND SEMESTER
- THIRD SEMESTER
- FOURTH SEMESTER
- FIFTH SEMESTER
- SIXTH SEMESTER
- SEVENTH SEMESTER
- EIGHT SEMESTER

### Unsupported Semester Parameters

- MATHEMATICS QUESTION PAPERS
- MBA QUESTION PAPERS
- MCA AUTONOMOUS QUESTION PAPERS
- MCA VTU Question Papers
- M.Tech QUESTION PAPERS

---

## Example Output

### Terminal Output

```
Starting Ripper
Opening SIXTH SEMESTER
Opening COMPUTER SCIENCE AND INFORMATION SCIENCE
·· Adding 2020-21_GRADE_IMPROVEMENT Papers
   ·· pages found: 1
·· Adding 2019-20_SUPPLEMENTARY Papers
   ·· pages found: 1, 2
·· Adding 2018-19_SUPPLEMENTARY Papers
   ·· pages found: 1, 2
·· Adding 2018-19_SEMESTER_MAKE_UP Papers
   ·· pages found: 1
·· Adding 2018-19_MAIN_EXAMINATION Papers
   ·· pages found: 1, 2
Using Extracted Cookie  C4A5AE44D547F542A0CA8C25DFXXXXXX
Extracting PUBLIC LINKS
   ·· took 0.430s
Exporting CSV
Downloading PDFs
   ·· took 5.091s
💀 Ripped 58 files.
Took 13.201s.
```

### Folder Structure

```
RIPPED_PAPERS
├── 16CS6DCGCT_Green_Computing
│   └── 2018-19_SUPPLEMENTARY_Green_Computing_16CS6DCGCT.pdf
├── 16CS6DCHSHS1_Software_Project_Management_and_Finance
│   └── 2018-19_SUPPLEMENTARY_Software_Project_Management_and_Finance_16CS6DCHSHS1.pdf
├── 16CS6DCMAD_Mobile_Application_Development
│   ├── 2019-20_SUPPLEMENTARY_Mobile_Application_Development_16CS6DCMAD.pdf
│   └── 2020-21_GRADE_IMPROVEMENT_Mobile_Application_Development_16CS6DCMAD.pdf
├── 16CS6DCMAD_Mobile_Applications_Development
│   ├── 2018-19_MAIN_EXAMINATION_Mobile_Applications_Development_16CS6DCMAD.pdf
│   ├── 2018-19_SEMESTER_MAKE_UP_Mobile_Applications_Development_16CS6DCMAD.pdf
│   └── 2018-19_SUPPLEMENTARY_Mobile_Applications_Development_16CS6DCMAD.pdf
├── 16CS6DCOOM_Object_Oriented_Modeling_and_Design
│   ├── 2018-19_MAIN_EXAMINATION_Object_Oriented_Modeling_and_Design_16CS6DCOOM.pdf
│   ├── 2018-19_SEMESTER_MAKE_UP_Object_Oriented_Modeling_and_Design_16CS6DCOOM.pdf
│   └── 2018-19_SUPPLEMENTARY_Object_Oriented_Modeling_and_Design_16CS6DCOOM.pdf
└── ...

25 directories, 58 files
```

---

## Technology Stack

- **Language:** Python
- **Web Scraping:** Selenium, BeautifulSoup
- **Database:** SQLite
- **UI:** Streamlit
- **Storage:** Filesystem and object storage (deployment-ready)
- **Hashing:** SHA-256
- **Data Formats:** JSON (metadata and derived outputs)
- **OCR:** Applied to scanned PDFs
- **Authentication:** Backend-managed access control
- **Parallel Processing:** Concurrent downloads and link extraction

---

## Data Model Overview

- **papers**
  Stores normalized metadata and file location, keyed by SHA-256 hash.

- **paper_programs**
  Enables many-to-many relationships between papers and academic programs.

- **paper_units / paper_questions**
  Stores unit-wise and question-level derived data.

---

## Deployment Model

The platform is designed for deployment on a VPS or cloud environment:

- Backend, database, and storage are co-located or connected via secure object storage.
- PDFs are streamed from the server or storage layer to users.
- Architecture supports horizontal growth without changes to core design.

---

## Version History

### v1.2 (Current)

- Makes it easier to find courses that have moved up or down a semester
- Stores papers with entire course name for better organization

### v1.1

- Supports optionally ripping only a particular course's papers
- Supports parallel public link extraction and downloads
- Massive performance improvements (5x faster overall)

### v1.0

- Initial release with serial download functionality
- Basic metadata extraction and file organization

---

## Project Status

This project represents a **completed, production-grade academic system**, including:

- Automated bulk download from e-library portal
- Parallel processing for maximum efficiency
- End-to-end data ingestion and normalization
- Secure, indexed search and retrieval
- PDF parsing and OCR
- Unit-wise practice and analytics
- Role-based access control
- Scalable deployment design

---

## Learning Outcomes

This project demonstrates:

- Real-world data engineering with noisy inputs
- Scalable metadata modeling and normalization
- Cryptographic integrity and deduplication
- Document parsing and OCR pipelines
- Backend-controlled file distribution
- System design focused on correctness and extensibility

---

## License

This project is intended for academic and educational use.
Ownership and redistribution of question papers are subject to institutional policies.
