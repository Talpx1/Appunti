<small>Nota: questo file è stato chiamato Docker-File.md (e non Dockerfile.md) per evitare che i text-editor (sì VSCode, per esempio tu...) lo interpretassero come un effettivo dockerfile e non come un file markdown</small>

# DockerFile

Fonti:
- https://docs.docker.com/engine/reference/builder/
- https://www.youtube.com/watch?v=3c-iBn73dDE&t=6121s

Serve a costruire un'immagine.  
Viene eseguito in maniera sequenziale. 

Questo file, che DEVE essere chiamato `Dockerfile`, senza alcuna estensione, contiene la descrizione, passo-passo, della nostra immagine.

Solitamente si va a costruire "al di sopra" (prendendo come fondamenta), un'immagine già esistente, aggiungendogli funzionalità, strumenti, servizi, filesystem, ecc., in base alle necessità.  

## Sintassi
Commento: `# Commento`  
➔ alcuni commenti sono delle parser directive (*vedi apposita sezione docs su parser directives*), e hanno un effetto sull'esecuzione del dockerfile.  

Istruzione: `ISTRUZIONE argomenti`  
➔ l'istruzione va sempre in maiuscolo, per convenzione  

Il Dockerfile deve iniziare con l'istruzione `FROM` (*vedi appositi appunti sotto*).  

Prima di questa riga sono ammesse solo parser directive o commenti.

### Exec form vs Shell form
Alcune istruzioni possono essere scritte in più modi, le principali sono:
- **exec form:** spesso identificata dalla presenza di parentesi quadre e/o dalla presenza di un riferimento ad un eseguibile. Sono nella maggior parte dei casi la forma preferita. Non aprono una sessione shell quando il comando che specificano viene eseguito.  
Nota: la *exec form* viene parsata come JSON, quindi le doppie virgolette sono OBBLIGATORIE.  
  
    es: 
    ```Dockerfile
    CMD ["executable","param1","param2"] 
    ```
- **shell form:** ricordano la *exec form*, ma al posto di dividere l'argomento (spesso un comando), in più stringhe nelle parentesi quadre, lo scrivono in maniera "plain" dopo l'istruzione (come l'istruzione `RUN`, per intenderci). Questa forma apre una sessione shell nell'esecuzione del comando (quindi via `/bin/sh -c`), quindi, al contrario della *exec form*, effettua sostituzioni di variabili d'ambiente del servizio in esecuzione nel container.

    es: 
    ```Dockerfile
    CMD command param1 param2 
    ```

Nel seguente esempio, la variabile d'ambiente `$HOME`, verrà sostituita nella *shell form*, ma non nella *exec form*:  
```Dockerfile
# exec form
CMD [ "echo", "$HOME" ]
```

```Dockerfile
# shell form
CMD echo $HOME
```

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

- ### Istruzione `CMD`  

`CMD ["<executable>","<param1>","<param2>"]` (*exec form*)  
oppure  
`CMD ["<param1>","<param2>"]`  
oppure  
`CMD <command> <param1> <param2> ` (*shell form*)  

La seconda forma di questo comando necessità della definizione di un'istruzione `ENTRYPOINT` (*vedi appunti sull'istruzione `ENTRYPOINT`*).     

L'istruzione `CMD` esegue un comando *entry point*.
Nella *exec form*, nella prima stringa si passa il nome dell'eseguibile installato nel container, mentre nelle seguenti si passa il resto del comando utilizzato per avviare il servizio.   

Ci può essere un solo comando CMD per Dockerfile.  
Se dovessero essercene molteplici, solo l'ultimo verrà considerato.    

Questa istruzione viene eseguita SOLO nel container. 

Si può identificare lo use-case di questo comando ponendo come presupposto che va usato per quel comando che avvia, effettivamente, il servizio. Dopo il quale ci aspettiamo che il servizio e il container funzionino come da aspettativa.  

Es: 
```Dockerfile
FROM node:latest
...
CMD ["node", "server.js"]
```

#### Differenza tra `RUN` e `CMD`

Si potrebbe pensare che al posto di `CMD ["abc", "xyz"]` si possa usare `RUN abc xyz`. Questo, pur funzionando sul piano teorico, ha una differenza: `CMD`, oltre ad eseguire il comando, marca quest'ultimo come *entry point*

## Costruire un'immagine da un Dockerfile

Per trasformare il nostro set di istruzioni in una vera e propria immagine, possiamo usare il comando  
`docker build -t <image_name>:<tag> <Dockerfile_path>`  

Con questo comando possiamo creare un'immagine che avrà nome `<image_name>` e tag `<tag>`, usando le istruzioni del Dockerfile situato in `<Dockerfile_path>`.

L'immagine sarà poi disponibile tra le immagini locali (verificabile tramite `docker images`).  








# TODO

- spiegazioni su "env replacement"
- docker ignore
- `FROM` aliases
- parser directives
- RUN mount
- RUN network
- più istruzioni:
  - `ENTRYPOINT`
  - `ENTRYPOINT` vs `CMD`
  - `ARG`
  - `LABEL`
  - `EXPOSE`
  - `VOLUME`
  - `WORKDIR`
  - `USER`
  - `HEALTCHECK`