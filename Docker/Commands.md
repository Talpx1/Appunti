# Docker Commands

Fonti:  
  - https://www.youtube.com/watch?v=3c-iBn73dDE&t=3242s
  - https://docs.docker.com/engine/reference/commandline/  

Tutti i comandi Docker iniziano con `docker`, vi sono poi alcuni "sottocomandi" (es: `container`, ...) che possono essere accodati per eseguire date operazioni.  
Es `docker container ls -a` ➔ visualizza tutti i container, compresi quelli inattivi.  

Per una lista completa di comandi con i loro argomenti, far riferimento alla documentazione https://docs.docker.com/engine/reference/commandline/  


## Docker Pull Command

con il comando `docker pull <image:tag>` è possibile scaricare in locale un'immagine da una repository docker.  
di default le immagini verranno scaricate da DockerHub.  

**Best Practice**: sempre meglio definire un tag ( = versione) per le immagini che si usano in un progetto.  

## Docker Run Command

con il comando `docker run <image:tag>` è possibile creare un container a partire da un'immaigne.  
se l'immagine non esiste in locale, verrà automaticamente scaricata da DockerHub.  
se non si usa la *detached mode*, per stoppere il container basterà premere `CTRL+C`.  
se non si definisce un nome per il container, docker ne assegnerà uno in automatico.  

### Argomenti utili per Docker Run

- `-d`: avvia il container in *detached mode* ➔ rilascia il terminale e fa girare il container in background.
- `-p <porta_host>:<porta_container>`: espone una porta del container (numero dopo i :), su una porta della macchina host (numero prima dei :) (fa un binding delle porte) ➔ il servizio esposto su <porta_container> sarà disponibile dall'host su <porta_host> (*vedi appunti sui container per altre info*).
- `--name <nome_container>`: assegna un nome al container che si sta andando a creare.
- `-e VARIABILE=valore`: questo flag può essere usato più volte nello stesso comando e permette di definire della variabili d'ambiente per il container.
- `--net <network_name>` oppure `--network <network_name>`: connette il container al network specificato.  

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

con il comando `docker logs <container_name || container_id>` è possibile visualizzare l'outut (logs) di un container se siamo in *detached mode*. 

## Docker Images Command

con il comando `docker images` è possibile visualizzare l'elenco di immagini disponibili in locale.  

## Docker Exec Command

con il comando `docker exec <container_name || container_id> <comando>` è possibile eseguire un comando all'interno del container, come se avessimo scritto il <comando> nel terminale del servizio all'interno del container stesso.  
il container DEVE essere in esecuzione, altrimenti si verificherà un errore.  
il <comando> DEVE essere un eseguibile.  

### Argomenti utili per Docker Exec

- `-i`: sta per *interactive*. Permette di mantenere `STDIN` aperto, quindi anche se il container non è stato avviato in *attached mode* (quindi è stato usato il flag `-d`) è possibile ricevere l'output del comando. Questo permette la composizione (o piping) dei comandi.  
- `-t`: alloca un driver pseudo-TTY al terminale del container. Questo permette di interagire via hardware (es. digirate con la tastiera) nel terminale del container.  
- `-it`: unisce i due flag precedenti, pemettendo di "accedere" al terminale del container (si potrebbe far finta di pensare di essere in ssh nel container).  
  ➔ per accedere al terminale si usa il comando `docker exec -it <container_name || container_id> /bin/bash` oppure `docker exec -it <container_name || container_id> sh`.  
- `-d`: esegue il comando in *detached mode* ➔ rilascia il terminale ed esegue il comando in background.

## Docker Network List Command
con il comando `docker network ls` è possibile visualizzare i network esistenti.  

## Docker Network Create Command
con il comando `docker network create <nome_network>` è possibile creare un nuovo network.  
