## DISPONIBILITÀ

Disponibilità per una consegna
======================
### `/v1/availability`, POST
Passare nel payload della richiesta la dimensione del raggio, il centro di 
massa e la lista di località per cui si vuole valutare la disponibilità
del delivery da parte di S24.  
La risposta sarà la copia dell'array `data` sottomesso, ripulito dalle
località che non soddisfano il test di disponibilità.  
Il test di disponibilità prevede che esista un fattorino abilitato entro
il raggio specificato come parametro.

#### PAYLOAD
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| *`radius` | *int* | max: 15KM |
| *`lat` | *float* | latitudine del centro di massa |
| *`lng` | *float* | longitudine del centro di massa |
| *`data` | *array* | max: 100 items, lista di località per cui valutare la disponibilità (vedi modello `LOCALITY` |

#### LOCALITY
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `id` | *int* | identificatore della località |
| *`lat` | *float* | latitudine della località |
| *`lng` | *float* | longitudine della località |