# Template Syntax

<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html </small>

È possibile usare JS in HTML tramite la sintassi `{{ espressione }}`. Viene effettuato l'escaping del contenuto automaticamente.  
Se non si vuole effettuare l'escaping, si dovrà usare la seguente sintassi:
```html
<span v-html="rawHTML"></spam>
```

**NOTA:** usare HTML senza escaping è pericoloso e rischia di esporre vulnerabilità come l'XSS

## Espressioni JS
<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#using-javascript-expressions </small>

È possibile usare espressioni JS nei seguenti posti:
- nei `v-`attributes
- dentro a `{{ }}`

Es (dalla documentazione ufficiale di Vue):
```js
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

**NOTA:** è possibile usare soltanto *espressioni*, ovvero istruzioni che si risolvono a un valore. Control structures, assegnazioni, ecc. non sono permesse.  

## Accesso alle globali
<small> Fonte: https://vuejs.org/guide/essentials/template-syntax.html#restricted-globals-access </small>

L'accesso alle globali è ristretto, clicca [qui](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsAllowList.ts#L3) per una lista di globali accessibili.

È possibile rendere delle globali accessibili tramite `app.config.globalProperties`.  