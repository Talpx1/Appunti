# Volumes

Fonti:
- https://youtu.be/3c-iBn73dDE?t=8846

## Cos'è un Volume?

I volumi sono un sistema studiato per la persistenza dei dati.  
Un container, dopo essere stoppato e ri-avviato, sia tramite `docker-compose` che tramite `docker run` o `docker start`, viene re-inizializzato allo stato originale. Questo comporta che, qualsiasi modifica effettuata verrà persa al riavvio.  
Per ovviare a questo problema, esistono i volumi, che rendono possibile salvare i dati di un container di modo che siano disponibili per il prossimo avvio.  

I volumi sono un collegamento tra file nel file system dell'host e filesystem del container.  
Infatti, i file o directory che devono essere persistiti,vengono 'montati' dal filesystem dell'host sul filesystem del container.

Qualsiasi modifica fatta ai file dell'host/container, viene riprodotta sul container/host.  

## Tipi di Volume

- ### Host Volumes

`docker run -v <host_data_directory>:<container_data_directory>`

Crea una referenza tra i file/directory situati/a in `<host_data_directory>`, montandola nel container nella directory specificata da `<container_data_directory>`.

Qualsiasi modifica fatta ai file dell'host/container, viene riprodotta sul container/host.  

- ### Anonymous Volumes

`docker run -v <container_data_directory>`

Docker crea un volume usando un hash univoco come nome (in `<docker_path>/volumes/random_hash/_data`), persistendo i file/directory specificati/a in `<container_data_directory>`.  

`<docker_path>`, di default, si trova in:  
- Windows: `C:\ProgramData\docker`.  
- Linux: `/var/lib/docker/volumes`.  
- MacOS: in una VM creata da docker in `/var/lib/docker/volumes`.  

Non è possibile referenziale questo volume.  

- ### Named Volumes

`docker run -v <volume_name>:<container_data_directory>`

Docker crea un volume usando il valore specificato in `<volume_name>` (in `<docker_path>/volumes/<volume_name>/_data`), persistendo i file/directory specificati/a in `<container_data_directory>`.

È possibile referenziale questo volume.  
È il metodo preferibile per creare i volumi.  

## Volumi in Docker Compose

Bisogna specificare i volumi per ogni container (o servizio).  
Si usa lo stesso tag, `volumes`, per tutti e tre i tipi di volume, ciò che cambia è la sintassi del valore.

Il tag accetta valori multipli.  

Si necessità poi di specificare tutti i named volumes che i container usano, questo va fatto in un apposito tag, `volumes`, che va messo allo stesso 'livello' del tag `services`.  
Questo tag accetta un valore multiline che deve venir composto da tag uguali al nome del named volume. Questi ultimi possono accettare a loro volta un valore multiline, composto da svariati tag, tra cui:
- `driver`: specifica il driver che il container deve utilizzare (se non serve specificare un driver il tag con nome del named volume accettano anche valori vuoti).  
Il valore `driver: local` indica di creare il volume nel filesystem locale.  
- `name`: permette di specificare un nome personalizzato al container.  

Es:  
```YAML
# docker-compose.yml

version: '3.1'
services:
    container_1:
        ...
        volumes:            
            - example_volume:/path/in/container
        ...
    ...
volumes:
    example_volume:
        driver: local
    ...
...
```      

Di seguito alcuni esempi sulla sintassi necessaria per la creazione dei vari tipi di volume.  

- #### Host Volumes

```YAML
# docker-compose.yml

version: '3.1'
services:
    container_1:
        ...
        volumes:
            - host/path/:container/path
            - ...
        ...
    ...
...
```  

- #### Anonymous Volumes

```YAML
# docker-compose.yml

version: '3.1'
services:
    container_1:
        ...
        volumes:
            - container/path
            - ...
        ...
    ...
...
```    

- #### Named Volumes

```YAML
# docker-compose.yml

version: '3.1'
services:
    container_1:
        ...
        volumes:
            - volume_name:container/path
            - ...
        ...
    ...
volumes:
    - volume_name
    - ...
...
```