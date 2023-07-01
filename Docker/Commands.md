# Docker Commands

Fonti:  
- https://www.youtube.com/watch?v=3c-iBn73dDE&t=3242s
- https://docs.docker.com/engine/reference/commandline/  

Tutti i comandi Docker iniziano con `docker`, vi sono poi alcuni "sotto-comandi" (es: `container`, ...) che possono essere accodati per eseguire date operazioni.  
Es `docker container ls -a` ➔ visualizza tutti i container, compresi quelli inattivi.  

Per una lista completa di comandi con i loro argomenti, far riferimento alla documentazione https://docs.docker.com/engine/reference/commandline/  


## Docker Pull Command

con il comando `docker pull <image:tag>` è possibile scaricare in locale un'immagine da una repository docker.  
di default le immagini verranno scaricate da DockerHub.  

**Best Practice**: sempre meglio definire un tag ( = versione) per le immagini che si usano in un progetto.  

## Docker Run Command

con il comando `docker run <image:tag>` è possibile creare un container a partire da un'immagine.  
se l'immagine non esiste in locale, verrà automaticamente scaricata da DockerHub.  
se non si usa la *detached mode*, per stoppare il container basterà premere `CTRL+C`.  
se non si definisce un nome per il container, docker ne assegnerà uno in automatico.  

### Argomenti utili per Docker Run

- `-d`: avvia il container in *detached mode* ➔ rilascia il terminale e fa girare il container in background.
- `-p <porta_host>:<porta_container>`: espone una porta del container (numero dopo i :), su una porta della macchina host (numero prima dei :) (fa un binding delle porte) ➔ il servizio esposto su `<porta_container>` sarà disponibile dall'host su `<porta_host>` (*vedi appunti sui container per altre info*).
- `--name <nome_container>`: assegna un nome al container che si sta andando a creare.
- `-e VARIABILE=valore`: questo flag può essere usato più volte nello stesso comando e permette di definire della variabili d'ambiente per il container.
- `--net <network_name>` oppure `--network <network_name>`: connette il container al network specificato.  
- `-v <host_data_directory>:<container_data_directory>`: crea un host volume (*vedi appunti sui volumi per più info*), facendo un binding tra filesystem dell'host specificato in `<host_data_directory>` e filesystem del container specificato in `<container_data_directory>`.  
- `-v <volume_name>:<container_data_directory>`: crea un named volume (*vedi appunti sui volumi per più info*), con nome `<volume_name>`, che persiste il filesystem del container specificato in `<container_data_directory>`.
- `-v <container_data_directory>`: crea un anonymous volume (*vedi appunti sui volumi per più info*)che persiste il filesystem del container specificato in `<container_data_directory>`.

## Docker Remove Command

con il comando `docker rm <container_name || container_id>` è possibile eliminare un container.  

### Argomenti utili per Docker Remove

- `-v` o `--volumes`: rimuove i volumi associati al container.  
- `-f` o `--force`: forza la rimozione del container.  

## Docker Start Command

con il comando `docker start <container_name || container_id>` è possibile far ripartire un container fermato precedentemente.  

### Argomenti utili per Docker Start

- `-d`: avvia il container in *detached mode* ➔ rilascia il terminale e fa girare il container in background

## Docker Stop Command

con il comando `docker stop <container_name || container_id>` è possibile fermare un container senza eliminarlo. 

## Docker Container List Command

con il comando `docker container ls` è possibile visualizzare i container correntemente in esecuzione.  

**aliases:** 
  - `docker ps`
  - `docker container ps`  

### Argomenti utili per Docker Container List

- `-a`: visualizza tutti i container, compresi quelli inattivi.  

## Docker Logs Command

con il comando `docker logs <container_name || container_id>` è possibile visualizzare l'output (logs) di un container se siamo in *detached mode*.

## Docker Rename Command

con il comando `docker rename <container_name || container_id> <new_name>` è possibile rinominare il container identificato da `<container_name || container_id>` con il valore `<new_name>`.  

## Docker Images Command

con il comando `docker images` è possibile visualizzare l'elenco di immagini disponibili in locale.  

## Docker Exec Command

