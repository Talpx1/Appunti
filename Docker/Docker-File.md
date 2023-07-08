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

Prima di questa riga sono ammesse solo parser directive, commenti oppure istruzioni `ARG` che specificano argomenti usati dall'istruzione `FROM`.

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

In caso di immagini multi-stage, l'istruzione `FROM` può essere usata più volte e può essere assegnato un alias ad ogni stage, nel seguente modo:  
`FROM image_name:tag AS alias`  
Questo stage può essere referenziato da altre istruzioni tramite `--from=alias`.  

In caso `FROM` facesse riferimento ad un'immagine multi-platform, si può scegliere l'immagine per la piattaforma desiderata tramite `FROM --platform=piattaforma image_name:tag`.


- ### Istruzione `ENV`  

`ENV VARIABILE=valore`

Multi-line (in caso di più env vars)  
```Dockerfile
ENV VAR1=val1 \
    VAR2=val2   
```

L'istruzione `ENV` serve per definire variabili d'ambiente per il container che verrà istanziato a partire da questa immagine.  

*Best Practice:* se si prevede l'utilizzo di questa immagine da un docker-compose (*vedi appunti su docker compose per più info*), conviene delegare la definizione delle env vars a quest'ultimo, al fine di avere maggior controllo e possibilità di eseguire cambiamenti senza dover fare un re-build dell'immagine.   

Queste variabili possono essere anche interpretate e sostituite all'interno del Dockerfile stesso (*vedi sezione su Env Replacement per più info*).  

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

## Env Replacement

Le variabili d'ambiente dichiarate con l'istruzione `ENV` (*vedi istruzione `ENV` sopra*), possono essere interpretate e usate da altre istruzioni.  

In un Dockerfile si può usare una variabile d'ambiente, precedentemente dichiarata, usando la seguente sintassi: `$VAR` o, preferibilmente, `${VAR}`.  

Si può fornire un valore di default, usato se la variabile non fosse definita, con la sintassi: `${VAR:-valore_di_default}`.

È, inoltre, possibile usare la sintassi `${VAR:+valore}` nel caso si volesse usare `valore` solo quando `VAR` è definita. Se questa non fosse definita, il risultato dell'espressione sarà una stringa vuota.  

I valori precedentemente menzionati possono essere delle stringhe oppure altre variabili d'ambiente.  

Per effettuare l'escape di una variabile d'ambiente si usa il carattere `\`.  
Es: `COPY \$FOO /var/www/html/`  

La sostituzione delle variabili d'ambiente è supportata dalle seguenti istruzioni:
- `ADD`
- `COPY`
- `ENV`
- `EXPOSE`
- `FROM`
- `LABEL`
- `STOPSIGNAL`
- `USER`
- `VOLUME`
- `WORKDIR`
- `ONBUILD`, quando usata in combinazione con una delle istruzioni sopra.

NOTA: se si desidera cambiare un valore ad una variabile d'ambiente, per usarne poi il valore aggiornato, assicurarsi di fare ciò in una nuova istruzione, in quanto la sostituzione del valore viene effettuata dopo che l'istruzione di sostituzione viene valutata. 

## Docker Ignore File

In caso fosse necessario ignorare dei file, in una maniera simile a come funziona un file `.gitignore`, è possibile creare nella root un file con nome `.dockerignore`.  

In questo file è possibile elencare file o directory da ignorare durante le operazioni eseguite nel Dockerfile.  

È possibile inserire commenti usando la sintassi `# commento`.  

Ci sono alcuni caratteri wildcard che possono essere usati per eseguire il matching dei percorsi:
- il carattere `*`, usato nel nome di un file/directory, eseguirà il match di qualsiasi file/directory, contenuti nella directory specificata, che iniziano/finiscono (a seconda che `*` sia dopo/prima il nome file/directory) con il nome del file/directory specificato/a.  
Es:  
`./folder/esempio_*` eseguirà il match di tutti i file/directory, nella cartella `folder`, il cui nome inizia per `esempio_`.  
`./folder/*_esempio` eseguirà il match di tutti i file/directory, nella cartella `folder`, il cui nome finisce per `_esempio`.  
- il carattere `*`, usato nel nome di una cartella del path, verrà sostituito con il nome delle cartelle disponibili in quel livello del filesystem, continuando in ognuna l'espressione di matching. 
Es:  
`./*/esempio_*` eseguirà il match di tutti i file/directory, il cui nome inizia per `esempio_`, in qualsiasi cartella un livello sotto la root. Possiamo pensare a questa espressione come ad un raggruppamento di espressioni in cui la prima `*` viene sostituita con il nome delle sottocartelle della root.  
- il carattere `?`, usato nel nome di un file/directory, verrà interpretato come placeholder per un singolo carattere, nel nome del file/directory specificato.  
Es:  
`./esempio?` eseguirà il match di tutti i file/directory, nella cartella root, il cui nome inizia per `esempio` ed ha uno e solo un carattere a seguire (`esempio1` e `esempioA`, sono esempi validi, mentre `esempio123` e `esempioABC` non lo sono).   
- il carattere `**`, in un'espressione di matching, rappresenta un qualsiasi numero (quindi una profondità indefinita) di subdirectories.
Es: 
`**/temp*` eseguirà il matching di qualsiasi file/directory il cui nome inizia con `temp`, situati in una qualsiasi subdirectory o anche nella root.  
- il carattere `!` seguito da un'espressione di matching, indica un'eccezione per i file/directory individuati dall'espressione che segue `!`.  
L'esempio seguente eseguirà il matching di tutti i files con estensione `.md` nella cartella root, con eccezione del file `README.md`.  
```ignore
*.md
!README.md
``` 
 










# TODO

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