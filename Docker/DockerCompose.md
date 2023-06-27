# Docker Compose

Docker Compose, o meglio `docker-compose`, è uno strumento che viene installato insieme a Docker.  
Offre una serie di comandi e funzioni utili per lavorare con applicazioni multi-container.  

Spesso Docker Compose viene usato per semplificare l'avvio dei container necessari per avviare un progetto, replicando i vari comandi `docker run ...` in un file apposito, chiamato `docker-compose.yaml`.  
Questo file verrà poi usato per inizializzare i container tramite un apposito comando offerto da Docker Compose.  

## Sintassi per il file docker-compose.yaml (o .yml)  

Le estensioni `.yaml` e `.yml` sono intercambiabili.  
Questi tipi di file si basano sull'indentazione per definre la loro struttura. Un'indentazione errata porta ad un errore nel momento dellésecuzione del file.  

I tag nel file seguono la struttura `tag: 'valore'`, oppure permettono valori multiline tramite la sintassi  
```YAML
tag:
  valore1:
    valore2:
      ...
```

In caso il tag accettasse valori multipli, si può definire la lista di valori in questo modo:    
```YAML
tag:
  valore1:
    valore2:
      ...
```

Il file inizia sempre con il tag `version`.
