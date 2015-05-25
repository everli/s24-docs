## STIME
È disponibile un endpoint per il calcolo della stima di un costo di consegna utilizzando S24.

Stima dei costi di consegna
======================
### `/v1/estimates/price`, GET
Specificare i seguenti parametri per il calcolo di una stima:  

| Chiave | Tipo | Descrizione |
| ------ | ---- | ----------- |
| *`origin` | *string* | Indirizzo di partenza, può essere un indirizzo testuale o una coppia (latitudine, longitudine) |
| *`destinaton` | *string* | Indirizzo di destinazione, può essere un indirizzo testuale o una coppia (latitudine, longitudine) |
| *`amount` | *float* | importo della spesa |
| *`contract_id` | *int* | Identificativo del contratto con cui calcolare la stima |
| `invoice_required` | *int* | Specifica se è richiesta la fattura. Default: `0` |

In caso di successo, viene restituito un oggetto come segue con i costi stabiliti dal contratto per ciascuna delle entità coinvolte:
```
{
    "object": "ESTIMATE",
    "data": {
        "customer": 118.5,
        "courier": 9.25,
        "whitelabel": 0
    }
}
```

