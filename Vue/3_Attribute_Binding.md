# Attribute Binding

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#attribute-bindings </small>

## v-bind

Per assegnare valori dinamici (espressioni JS) a un attributo, è possibile usare `v-bind`.  
`v-bind:` seguito da un attributo valido, permetterà di assegnare a tale attributo valori dinamici.  
Es (usando SFC):  
```js
// script setup
const id_dinamico = 'id_esempio'

```
```html
<!-- template -->
<div v-bind:id="id_dinamico"></div>
```

`v-bind:attributo` può essere accorciato in `:attributo`, di fatto omettendo `v-bind`.  
Es (usando lo stesso script setup di sopra):
```html
<!-- template -->
<div :id="id_dinamico"></div>
```

Inoltre se il nome del nostro valore dinamico (il nome della variabile) è uguale al nome dell'attributo a cui vogliamo assegnarlo, possiamo omettere di passare il valore esplicitamente, abbreviando a `:attributo`.  
Es:  
```js
// script setup
const id = 'id_esempio' //NOTA: la variabile ha nome id, proprio come l'attributo HTML

```
```html
<!-- template -->
<div :id></div> <!-- equivalente a :id="id" o a v-bind:id -->
```

### v-bind con valori bool

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#boolean-attributes </small>

`v-bind` funziona in modo leggermente diverso con i valori bool.  
Usiamo lo stesso esempio della documentazione ufficiale di Vue, in cui abbiamo il seguente pulsante:  
```html
<!-- is_disabled è una variabile definita nello script setup -->
<button :disabled="is_disabled"></button>
```
In questo caso, abbiamo 3 possibili comportamenti:
- `is_disabled` ha un valore truthy: `disabled` viene assegnato al pulsante;
- `is_disabled` ha valore `""`: `disabled=""` viene assegnato al pulsante;
- `is_disabled` ha un valore falsy: `disabled` viene omesso (non renderizzato) e il pulsante non sarà disabilitato;


### Assegnazione di più attributi contemporaneamente

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#dynamically-binding-multiple-attributes </small>

Poniamo caso di avere un oggetto di questo tipo, in cui le proprietà sono nel formato `nomeAttributo: valore attributo`:
```js
//script setup
const attributes = {id: 'id_esempio', name: 'nome_esempio'}
```
possiamo assegnare tutti gli attributi in una sola volta, in questo modo
```html
<input v-bind="attributes" />
```