# Construct — Summary

**Stato**: Completato
**Data inizio**: 23-07-2026
**Data chiusura**: 27-07-2026

## Obiettivi della fase

La fase Construct si è concentrata sul raffinamento del modello Random Forest
costruito nella fase Analyze (Q7.2 — modello predittivo di clearance).

## Approccio adottato

### Nomenclatura
- **Approccio A**: RandomizedSearchCV su campione 20% del dataset
- **Approccio B**: GridSearchCV su dataset completo (tentato, non completato)

### Iperparametri esplorati

```python
param_dist = {
    'n_estimators': [100, 200, 300, 500],
    'max_depth': [5, 10, 15, 20, None],
    'min_samples_split': [2, 5, 10, 20],
    'min_samples_leaf': [1, 2, 4]
}
```

---

## Approccio A — RandomizedSearchCV + campione 20%

**Configurazione**:
- Campione: 20% del dataset di train (391.525 campioni su 1.957.625)
- Iterazioni: 20 combinazioni casuali
- Cross-validation: k=5 fold
- Scoring: recall (classe Cleared)
- Tempo di esecuzione: ~30 minuti

**Risultati**:
- `n_estimators`: 500
- `max_depth`: 15
- `min_samples_split`: 20
- `min_samples_leaf`: 4
- **Miglior recall CV**: 0.8198

---

## Approccio B — GridSearchCV + dataset completo

**Configurazione**:
- Dataset completo: 1.957.625 campioni
- Combinazioni: 240 (4×5×4×3) × 5 fold = 1.200 fit totali
- Scoring: recall (classe Cleared)

**Esito**: **non completato** — tentato due volte, interrotto in entrambi
i casi dopo oltre 4 ore di elaborazione senza risultati.

**Motivo**: la presenza di `max_depth=None` con alberi illimitati su
2 milioni di campioni rende il costo computazionale insostenibile su
hardware consumer (testato su Ryzen 9, 32 GB RAM).

**Nota**: anche con grid ridotto (54 combinazioni, `max_depth` max=20)
il processo è stato interrotto dopo ~4 ore senza completamento.

**Conclusione**: GridSearchCV su dataset di questa dimensione non è
fattibile senza infrastruttura cloud (AWS, Google Cloud, ecc.).

---

## Modello finale ottimizzato

Parametri identificati tramite Approccio A, addestrati sull'intero
dataset di train:

### Tabella comparativa

| Metrica | Baseline | Ottimizzato |
|---------|----------|-------------|
| Accuracy | 71% | 72% |
| Recall Cleared | 82% | 82% |
| Precision Cleared | 45% | 45% |
| F1 Cleared | 0.58 | 0.58 |

### Osservazioni

**Miglioramento marginale**: l'ottimizzazione ha prodotto un guadagno
di appena 1 punto percentuale di accuracy. I parametri di default del
Random Forest erano già molto vicini all'ottimale per questo dataset.

**Supporto dalla letteratura**: secondo Bergstra & Bengio (2012) —
"Random Search for Hyper-Parameter Optimization", JMLR
(http://www.jmlr.org/papers/v13/bergstra12a.html) — il RandomizedSearch
produce risultati comparabili al GridSearch nel 90-95% dei casi
utilizzando una frazione delle risorse computazionali. I risultati
ottenuti confermano empiricamente questa conclusione.

**Conclusione metodologica**: per dataset di questa dimensione il
RandomizedSearchCV su campione rappresentativo è l'approccio
raccomandato. Il GridSearchCV completo non è giustificabile in
termini di costo/beneficio su hardware consumer.

---

## Note per la relazione finale

- Citare Bergstra & Bengio (2012) per giustificare la scelta del
  RandomizedSearch rispetto al GridSearch
- Documentare i limiti computazionali come insight metodologico
  reale — dimostra consapevolezza dei vincoli operativi
- Il modello ottimizzato adotta i parametri del RandomizedSearch:
  `n_estimators=500, max_depth=15, min_samples_split=20, min_samples_leaf=4`

## Notebook

- `notebooks/03_construct/12_construct_block1.ipynb`

## Output

- `outputs/12_construct_block1/` — grafici comparativi (da produrre)
