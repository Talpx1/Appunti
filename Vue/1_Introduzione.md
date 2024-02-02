# Vue

## Installazione

Per l'installazione, segui la guida ufficiale di Vue: https://vuejs.org/guide/quick-start.html

## L'istanza dell'app
<small> Fonte: https://vuejs.org/guide/essentials/application.html </small>

L'istanza dell'applicazione si ottiene con `createApp(ComponenteRoot)`.  
Il `ComponenteRoot` e il componente che contiene al suo interno tutta la nostra app.  

Per far in modo che l'applicazione venga montata nella `index.html` si usa `app.count('#nodo')`.  
`app` è una variabile, di solito `const` che contiene l'istanza della nostra applicazione (ritornata da `createApp`).  
`#nodo` è l'id dell elemento HTML presente nella `index.html`, dentro al quale verrà iniettata l'app.

`app` espone un oggetto `.config` su cui è possibile chiamare dei metodi per la configurazione di alcune opzioni:  
- `.errorHandler((error) => {...})`: permette di creare un handler globale per le eccezioni  
- `.component`: permette di registrare globalmente un componente

**BEST PRACTICE:** è meglio definire il minor numero di componenti globali possibile. Questo perché non sono *three-shaking* friendly, ovvero non permettono di essere omessi dal bundle di produzione se non vengono usati, mentre i componenti registrati in maniera locale sono più ottimizzati sotto questo aspetto.  

È possibile avere più istanze dell'app:
```js
const app1 = createApp(ComponenteRoot1)
app1.mount('#nodo1')

const app2 = createApp(ComponenteRoot2)
app1.mount('#nodo2')
```