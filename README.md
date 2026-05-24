# Medicine Inventory Management System – Project 1

## Overview
A relational database that models a medicine inventory management system.  
Doctors issue prescriptions; pharmacy staff retrieve medicines; bills are generated automatically via triggers.

## Entities
| Table | Description |
|---|---|
| `DISEASE` | Disease catalogue with type classification |
| `DOCTOR` | Physicians with qualification and specialisation |
| `PATIENT` | Patient demographics |
| `MEDICINE` | Medicine details, pricing, dosage |
| `MEDICINE_DISEASE` | Many-to-many: which medicine treats which disease |
| `PRESCRIPTION` | Issued by a doctor for a patient |
| `PRESCRIPTION_MEDICINE` | Many-to-many: medicines and quantities in a prescription |
| `BILL` | Auto-generated bill for each prescription |

## Repo Structure
```
├─ README.md
├─ report.docx              ← Written report
├─ presentation.pptx        ← Slide deck
├─ video_link.txt           ← YouTube / MS Stream URL
├─ sql/
│   ├─ create_tables.sql    ← DDL: tables + views
│   ├─ load_data.sql        ← INSERT test data (≥12 rows/table)
│   ├─ queries.sql          ← Sample retrieval & update queries
│   └─ triggers.sql         ← 5 triggers (auto-bill, expiry guard)
└─ src/
    └─ app.py               ← Bonus: Flask/Python UI
```

## How to Run

### Prerequisites
- MySQL 8.0+ (or MariaDB 10.5+)
- Python 3.9+ with `mysql-connector-python` (for the bonus UI)

### Steps
```bash
# 1. Create the database
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS medicine_db;"

# 2. Create tables, views
mysql -u root -p medicine_db < sql/create_tables.sql

# 3. Load triggers
mysql -u root -p medicine_db < sql/triggers.sql

# 4. Insert test data
mysql -u root -p medicine_db < sql/load_data.sql

# 5. Run sample queries
mysql -u root -p medicine_db < sql/queries.sql
```

### Bonus UI (Flask)
```bash
cd src
pip install flask mysql-connector-python
python app.py
# Open http://localhost:5000
```

## Design Notes
- Bills are created **automatically** by trigger `trg_after_prescription_insert`.
- Bill totals are **recalculated automatically** whenever prescription medicines are inserted, updated, or deleted.
- Trigger `trg_check_medicine_expiry` prevents adding expired medicines to a prescription.
- `MEDICINE_DISEASE` resolves the many-to-many relationship between medicines and diseases.
- `PRESCRIPTION_MEDICINE` stores quantity alongside the join, modelling a weak entity.
