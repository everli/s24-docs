## ORDINI
Il modello dell'ordine è composto come segue (le chiavi con * sono obbligatorie per la creazione di un nuovo ordine, mentre quelle precedute da ~ sono in sola lettura):

#### ORDER
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| ~`id` | *int* | identificatore univoco |
| *`contract_id` | *int* | identificatore del contratto da applicare per quest'ordine |
| `ref_id` | *string* | riferimento *custom* all'ordine |
| *`customer` | *json* | oggetto che identifica il cliente (vedi modello `CUSTOMER`) |
| *`store` | *json* | oggetto che identifica il negozio (vedi modello `STORE`) |
| *`details` | *array* | lista di prodotti (vedi modello `ORDER_DETAIL`) |
| `customer_invoice` | *json* | informazioni relative alla fatturazione (comporta un incremento di 2 euro sul totale dell'ordine, vedi modello `CUSTOMER_INVOICE`) |
| *`deliver_at_start` | *string* | ora di inizio consegna (formato `Y-m-d H:i:s`). Questo valore deve essere almeno 45 minuti successivo al momento dell’inserimento dell’ordine e non superare i 7 giorni dal momento dell'inserimento |
| *`deliver_at_end` | *string* | ora di fine consegna (formato `Y-m-d H:i:s`). Questo valore deve essere almeno 30 minuti superiore a `deliver_at_start` |
| ~`amount` | *float* | costo della spesa (costo scontrino). All'inizio viene calcolato ciclando sui dettagli dell'ordine |
| `notes` | *string* | note riguardanti l'ordine |
| ~`code` | *string* | codice dell'ordine |
| ~`total` | *float* | costo totale (per il cliente) |
| ~`status_code` | *string* | rappresenta lo stato dell'ordine: *DRAFT*, *OPEN*, *CONFIRMED*, *ASSIGNED*, *PAID*, *DELIVERED*, *CLOSED* |
| ~`delivery_cost_fixed` | *float* | costo di consegna applicato in base al contratto |
| ~`delivery_cost_percentage` | *float* | costo dell'ordine con percentuale di sovrapprezzo, in base al contratto applicato |
| ~`created_at` | *string* | data di creazione |
| ~`updated_at` | *string* | data di ultima modifica |
| ~`deleted_reason` | *int* | costante numerica che indica la motivazione per cui un ordine è stato eliminato |

#### CUSTOMER
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| ~`id` | *int* | identificatore del cliente |
| *`address` | *string* | indirizzo del cliente |
| *`postal_code` | *string* | CAP |
| *`city` | *string* | paese |
| *`province` | *string* | provincia |
| *`country` | *string* | nazione |
| *`street_number` | *string* | numero civico |
| `doorbell` | *string* | scritta sul campanello |
| *`first_name` | *string* | nome |
| *`last_name` | *string* | cognome |
| *`email` | *email* | email |
| *`phone` | *string* | numero di telefono **valido** (inizia con +39, senza spazi) |
| `vat` | *string* | partita IVA o codice fiscale del cliente |
| `ref_id` | *string* | riferimento *custom* al customer |
| `lat` | *float* | latitudine del cliente, se mancante viene calcolata da S24 |
| `lng` | *float* | longitudine del cliente, se mancante viene calcolata da S24 |
| ~`full_name` | *string* | nome e cognome del cliente |
| ~`short_description` | *string* | nome, cognome e telefono del cliente |

#### STORE
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| ~`id` | *int* | identificatore del negozio |
| *`name` | *string* | nome del negozio |
| *`address` | *string* | indirizzo del cliente |
| *`postal_code` | *string* | CAP |
| *`city` | *string* | paese |
| *`province` | *string* | provincia |
| *`country` | *string* | nazione |
| `street_number` | *string* | numero civico |
| `phone` | *string* | numero di telefono **valido** (inizia con +39, senza spazi) |
| `lat` | *float* | latitudine del cliente, se mancante viene calcolata da S24 |
| `lng` | *float* | longitudine del cliente, se mancante viene calcolata da S24 |
| `ref_id` | *string* | riferimento *custom* allo store |

#### ORDER_DETAIL
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| ~`id` | *int* | identificatore del prodotto |
| *`name` | *string* | nome del prodotto |
| *`quantity` | *float* | quantità (min: 1) |
| *`quantity_found` | *float* | quantità trovata dal picker/shopper (min: 1) |
| *`description` | *string* | descrizione |
| *`replaceable` | *int* | costante numerica che indica la sostituibilità del prodotto (spiega cosa fare al fattorino in caso non trovi il prodotto). Valori consentiti: 1 - cancella il prodotto dall'ordine, 2 - sostituisci il prodotto con uno similare, 3 - chiama il cliente e proponi alternative |
| *`type` | *int* | costante che indica l'unità di misura: Valori consentiti: 1 - pezzo, 2 - grammi, 3 - chilogrammi, 4 - litri |
| `value` | *float* | valore associato all'unità di misura |
| *`price` | *float* | prezzo (unitario) |
| `price_real` | *float* | uso interno |
| `surcharge_fixed` | *float* | uso interno |
| `limit` | *int* | uso interno |
| `url_thumbnail` | *string* | url per l'anteprima dell'immagine |
| `note` | *string* | note riguardanti il prodotto |
| `category_name` | *string* | categoria merceologica del prodotto. I dettagli vengono ordinati secondo questo campo |
| `category_id` | *int* | identificativo della categoria del prodotto. |
| `ref_id` | *string* | riferimento ad un eventuale riferimento custom |

#### CUSTOMER_INVOICE
| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| ~`id` | *int* | identificatore |
| *`company` | *string* | Azienda, Ragione Sociale |
| *`vat` | *string* | Partita IVA o Codice Fiscale (max: 16 caratteri) |
| *`address` | *string* | indirizzo |
| *`postal_code` | *string* | CAP |
| *`city` | *string* | paese |
| *`province` | *string* | provincia |
| *`country` | *string* | nazione |
| *`street_number` | *string* | numero civico |

Creazione di un ordine
======================
### `/v1/orders`, POST
Passare nel payload della richiesta il modello dell'ordine che si vuole creare. In caso di successo, l'ordine è nello stato `DRAFT`.

Modifica di un ordine
======================
### `/v1/orders/{order_id}`, PUT
È possibile, prima che un ordine sia confermato (`status = CONFIRMED`) modificare alcune informazioni dell'ordine.  
Passare nel payload della richiesta un json con i valori che si vogliono modificare. I campi dell'ordine che si possono modificare sono:
- `ref_id`
- `notes`  

In caso di successo viene tornato il modello dell'ordine.

Apertura di un ordine
======================
### `/v1/orders/{order_id}/open`, POST
Effettua il passaggio dallo stato `DRAFT` allo stato `OPEN`.

Conferma di un ordine
======================
### `/v1/orders/{order_id}/confirm`, POST
Conferma l'ordine: l'ordine è preso in carico dalla piattaforma S24. Vengono ricercati e notificati i fattorini disponibili per la consegna.

Eliminazione di un ordine
======================
### `/v1/orders/{order_id}`, DELETE
Un ordine può essere eliminato solamente se in stato `DRAFT` o `OPEN`.  
In caso di successo, la risposta è come segue:
```
{
  deleted: true
  id: $order_id
}
```

Elenco ordini
======================
### `/v1/orders`, GET
Restituisce l'elenco degli ordini.

Recupero di un ordine
======================
### `/v1/orders/{order_id}`, GET
Restituisci la risorsa specificata dall'id.
