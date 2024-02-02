# Direttive

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#directives </small>

Le direttive sono attributi speciali, che iniziano per `v-`.  

Le direttive, con alcune eccezioni, accettano come valori solo *espressioni* JS.  

Vedi la lista completa di direttive sulla documentazione ufficiale: https://vuejs.org/api/built-in-directives.html 

## Argomenti

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#arguments </small>

Alcune direttive possono essere corredate di *argomenti*, che vengono messi dopo la direttiva e preceduti da `:`.  
Es: 
```html 
<div v-bind:id="some_id">
``` 

### Argomenti dinamici

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#dynamic-arguments </small>

È possibile usare delle espressioni JS, tra parentesi quadre, per definire degli argomenti in maniera dinamica.
Es (dalla documentazione ufficiale di Vue):  
```html
<a v-bind:[attributeName]="url"> ... </a>
```
Qui `attributeName` è una variabile JS che ha come valore attributo HTML valido, per esempio `href`.  
In tal caso, l'istruzione precedente si risolverebbe in:
```html
<!-- dato che attributeName era href, [attributeName] si risolve effettivamente in quanto segue -->
<a v-bind:href="url"> ... </a>
```

Questo concetto può essere applicato anche agli eventi:
```html
<a v-on:[eventName]="funzioneJS"> ... </a>
<!-- supponendo che eventName abbia valore 'click', l'istruzione precedente si risolverebbe in quanto segue -->
<a v-on:click="funzioneJS"> ... </a> <!-- oppure, abbreviato, <a @click="funzioneJS">  -->
```

## Modificatori

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#modifiers </small>

Alcune direttive possono accettare, oltre che agli argomenti, anche dei modificatori, che appunto modificano il comportamento predefinito di queste direttive.   

Prendiamo come esempio `v-on` con l'argomento `submit`: in questo caso possiamo prevenire il comportamento di default del submit di un form (`event.preventDefault()`) semplicemente con il modificatore `.prevent`.
```html
<form v-on:submit.prevent="onSubmit">...</form>
```

La sintassi si una direttiva, può essere schematizzata in questo modo (immagine dalla documentazione ufficiale di Vue):
![Immagine non trovata :(](https://vuejs.org/assets/directive.7WSr6AKH.png)


