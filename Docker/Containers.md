# Containers

I containers sono istanze eseguibili di un'immagine.  
Possono essere collegati a uno o più network.  
Possono esporre delle porte tramite le quali ricevere o inviare dati.
Possono implementare uno stato che persiste dei dati, tramite i volumi.  

I container possono essere creati, manipolati ed eliminati tramite gli appositi comandi, oppure tramite Docker Compose.

I container, di default, sono isolati da altri container e dal sistema host.
Questo isolamento può essere modificato tramite i network, le porte, i volumi e gli altri sistemi messi a disposizione da Docker.  

Nel sistema host, i container possono essere visti come dei processi.  