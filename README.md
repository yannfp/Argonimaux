# Argonimaux

A desktop application to visualize the movements of marine animals and buoys around Europe, built on CNES satellite tracking data from 2000 to 2023.

**🏆 Regional Prize for Creativity — NSI Trophy 2023**

---

## Origin

The idea came from a class of 6th graders at our school. They were studying marine biodiversity and wanted a way to track specific turtles, sharks, and buoys near France — the existing CNES tool covered the whole world and was hard to navigate for their use case. Their teacher reached out to our Terminale NSI class, and three of us took it on.

---

## What it does

- Displays a map of Europe divided into a coordinate grid
- Colour-codes each zone based on how many tracked animals have passed through it (green → orange → red → dark red)
- Lets the user click a zone to see which animals have been recorded there
- Renders the full trajectory of a selected animal using Folium (interactive HTML map)
- Pulls all data from a local SQLite database populated from CNES `.txt` files

---

## Stack

- **Python** — tkinter (UI), PIL (image handling), Folium (map rendering), SQLite3 (database)
- **SQLite** — stores all animal coordinates extracted from CNES data files
- **Folium** — generates interactive HTML maps with trajectory polylines

---

## Architecture

```
CNES .txt files
    └── extractionCSV.py     — parses raw data into lists
    └── gestionBD.py         — inserts records and manages the SQLite DB

Argonimaux.db (SQLite)
    └── animaux table        — coordinates per timestamp
    └── liens table          — maps animal name + type to its ID

tkinter.py                   — main window, grid, click detection, combobox
folium.py                    — renders selected animal trajectory to carte.html
```

---

## Team

Built in three weeks by a team of three students from Lycée Le Ferradou (Toulouse) for the NSI Trophy 2023:

| Name | Role |
|------|------|
| **Yann Fernandez Puig** | Data management — CNES file extraction and SQLite database |
| **Lucas Michalet** | UI — tkinter window, map import, coordinate grid in degrees |
| **Jules Turchi** | Trajectory display — Folium polylines and zone colour logic |

Supervised by Jérôme Bussy and Francis Ceuille (NSI teachers).

---

## Setup

**Requirements:**

```bash
pip install tk folium Pillow
```

**First run only** — uncomment line 52 in `tkinter.py` to initialise the database tables:

```python
gestion.creation(connection)
```

Then run:

```bash
python tkinter.py
```

> Re-comment the `creation()` call before running again — it only needs to run once.

---

## Data

Animal tracking data comes from the [CNES Argonautica programme](https://enseignantsmediateurs.cnes.fr/fr/projets/argonautica/argonimaux). The `.txt` files need to be downloaded manually and placed in the project directory.

Tracked animals include turtles (Anna Antimo, Ashoka, Bambi, Rosa, Vita…), buoys (ChildOceans, Coris, Méduse…), and sharks (Anna Pèlerine).

---

## Known limitations

- Map dimensions are hardcoded — the pixel-to-degree conversion only works with the included `openstreetmap.png` at its exact dimensions
- Data files must be downloaded and placed manually (no API integration yet)
- The interface is functional but minimal
