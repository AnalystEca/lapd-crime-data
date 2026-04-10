# Istruzioni per Cowork — Setup struttura progetto LAPD Crime Analysis

Di seguito i passaggi da eseguire nella cartella del progetto. La cartella attualmente contiene due file CSV:
- `csv-2010-2019.csv`
- `csv-2020-2024.csv`

(se i nomi esatti sono leggermente diversi, adattali di conseguenza ma mantieni la stessa logica)

E un notebook Jupyter in cui sono già stati caricati i due dataframe.

---

## 1. Creare la struttura di cartelle

Dentro la cartella principale del progetto, creare le seguenti sottocartelle:

```
data/
├── raw/
└── processed/
notebooks/
src/
```

## 2. Spostare i file CSV

Spostare entrambi i file CSV (`csv-2010-2019.csv` e `csv-2020-2024.csv`) dalla cartella principale dentro `data/raw/`.

## 3. Spostare il notebook

Spostare il notebook Jupyter esistente dentro la cartella `notebooks/` e rinominarlo in `01_data_loading.ipynb`.

## 4. Creare il file `.gitignore`

Nella cartella principale del progetto, creare un file chiamato `.gitignore` con il seguente contenuto esatto:

```
# Dati
data/raw/
data/processed/
*.csv

# Jupyter
.ipynb_checkpoints/
*/.ipynb_checkpoints/*

# Python
__pycache__/
*.pyc
.venv/
venv/
env/

# OS / Editor
.DS_Store
Thumbs.db
.vscode/
.idea/
```

## 5. Creare il file `README.md`

Nella cartella principale del progetto, creare un file chiamato `README.md` con il seguente contenuto:

```markdown
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
```

## 6. Creare il file `requirements.txt`

Nella cartella principale del progetto, creare un file chiamato `requirements.txt` con il seguente contenuto:

```
pandas
numpy
matplotlib
seaborn
jupyter
```

---

## Struttura finale attesa

Al termine delle operazioni, la cartella del progetto deve avere questo aspetto:

```
nome-progetto/
├── data/
│   ├── raw/
│   │   ├── csv-2010-2019.csv
│   │   └── csv-2020-2024.csv
│   └── processed/
├── notebooks/
│   └── 01_data_loading.ipynb
├── src/
├── .gitignore
├── README.md
└── requirements.txt
```

## Nota importante

**Non eseguire alcun comando git.** Il setup del repository git verrà gestito separatamente in un secondo momento.
