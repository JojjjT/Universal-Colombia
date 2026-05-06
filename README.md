# 🎵 NoSQL Data Lake — Online Record Store
**Universal Studios Colombia | Practical Workshop on NoSQL Databases with MongoDB**

---

## 📋 Project Overview

This repository contains the complete implementation of a **NoSQL Data Lake** for an online record store, developed as a practical workshop for the NoSQL Databases course at Universidad Externado de Colombia. The project models a music catalog using **MongoDB Atlas**, where each album is a **collection** and each song is a **document**.

**Tech Stack:** Python 3.11 · PyMongo 4.17.0 · MongoDB Atlas · Rich 15.0.0 · Jupyter Notebook

---

## 🎤 Selected Artists and Albums

| Artist | Album | Year | Genre |
|---------|-------|-----|--------|
| Lady Gaga | The Fame | 2008 | Dance-pop / Electropop |
| Diomedes Díaz | El Regreso del Cóndor | 1992 | Vallenato |
| BTS | Map of the Soul: 7 | 2020 | K-Pop / Hip-Hop |
| Michael Jackson | Thriller | 1982 | Pop / R&B / Funk |
| Queen | A Night at the Opera | 1975 | Progressive Rock |
| Ivy Queen | Diva | 2003 | Reggaeton / Latin Hip-Hop |

---

## 🗂️ Repository Structure

```
main/                              ← Task 1: Initial Database
├── Taller_NoSQL_Universal_Studios.ipynb
├── images/
│   ├── lady_gaga_the_fame.jpg
│   ├── diomedes_el_regreso_del_condor.jpg
│   ├── bts_map_of_the_soul_7.jpg
│   ├── michael_jackson_thriller.jpg
│   ├── queen_a_night_at_the_opera.jpg
│   └── ivy_queen_diva.jpg
├── disquera_tarea1.csv
└── bson_tarea1/
    ├── lady_gaga_the_fame.json      ← complete collection
    ├── lgf_001.json … lgf_005.json  ← individual documents
    └── [other collections and documents]

← Task 2: Song Insertion
│ disquera_tarea2.csv
├──  bson_tarea2/

 ← Task 3: Deletions
│ disquera_tarea3.csv
└──  bson_tarea3/
```

---

## 📐 Data Model

### Modeling Rules
- Each **album** → a **collection** in MongoDB
- Each **song** → a **document** within its collection
- Every album and song has a unique `_id` field

### Required Fields per Document (Song)

```json
{
  "_id": "lgf_001",
  "title": "Just Dance",
  "release_year": 2008,
  "artist": "Lady Gaga",
  "cover_image_id": "images/lady_gaga_the_fame.jpg"
}
```

### Optional Fields Added

| Field | Type | Description |
|-------|------|-------------|
| `genre` | string | Musical genre of the song |
| `duration_sec` | integer | Duration in seconds |
| `track_number` | integer | Track number within the album |
| `popularity` | integer | Popularity score (0–100) |

---

## 🎯 Task 1 — Initial Database Creation
**Branch:** `main`

Six collections were created in MongoDB Atlas (database `BD_DisqueraUniversal`), one per artist/album. Each collection was loaded with the **5 most representative songs** of the selected album using `insert_many()`. Before each insertion, `collection.drop()` is executed to ensure idempotency.

### ✅ Connection Output

```
✅ Pinged your deployment. You successfully connected to MongoDB!
🗄️  Active database: BD_DisqueraUniversal
✅ Data for the 6 albums defined correctly.
```

### ✅ Insertion Output

```
🚀 Inserting albums into MongoDB...

📀 Lady Gaga — The Fame: 5 songs inserted into 'lady_gaga_the_fame'
📀 Diomedes Díaz — El Regreso del Cóndor: 5 songs inserted into 'diomedes_diaz_el_regreso_del_condor'
📀 BTS — Map of the Soul: 7: 5 songs inserted into 'bts_map_of_the_soul_7'
📀 Michael Jackson — Thriller: 5 songs inserted into 'michael_jackson_thriller'
📀 Queen — A Night at the Opera: 5 songs inserted into 'queen_a_night_at_the_opera'
📀 Ivy Queen — Diva: 5 songs inserted into 'ivy_queen_diva'
```

---

## 🎯 Task 2 — Album Updates
**Branch:** `updates`

Two **lesser-known songs** per album were identified and added. The insertion process uses a safe pattern: before inserting each document, it verifies if the `_id` already exists, avoiding `DuplicateKeyError` during re-executions.