con il comando `docker exec <container_name || container_id> <comando>` è possibile eseguire un comando all'interno del container, come se avessimo scritto il `<comando>` nel terminale del servizio all'interno del container stesso.  
il container DEVE essere in esecuzione, altrimenti si verificherà un errore.  
il `<comando>` DEVE essere un eseguibile.  

### Argomenti utili per Docker Exec

- `-i`: sta per *interactive*. Permette di mantenere `STDIN` aperto, quindi anche se il container non è stato avviato in *attached mode* (quindi è stato usato il flag `-d`) è possibile ricevere l'output del comando. Questo permette la composizione (o piping) dei comandi.  
- `-t`: alloca un driver pseudo-TTY al terminale del container. Questo permette di interagire via hardware (es. digitare con la tastiera) nel terminale del container.  
- `-it`: unisce i due flag precedenti, permettendo di "accedere" al terminale del container (si potrebbe far finta di pensare di essere in ssh nel container).  
  ➔ per accedere al terminale si usa il comando  
  `docker exec -it <container_name || container_id> /bin/bash`  
  oppure  
  `docker exec -it <container_name || container_id> sh`  
  oppure  
  `docker exec -it <container_name || container_id> /bin/sh`  
- `-d`: esegue il comando in *detached mode* ➔ rilascia il terminale ed esegue il comando in background.

## Docker Network List Command
con il comando `docker network ls` è possibile visualizzare i network esistenti.  

## Docker Network Create Command
con il comando `docker network create <nome_network>` è possibile creare un nuovo network.  

## Docker Build Command

con il comando `docker build -t <image_name>:<tag> <Dockerfile_path>` possiamo trasformare un Dockerfile (*vedi appunti su Dockerfile*) in un'immagine
  
L'immagine avrà nome `<image_name>` e tag `<tag>`, e verrà costruita usando le istruzioni del Dockerfile situato in `<Dockerfile_path>`.

Possiamo immaginare questo comando come una sorta di avvio per un compiler che trasforma un Dockerfile in un'immagine.  

## Docker Remove Image Command

con il comando `docker rmi <image_name || image_id>` possiamo eliminare un'immagine salvata in locale.

Per poter eliminare un'immagine è necessario che nessun container, attivo o meno, sia istanza di quell'immagine.  

## Docker Container Prune Command

con il comando `docker container prune` è possibile eliminare tutti i container inattivi.  

## Docker System Prune Command

con il comando `docker system prune` è possibile eliminare tutti i container e immagini inutilizzati/e.  

## Docker Tag Command

con il comando `docker tag <source_image>:<tag> <target_image>:<tag>` è possibile creare una *target image* che fa riferimento a una *source image*. 

Una *target image* è un clone di una *source image*, che però ha un nome immagine completo, che torna utile per le operazioni di `docker push`.

Un nome completo ha il seguente formato: `<repo_uri>:<port>/<path>`.  
Sia `<host>` che `<port>` sono opzionali in caso si intenda usare l'immagine su Docker Hub.  

- `<host>`: uri della repo in cui l'immagine verrà pushata (default: dockerhub.io).  
- `<port>`: porta tramite la quale contattare la repo (default: 8080).  
- `<path>`: nome dell'immagine in formato `vendor/nome:versione`.  

## Docker Push Command

con il comando `docker push <image_name>:<tag>` è possibile caricare l'immagine `<image_name>` taggata con `<tag>`, su una Docker Repository (*vedi appunti su repository per più info*).

Per specificare la repository remota su cui eseguire il push, l'immagine dev'essere propriamente taggata col comando `docker tag`.  

Per eseguire il push, bisogna prima loggarsi sulla repository con il comando `docker login`.  

Il `<tag>` può essere omesso.

### Argomenti utili per Docker Push
- `-a`: pusha tutti i tag dell'immagine.  
- `-q`: sopprime l'output.

**Alias:** `docker image push`  

## Docker Login Command

con il comando `docker login <registry_server_uri>` è possibile eseguire il login in una docker repository (*vedi appunti su repository per più info*).  

Le credenziali per la repositori vengono salvate in:
- Linux: `$HOME/.docker/config.json`  
- Windows: `%USERPROFILE%/.docker/config.json`  
