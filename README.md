# Argonimaux

A desktop application to visualize the movements of marine animals and buoys around Europe, built on CNES satellite tracking data from 2000 to 2023.

**🏆 Regional Prize for Creativity — NSI Trophy 2023**

---

## Origin

The idea came from a class of 6th graders at our school. They were studying marine biodiversity and wanted a way to track specific turtles, sharks, and buoys near France — the existing CNES tool covered the whole world and was too broad for their use case. Their teacher reached out to our Terminale NSI class, and three of us took it on.

---

## What it does

- Displays a map of Europe divided into a coordinate grid
- Colour-codes each zone based on how many tracked animals have passed through it (green → orange → red → dark red)
- Lets the user click a zone to see which animals have been recorded there
- Renders the full trajectory of a selected animal in an interactive HTML map (Folium), with date markers placed every 75 data points
- Links to the project's Instagram page and the original CNES data source

---

## Stack

- **Python** — tkinter (UI), PIL (image handling), Folium (map rendering), SQLite3 (database)
- **SQLite** — stores all animal coordinates extracted from CNES `.txt` files
- **Folium** — generates an interactive `carte.html` with trajectory polylines and date markers

---

## Architecture

```
main.py                          — entry point

CNES .txt files (not included)
    └── GestionDonnees/
            extractionCSV.py     — parses raw .txt data into lists
            gestionBD.py         — manages the SQLite DB (creation, inserts, queries)

Argonimaux.db (generated on first run)
    └── animaux table            — one row per GPS fix (id, num, class, date, time, lat, lon)
    └── liens table              — maps animal name + type to its tracking number

Affichage/
    tkinter.py                   — main window, grid rendering, click detection, combobox, buttons
    openstreetmap.png            — base map image
    logoPage.png                 — app logo
    logo_instagram.jpg           — Instagram button icon
    Onest-Regular.ttf            — custom font
    icon.ico                     — window icon
```

---

## Tracked animals

| Type | Animals |
|------|---------|
| Turtles | Anna Antimo, Antioche, Ashoka, Bambi, Bouton d'or, Chacahe, Danae, Delta, Domino, Laura, Moana, Muse, Neus, Ninja-Balaeres, Rosa, Tikaf, Tom, Vita, Zamzam |
| Sharks | Anna Pelerine, Gary, Marie B |
| Buoys | ChildOceans, Coris, Coris 2, Zelisca, Pegase 2019, Phebus, VenusExpe, Meduse |

---

## Team

Built by a team of three students from Lycée Le Ferradou (Toulouse) for the NSI Trophy 2023:

| Name | Role |
|------|------|
| **Yann Fernandez Puig** | Data management — CNES file extraction and SQLite database |
| **Lucas Michalet** | UI — tkinter window, map import, coordinate grid in degrees |
| **Jules Turchi** | Trajectory display — Folium polylines and zone colour logic |

Supervised by Jérôme Bussy and Francis Ceuille (NSI teachers).

---

## Setup

**Install dependencies:**

```bash
pip install folium Pillow
```

**Download the CNES data files** from the [Argonautica programme](https://enseignantsmediateurs.cnes.fr/fr/projets/argonautica/argonimaux) and place the `.txt` files in the root of the project (one file per animal, named exactly as listed in the tracked animals table above).

**Run:**

```bash
python main.py
```

The database is created automatically on first run.

---

## Known limitations

- Map dimensions are hardcoded — the pixel-to-degree conversion only works with the included `openstreetmap.png` at its exact dimensions
- Data files must be downloaded manually from CNES (no API integration)
- Windows only (`fenetre.state("zoomed")` and `.iconbitmap()` are Windows-specific)
