# Coinbase Exporter

[![License: GPLv3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

Ein **Coinbase Exporter** für Accounts und Transaktionen über die **Coinbase API v2**.  
Das Tool erstellt **CSV- und SQLite-Exporte** sowie einen **Jahresreport (CSV & PDF)** für das Finanzamt nach **FIFO-Prinzip** mit Berücksichtigung der **1-Jahres-Haltefrist** in Deutschland.

---

## Features ✨

- 📥 **Import von Coinbase-Daten** über die API (Accounts & Transaktionen)  
- 📊 **CSV- und SQLite-Export** der Rohdaten  
- 📑 **Automatisch formatierter Jahresreport (CSV + PDF)** mit Präfix `YYYYMMDD_Coinbase_Report`  
- 🔄 **FIFO-Logik** zur Ermittlung steuerpflichtiger / steuerfreier Verkäufe  
- 🗓️ **Jahresfilter mit `--year`** (aktuelles Jahr: 01.01.–heute, Vorjahre: 01.01.–31.12.)  
- ⚖️ **Auswertung der 1-Jahres-Frist** nach deutschem Steuerrecht  
- 🪵 **Logging** (RotatingFileHandler, optional mit `-v` auch auf Konsole)  
- 🔒 **Sichere Authentifizierung** per API-Key/Secret aus `.env`-Datei oder Umgebungsvariablen  
- 🐍 **Python 3.9+ kompatibel**

---

## Installation ⚙️

1. Repository klonen:
   ```bash
   git clone https://github.com/deinuser/coinbase_exporter.git
   cd coinbase_exporter
   ```

2. Abhängigkeiten installieren:
   ```bash
   pip install -r requirements.txt
   ```

3. `.env`-Datei anlegen (oder die vorhandene Vorlage `coinbase_exporter.env` verwenden):
   ```env
   # coinbase_exporter.env
   CB_API_KEY=dein_api_key
   CB_API_SECRET=dein_api_secret
   CB_OUT_DIR=./coinbase_exports
   ```

---

## Nutzung 🚀

```bash
# Jahresreport für 2024
python3 coinbase_exporter.py --year 2024 --csv

# Laufendes Jahr (2025: 01.01.–heute), inkl. SQLite
python3 coinbase_exporter.py --year 2025 --csv --sqlite -v
```

### Wichtige Argumente
| Argument        | Beschreibung |
|-----------------|--------------|
| `--year <YYYY>` | Kalenderjahr für Report. Aktuelles Jahr = 01.01.–jetzt. |
| `--csv`         | Rohdaten als `accounts.csv` & `transactions.csv` exportieren. |
| `--sqlite`      | Rohdaten in `coinbase.sqlite` speichern. |
| `-v`            | Log-Ausgabe auch auf der Konsole anzeigen. |
| `--out-dir`     | Ausgabeordner (Default: `./coinbase_exports`). |

---

## Ausgaben 📂

- **Rohdaten (optional mit `--csv`)**
  - `accounts.csv`
  - `transactions.csv`
- **SQLite (optional mit `--sqlite`)**
  - `coinbase.sqlite`
- **Report (bei gesetztem Zeitraum, z. B. `--year`)**
  - `YYYYMMDD_Coinbase_Report.csv`
  - `YYYYMMDD_Coinbase_Report.pdf` (formatiert, mit FIFO-Analyse)
- **Logfiles**
  - `coinbase_exporter.log` (Rotating, max. 1 MB × 3)

---

## Beispiel: PDF-Report 📑

Der PDF-Report enthält:
- Zeitraum des Reports
- Übersicht aller Accounts & Währungen
- Tabelle aller Verkäufe (FIFO-basiert)
  - steuerpflichtige vs. steuerfreie Mengen
  - Gewinne nach steuerpflichtig / steuerfrei
  - Kennzeichnung, ob **komplett steuerfrei**

---

## Hinweise ⚖️

- Dieses Tool liefert **keine Steuerberatung**.  
- Die Daten sollen helfen, **Nachweise für das Finanzamt** vorzubereiten.  
- Prüfe deine Ergebnisse mit einem Steuerberater oder spezialisierten Tools.  

---

## Entwicklung 🛠️

### Linting
```bash
flake8 coinbase_exporter.py
```

### Tests
(Geplant: Unit Tests für FIFO-Logik)

---

## Lizenz 📄

Dieses Projekt steht unter der **GNU General Public License v3.0**.
