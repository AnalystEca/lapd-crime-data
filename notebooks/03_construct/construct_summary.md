# Construct — Summary

**Stato**: In corso
**Data inizio**: 23-07-2026
**Data chiusura**: 

## Obiettivi della fase

La fase Construct si concentra sul raffinamento dei modelli predittivi
costruiti nella fase Analyze (Blocchi 6-7):

- **Random Forest** (Q7.2): ottimizzazione degli iperparametri per
  migliorare le metriche di classificazione del modello di clearance
- **Prophet** (Q7.1): aggiunta di regressori esterni (festività USA)
  e tuning del `changepoint_prior_scale` per ridurre la sovrastima
  sistematica

## Approccio

### Perché ottimizzare

Il modello baseline del Blocco 7 usa parametri di default:
- Random Forest: `n_estimators=100`, `max_depth=10`, `class_weight='balanced'`
- Prophet: configurazione standard senza festività

L'ottimizzazione degli iperparametri può migliorare le metriche senza
cambiare l'architettura del modello. Per un progetto di portfolio è
importante documentare questo processo e il trade-off tra qualità
del risultato e costo computazionale.

### Due approcci a confronto

Per il Random Forest testeremo e confronteremo due strategie:

**Approccio B — RandomizedSearchCV + campione 20%**:
- Testa un sottoinsieme casuale di combinazioni di iperparametri (10-20)
- Eseguito su un campione del 20% del dataset (~490.000 campioni)
- Tempo stimato: 15-20 minuti
- Qualità: quasi ottimale (tipicamente entro 1% dal massimo)

**Approccio A — GridSearchCV + dataset completo**:
- Testa tutte le combinazioni possibili di iperparametri
- Eseguito sull'intero dataset (2.447.032 campioni)
- Tempo stimato: ~2 ore
- Qualità: ottimale

**Baseline** (già disponibile dal Blocco 7):
- Parametri di default, nessuna ottimizzazione
- accuracy 71%, recall Cleared 82%, precision Cleared 45%

### Iperparametri da ottimizzare

```python
param_dist = {
    'n_estimators': [100, 200, 300, 500],
    'max_depth': [5, 10, 15, 20, None],
    'min_samples_split': [2, 5, 10, 20],
    'min_samples_leaf': [1, 2, 4]
}
```

### Confronto finale

Al termine produrremo una tabella comparativa con le tre versioni:

| Versione | Strategia | Tempo | Accuracy | Recall Cleared | Precision Cleared |
|----------|-----------|-------|----------|----------------|-------------------|
| Baseline | Default | - | 71% | 82% | 45% |
| B | RandomizedSearchCV + 20% | ~20 min | TBD | TBD | TBD |
| A | GridSearchCV + 100% | ~2 ore | TBD | TBD | TBD |

Obiettivo: documentare il trade-off tempo/qualità e identificare
il punto di equilibrio ottimale per scenari operativi reali.

## Piano di esecuzione

- **Mattina**: Approccio B (RandomizedSearchCV + campione 20%)
- **Pomeriggio**: Approccio A (GridSearchCV + dataset completo, esecuzione lunga)
- **Giorno successivo**: confronto risultati, conclusioni, aggiornamento summary

## Notebook

- `notebooks/03_construct/12_construct_block1.ipynb` — da creare

## Output attesi

- `outputs/12_construct_block1/` — grafici comparativi delle metriche
- Modello Random Forest ottimizzato salvato come file `.pkl`
- Tabella comparativa baseline vs RandomizedSearch vs GridSearch
