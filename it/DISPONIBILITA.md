## DISPONIBILITÀ

Disponibilità generica per una consegna
======================
### `/v1/availability`, POST
Il servizio filtra i punti vendita che non soddisfano il test di disponibilità di S24: nel caso non esista un fattorino abilitato all'interno del raggio specificato come parametro il test dà risultato negativo.  
Passare nel payload della richiesta la dimensione del raggio, il centro di massa (coordinate del cliente) e la lista di località (coordinate dei punti vendita) per cui si vuole valutare la disponibilità.  
La risposta sarà la copia dell'array `data` sottomesso, ripulito dalle località che non soddisfano il test.

#### PAYLOAD
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| *`radius` | *int* | max: 15KM |
| *`lat` | *float* | latitudine del centro di massa |
| *`lng` | *float* | longitudine del centro di massa |
| *`data` | *array* | max: 100 items, lista di località per cui valutare la disponibilità (vedi modello `LOCALITY`) |

#### LOCALITY
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `id` | *int* | identificatore della località |
| *`lat` | *float* | latitudine della località |
| *`lng` | *float* | longitudine della località |