### ✅ Post-Task 2 Integrity Verification Table

```
         Collections Summary — Post Task 2
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┓
┃ Collection                    ┃ Artist          ┃ Album                ┃ Total Songs    ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━┩
│ lady_gaga_the_fame            │ Lady Gaga       │ The Fame             │       7        │
│ diomedes_diaz_el_regreso...   │ Diomedes Díaz   │ El Regreso del Cóndor│       7        │
│ bts_map_of_the_soul_7         │ BTS             │ Map of the Soul: 7   │       7        │
│ michael_jackson_thriller      │ Michael Jackson │ Thriller             │       7        │
│ queen_a_night_at_the_opera    │ Queen           │ A Night at the Opera │       7        │
│ ivy_queen_diva                │ Ivy Queen       │ Diva                 │       7        │
└───────────────────────────────┴─────────────────┴──────────────────────┴────────────────┘
```

---

## 🎯 Task 3 — Record Deletion
**Branch:** `deletions`

Two types of deletions were performed: the **entire Ivy Queen collection** (`ivy_queen_diva`) using `collection.drop()`, and **2 songs per album** across the remaining 5 using `delete_many()` with the `$in` operator.

### ✅ Final Status Table — Post Task 3

```
                          📊 Final Status — Post Task 3
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┓
┃ Collection                    ┃ Artist          ┃ Album                ┃ Remaining Songs   ┃    Status    ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━┩
│ lady_gaga_the_fame            │ Lady Gaga       │ The Fame             │          5        │  ✅ Active   │
│ diomedes_diaz_el_regreso...   │ Diomedes Díaz   │ El Regreso del Cóndor│          5        │  ✅ Active   │
│ bts_map_of_the_soul_7         │ BTS             │ Map of the Soul: 7   │          5        │  ✅ Active   │
│ michael_jackson_thriller      │ Michael Jackson │ Thriller             │          5        │  ✅ Active   │
│ queen_a_night_at_the_opera    │ Queen           │ A Night at the Opera │          5        │  ✅ Active   │
│ ivy_queen_diva                │ Ivy Queen       │ Diva                 │          0        │ 🗑️ Deleted  │
└───────────────────────────────┴─────────────────┴──────────────────────┴───────────────────┴──────────────┘
```

> **Note:** The CSV/BSON export cell for Task 3 generated a `PermissionError` when attempting to overwrite `disquera_tarea3.csv` while it was open in another process. All MongoDB operations (deletions) were executed and verified correctly; the error was strictly related to local disk writing.

---

## 🏗️ Design Decisions

*   **Collection naming in snake_case without accents** (e.g., `lady_gaga_the_fame`): Prevents encoding issues across operating systems and facilitates code referencing.
*   **Manual string `_id`** (e.g., `lgf_001`): Makes documents self-descriptive, simplifies debugging, and produces readable CSV/BSON files without needing to convert `ObjectId`.
*   **JSON as a substitute for binary BSON**: Real binary BSON requires `mongodump` (CLI). From Python/Jupyter, equivalent JSON files are exported; the structure is identical, only the binary encoding differs.
*   **Idempotent Task 1**: Using `collection.drop()` before `insert_many()` allows for re-running the notebook without accumulating duplicates.

---

## 💡 Findings and Lessons Learned

*   **Schema flexibility requires explicit discipline**: MongoDB does not enforce structure, but for CSV export to work correctly, a consistent schema had to be defined and respected within the Python code.
*   **`drop()` vs `delete_many()`**: These are conceptually different operations. `drop()` removes the entire collection and its indexes; `delete_many()` removes documents but preserves the collection and its metadata.
*   **The `Rich` library significantly improves output readability**: Tables generated with `rich.table.Table` made visual verification of the state of each collection much easier throughout the three tasks.

---

## 🚀 Instructions to Replicate

1.  **Clone the repository**: `git clone [https://github.com/JojjjT/Universal-Colombia.git](https://github.com/JojjjT/Universal-Colombia.git)`
2.  **Install dependencies**: `pip install pymongo rich`
3.  **Configure connection string**: Replace `MONGO_URI` in the notebook with your own MongoDB Atlas credentials.
4.  **Run the notebook**: Execute cells sequentially from top to bottom.

---

## 👥 Team

*   **Joshua Trujillo**: Development, documentation, and repository management.
*   **Juan Amezquita**: Data modeling and testing.
*   **Valeria Villamil**: Repository upload and delivery.
*   **Juan Navarrete**: Review and data integrity.

*Universidad Externado de Colombia — NoSQL Databases*
