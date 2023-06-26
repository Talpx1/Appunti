# Docker Networks  

Fonti:  
  - https://www.youtube.com/watch?v=3c-iBn73dDE&t=4480s

Docker crea un Docker Network isolato in cui i container sono instanziati. Se quindi i container sono nello stesso network, possono raggiungersi tramite il solo nome del container.  
Se i container che devono comunicare sono in network separati, essi dovranno comunicare tramite l'host usando le porte che vengono appositamente esposte.  
  ➔ è il motivo per cui il browser, per esempio, non può raggiungere il container direttamente ma ha bisogno di farlo tramite la porta ppositamente esposta.

**Best Practice:** quando definiamo il binding delle porte tra host e container, se non vi sono conflitti è consigliabile utilizzare la stessa porta sia per l'host che per il container.  

Per semplicità possiamo immaginare che ogni network sia una LAN e i vari container al suo interno siano dei client. Il nome del container può essere immaginato come una sorta di alias dell'ip del container/client immaginario.  

## Visualizzare i network esistenti
Possiamo visualizzare i network esistenti tramite il comando `docker network ls` (*vedi gli appunti sui comandi per una spiegazione più approfondita su questo comanmdo*).  

## Creare un nuovo network
Possiamo creare un nuovo network tramite il comando `docker network create <nome_network>` (*vedi gli appunti sui comandi per una spiegazione più approfondita su questo comanmdo*).  

## Instaziare un container all'interno di un network
Per far sì che un container venga instanziato all'interno di un network, dobbiamo esplicitamente indicarlo nel momento in cui creiamo il container.
Questo può essere fatto tramite apposito flag nel comando `docker run`: `--net <network_name>` oppure `--network <network_name>`.  
