<small>Nota: questo file è stato chiamato Docker-File.md (e non Dockerfile.md) per evitare che i text-editor (sì VSCode, per esempio tu...) lo interpretassero come un effettivo dockerfile e non come un file markdown</small>

# DockerFile

Fonti:
- https://docs.docker.com/engine/reference/builder/
- https://www.youtube.com/watch?v=3c-iBn73dDE&t=6121s

Serve a costruire un'immagine.  
Viene eseguito in maniera sequenziale.  

## Sintassi
Commento: `# Commento`  
➔ alcuni commenti sono delle parser directive (*vedi apposita sezione docs su parser directives*), e hanno un effetto sull'esecuzione del dockerfile.  

Istruzione: `ISTRUZIONE argomenti`  
➔ l'istruzione va sempre in maiuscolo, per convenzione  

Il Dockerfile deve iniziare con l'istruzione `FROM` (*vedi appositi appunti sotto*).  

Prima di questa riga sono ammesse solo parser directive o commenti.  

## Istruzioni 

- ### Istruzione `FROM`  

`FROM image_name:tag`

L'istruzione `FROM` serve per dichiarare quale immagine genitore stiamo andando ad usare.  

Se non si specifica il tag, verrà usato il tag `latest`.  

Es: 
```Dockerfile
FROM mariadb:latest
```

- ### Istruzione `ENV`  

`ENV VARIABILE=valore`

Multi-line (in caso di più env vars)  
```Dockerfile
ENV VAR1=val1 \
    VAR2=val2   
```

L'istruzione `ENV` serve per definire variabili d'ambiente per il container che verrà istanziato a partire da questa immagine.  

*Best Practice:* se si prevede l'utilizzo di questa immagine da un docker-compose (*vedi appunti su docker compose per più info*), conviene delegare la definizione delle env vars a quest'ultimo, al fine di avere maggior controllo e possibilità di eseguire cambiamenti senza dover fare un re-build dell'immagine.   

Es: 
```Dockerfile
FROM node:12-alpine3.11
...
ENV NODE_VERSION=12.22.7
```

- ### Istruzione `RUN`  

`RUN <comando>`


L'istruzione `RUN` permette di eseguire un comando da terminale nel container, in accordo con l'OS installato (o meglio, che si ha dato direttiva di installare) nel futuro container che verrà generato da questa immagine.  

I comandi saranno eseguiti SEMPRE nel container e MAI nell'host. 

Es: 
```Dockerfile
FROM ubuntu:latest
RUN mkdir -p /home/app
```

- ### Istruzione `COPY`  

`COPY <host_path> <container_path>`


L'istruzione `COPY` permette di copiare un file o una directory, situato/a nella macchina host, nel path specificato in `<host_path>`, all'interno del container, nella posizione indicata da `<container_path>`.  

Questa istruzione viene eseguita nell'host. 

Es: 
```Dockerfile
FROM ubuntu:latest
COPY . /home/app
```
