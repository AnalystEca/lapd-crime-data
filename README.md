# LAPD Crime Analysis (2010–2024)

Analisi esplorativa dei crimini registrati dal Los Angeles Police Department
dal 2010 al 2024, basata sui dataset open data di data.lacity.org.

## Dati
I CSV non sono inclusi nel repo. Scaricali da data.lacity.org:
- Crime Data from 2010 to 2019
- Crime Data from 2020 to 2024

E posizionali nella cartella `data/raw/`.

## Struttura
- `data/raw/` — dataset originali (non versionati)
- `data/processed/` — dataset puliti (non versionati)
- `notebooks/` — notebook di analisi
- `src/` — funzioni riutilizzabili

## Setup
pip install -r requirements.txt
