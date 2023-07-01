# Docker Compose

Fonti:
 - https://www.youtube.com/watch?v=3c-iBn73dDE&t=5333s  

Docker Compose, o meglio `docker-compose`, è uno strumento che viene installato insieme a Docker.  
Offre una serie di comandi e funzioni utili per lavorare con applicazioni multi-container.  

Spesso Docker Compose viene usato per semplificare l'avvio dei container necessari per avviare un progetto, replicando i vari comandi `docker run ...` in un file apposito, chiamato `docker-compose.yaml`.  
Questo file verrà poi usato per inizializzare i container tramite un apposito comando offerto da Docker Compose.  

Nota: il file non deve chiamarsi per forza `docker-compose.yml`, anche se per convenzione, ove possibile, si tende a mantenere questa dicitura.  

## Sintassi per il file docker-compose.yaml (o .yml)  

Le estensioni `.yaml` e `.yml` sono intercambiabili.  
Questi tipi di file si basano sull'indentazione per definire la loro struttura. Un'indentazione errata porta ad un errore nel momento dell'esecuzione del file.  

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
  - valore1
  - valore2
  ...
```

Il file inizia sempre con il tag `version`.  
La versione attuale è la 3.1 ➔ `version: '3.1'`.  

Vi è poi, sempre, il tag `services`.  
Questo tag accetta un valore multi-line, che ha lo scopo di rappresentare la struttura dei container.  

Il file supporta commenti tramite la sintassi `# commento`.  

Es:
```YAML
version: '3.1'
services:
  container_1:
    ...
  container_2:
    ...
```

## Struttura di un container in Docker Compose

Dopo aver definito il tag `services` e aver scelto il nome del container, necessitiamo di descrivere la struttura di quest'ultimo. Ciò richiede l'utilizzo di diversi tag (quindi il tag col nome del container che abbiamo scelto, es: `container_1:`, accetta un valore multiline).  

- ### Il tag `image`

Il tag `image` viene usato per definire l'immagine da cui partire per costruire il container.
Questo tag può essere omesso in caso l'immagine che vogliamo usare non sia su Docker Hub ma sia un Dockerfile creato da noi (*vedi tag `build`*).    
Il nome delle immagini e i relativi tag possono essere trovati su Docker Hub (*vedi appunti su images per una spiegazione completa*).  
Questo tag equivale al nome immagine nel comando `docker run ...`.  

Es:
```YAML
version: '3.1'
services:
  container_1:
    image: php:8.2-fpm-alpine3.18
    ...
  ...
```

- ### Il tag `ports`

Il tag `ports` permette di definire i port-bindings tra host e container (*vedi appunti su containers e networks per una spiegazione più approfondita*).  
Questo tag può essere omesso in caso non servisse esporre porte dal container.  
Questo tag accetta valori multipli nel formato `- porta_host:porta_container`.    
Questo tag equivale al flag `-p` nel comando `docker run ...`.

Es:
```YAML
version: '3.1'
services:
  container_1:
    image: php:8.2-fpm-alpine3.18
    ports:
      - 9000:9000
      - ...
    ...
```

- ### Il tag `environment`

Il tag `environment` permette di definire i valori per le variabili d'ambiente nel container.  
Questo tag può essere omesso in caso non servissero variabili d'ambiente nel container.  
Questo tag accetta valori multipli nel formato `- VARIABILE=valore`.    
Questo tag equivale al/ai flag `-e` nel comando `docker run ...`.

Es:
```YAML
version: '3.1'
services:
  container_1:
    image: mariadb:latest
    environment:
      - MARIADB_ROOT_PASSWORD: example
      ...
    ...
  ...
```  

## Networks in Docker Compose

Di default, Docker Compose crea automaticamente un network comune per i container definiti al suo interno.  

## Avviare un progetto con Docker Compose

Per avviare un progetto usando Docker Compose e seguendo le direttive date in `docker-compose.yml`, si usa l'apposito comando:  
`docker-compose -f <file_name> up`  

Nota che, se il nome del file è `docker-compose.yml` e stiamo eseguendo il comando dalla stessa directory in cui esso è situato, non è necessario specificare il file tramite il flag `-f <file_name>`, riducendo il comando a `docker-compose up`.  

Nota che, se si avvia il progetto in *attached mode*, i logs dei container saranno mischiati.  

### Argomenti utili per il comando Docker Compose Up

- `-d`: avvia i container in *detached mode* ➔ rilascia il terminale e fa girare i container in background.

## Fermare un progetto con Docker Compose

Per fermare un progetto, attualmente in esecuzione, (quindi stoppare i relativi container) usando Docker Compose , si usa l'apposito comando:  
`docker-compose -f <file_name> down`  

Nota che, se il nome del file è `docker-compose.yml` e stiamo eseguendo il comando dalla stessa directory in cui esso è situato, non è necessario specificare il file tramite il flag `-f <file_name>`, riducendo il comando a `docker-compose down`.  

# TODO
- volumes
- migliorare gli appunti sui network
- aggiungere altri tag di costruzione dei container (es `build`, `depends_on`, ...)









