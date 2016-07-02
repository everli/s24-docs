## DISPONIBILITÀ

Disponibilità generica per una consegna
======================
### `/v1/availability`, POST
Il servizio filtra i punti vendita che non soddisfano il test di disponibilità di S24: nel caso non esista un fattorino abilitato all'interno del raggio specificato come parametro il test dà risultato negativo.  
Passare nel payload della richiesta la dimensione del raggio, il centro di massa (coordinate del cliente) e la lista di località (coordinate dei punti vendita) per cui si vuole valutare la disponibilità.  
Nel caso in cui la distanza tra cliente e punto vendita superi la distanza concordata contrattualmente, la richiesta è considerata invalida e viene ritornato un errore di tipo `PRECONDITION FAILED` (`status_code 412`).  
Nel caso in cui invece questa prima condizione venga verificata, la risposta sarà la copia dell'array `data` sottomesso, ripulito dalle località che non soddisfano il test.

#### PAYLOAD
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| *`radius` | *int* | max: 10KM |
| *`lat` | *float* | latitudine del centro di massa (cliente) |
| *`lng` | *float* | longitudine del centro di massa  (cliente) |
| *`data` | *array* | max: 100 items, lista di località per cui valutare la disponibilità (vedi modello `LOCALITY`) |

#### LOCALITY
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `id` | *int* | identificatore della località |
| *`lat` | *float* | latitudine della località (punto vendita) |
| *`lng` | *float* | longitudine della località (punto vendita) |

Disponibilità specifica per slot orari
=======================
### `/v1/availability/time`, POST
Questo servizio filtra una serie di slot orari (per i quali idealmente il cliente vorrebbe proporre la consegna) secondo l'effettiva disponibilità della rete logistica di S24.  
Nel caso in cui per una certa fascia oraria non sia possibile effettuare la consegna (nessuna disponibilità/fattorini occupati), tale slot viene rimosso dalla lista degli input.
#### PAYLOAD
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| *`radius` | *int* | max: 10KM |
| *`lat` | *float* | latitudine del punto vendita |
| *`lng` | *float* | longitudine del punto vendita |
| *`data` | *array* | oggetto specifico che indica il modello giornaliero della disponibilità. Vedi modello `DATA` |

#### DATA
- *Chiavi*:  `{Y-m-d}` Data per cui si vuole valutare la disponibilità nel formato anno-mese-giorno
- *Valori*: `[(H:i:s)_1, (H:i:s)_2, ..., (H)_n]` Lista di slot, ogni elemento rappresenta l'ora di inizio consegna (formato ore:minuti:secondi)

Esempio di richiesta:
```
{
  "lat": 44.5067177,
  "lng": 10.9475117,
  "radius": 10,
  "data": {
    "2015-10-28" : ["09:00:00", "10:00:00", "11:00:00", "12:00:00"],
    "2015-10-29" : ["09:00:00", "10:00:00", "11:00:00", "12:00:00"],
    "2015-10-30" : ["09:00:00", "10:00:00", "11:00:00", "12:00:00"]
  }
}
```
Esempio di risposta:
```
{
  "object": "list",
  "data": {
    "lat": 44.5067177,
    "lng": 10.9475117,
    "data": {
      "2015-10-28": ["09:00:00"],
      "2015-10-29": ["09:00:00", "11:00:00", "12:00:00"],
      "2015-10-30": []
    }
  }
}
```
