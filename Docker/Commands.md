# Docker Commands

Tutti i comandi Docker iniziano con `docker`, vi sono poi alcuni "sottocomandi" (es: `container`, ...) che possono essere accodati per eseguire date operazioni.  
Es `docker container ls -a` ➔ visualizza tutti i container, compresi quelli inattivi.  


## Docker Pull Command

con il comando `docker pull <image:tag>` è possibile scaricare in locale un'immagine da una repository docker.  
di default le immagini verranno scaricate da DockerHub.  

## Docker Run Command

con il comando `docker run <image:tag>` è possibile creare un container a partire da un'immaigne.  
se l'immagine non esiste in locale, verrà automaticamente scaricata da DockerHub.  

### Argomenti utili per Docker Run

- `-d`: avvia il container in *detached mode* ➔ rilascia il terminale e fa girare il container in background

## Docker Start Command

con il comando `docker start <container_name || container_id>` è possibile far ripartire un container fermato precedentemente.  

### Argomenti utili per Docker Start

- `-d`: avvia il container in *detached mode* ➔ rilascia il terminale e fa girare il container in background

## Docker Stop Command

con il comando `docker stop <container_name || container_id>` è possibile fermare un container senza eliminarlo.  
