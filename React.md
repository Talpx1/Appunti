# REACT


<p> Fonti:  

- https://www.youtube.com/watch?v=bMknfKXIFA8
</p>


## Vantaggi

- genera codice componibile e riutilizzabile: vengono definiti molti componenti che si occupano di una piccola cosa
  - questi piccoli componenti possono essere combinati tra loro per creare componenti piú grandi e funzionali
  - questi piccoli componenti possono essere utilizzati piú volte in piú posti diversi rendendoli riutilizzabili (non si ripete lo stesso codice)

- é dichiarativo:
  - basta dire cosa si desidera fare, l'implementazione  di come farlo non dev'essere specificata (é gestita da `React`)
  - opposto di imperativo → bisogna descrivere ogni passaggio necessario per compiere un'operazione (`vanilla JS` é imperativo)

- é componibile:
  - é possibile definire piccoli pezzi di interfaccia che possono essere assemblati tra loro per creare porzioni di interfaccia piú grandi
  - → questi pezzetti di interfaccia possono essere riutilizzati piú vole e in piú posti


## Import

- é possibile importare `React` da CDN:
  - inserire tags script del CND in head
  - inserire script tag con `type="text/babel"` 
    - → punta a un file js locale dove si puó scrivere usando `React` e `Babel` (`Babel` abilita `JSX`)

  non é la maniera ottimale, é facile e veloce ma limita la potenza dello strumento → in questo modo la variabile `ReactDOM` é gia disponibile nello script js  
  
- si importa `React` da `npm` (o altri package managers):
  - → in questo caso la variabile `ReactDOM` non é definita e `JSX` non viene interpretato, quindi bisonga:
    - importare `React` per `JSX` → `import React from "react"`
    - importare `ReactDOM` → `import ReactDOM from "react-dom"`


## Basi

- istruzione `ReactDOM.render(param1, param2)`:
  - `param1`: cosa voglio renderizzare (es: espressione `JSX`)
  - `param2`: dove voglio renderizzarlo 
    - → il contenuto di `param1` verrá messo all'interno dell'elemento indicato da `param2` → `param2` dev'essere un `DOMNode`
      - es: ```document.getElementById('root')``` → di solito si usa un elemento con ```id="root"``` come 'punto di ignezione'
  - es: `ReactDOM.render(<h1>Hello, World!</h1>, document.getElementById('root'));`
  - es2: `ReactDOM.render(<h1>Hello, World!</h1>, document.querySelector('#root'));`
  
- la riusabilitá e componibilitá del codice é fornita tramite i componenti


## Sintassi

- nel codice di `React` (con `Babel`) é possibile inserire una sorta di `HTML`, chiamato `JSX (JavaScriptXML)` 
  - → i tag `JSX` sono delle espressioni che vengono convertite in oggetti JS

- in `JSX` al posto di `class="css_class1 ..."` si usa `className="css_class1 ..."` → la parola `class` in `JS` é riservata

- in funzioni che accettano `JSX` o nello statement di return di un componente é possibile ritornare un solo tag di primo livello 
  - → altri tag possono essere nestati dentro questo 

- se si vuole ritornare un'espressione multiline `JSX`, questa va messa tra parentesi → si evita l'errato autopiazzamento del `;`
  - es: 
  ```js 
	const elem = (
	  <div>
	     <h1>Title</h1>
	     <p>paragraph</p>
	  </div>
	);
  ```
  
- siccome un solo tag di primo livello puó essere ritornato, é possibile usare un `fragment` 
  - → `<> JSX_HERE </>` → non renderizza nessun tag `HTML` (al contrario di un `div` wrapper che renderizzerebbe un `div`, appunto)  


## Componenti

I componenti sono piccoli pezzi di codice che possono essere assemblati tra loro per creare componenti ancora piú grandi e funzionali.  
Possono essere definiti, in modo tecnico, funzioni che ritornano espressioni `JSX`.  

- per creare un componente basta definire una funzione che ritorna un'espressione `JSX`

- i componenti vanno definiti in `PascalCase`

- per usare un componente basta usare il nome del componente come un tag `HTML`
	- → es `<Componente></Componente>` o `<Componente />`

- i tag componente `JSX` possono accettare l'auto-chiusura (`<Componente />` al posto di creare `<Componente></Componente>` senza tag nestati)  

- componenti possono essere usati all'interno di altri componenti

- i componenti di solito risiedono in un proprio file
  - preferibilmente il file deve chiamarsi come il componente (o l'export di default), seguendo il `PascalCase` 
  - siccome in questio file si usa `JSX`, bisogna importare `React` tramite `import React from "react"` 
    - dalla versione 17 di `React` é possibile evitare questo import 
    - non serve importare `ReactDOM` → non si esegue nessun rendering
  - es: il componente Header sará definito in un file Header.jsx

- si usa l'estensione `.jsx` per definire file in cui si usa `JSX` (`.tsx` in caso si usi `Typescript`)

- i componenti, se definiti in un file diverso da quello in cui si sta lavorando, vanno importati
  - es: sto lavorando nel file App.jsx e voglio usare il componente Header definito in Header.jsx. In questo caso, in App.jsx, dovró importare Header tramite:
    - `import Header from ./path_to_file/Header` → se il componente viene esportato di default in Header.jsx 
    - `import { Header } from ./path_to_file/Header` → se il componente NON viene esportato di default in Header.jsx 
      - nello statemant di import non é necessario indicare l'estensione del file
      - é necessario includere il `./` per esplicitare il fatto che si sta importando da un file e non da una libreria

- i componenti, per essere usati al di fuori del file in cui sono definiti, devono essere esportati
  - export di default: `export default funcion Componente{ ... }` → é possibile avere un solo export di default in ogni file
  - export NON default: `export funcion Componente{ ... }` → é possibile piú export NON default in un file

- nel file di un componente, possono essere specificate anche altre funzioni, costanti e qualsiasi espressione `JS` valida → possono anche non venir esportate

- i componenti possono essere definiti tramite classi o funzioni → il modo moderno di usare `React` prevede di usare le funzioni
  - → questo modo é anche molto piú veloce, pulito e meno verboso 


## Filesystem

é possibile suddvidere i file (componenti, ...) creati in sottocartelle.  
Spesso ci sono diversi modi per organizzare il progetto, quindi la scelta é anche personale. 

- si ha sempre un file `index.html` → contiene la parte 'al di fuori' di `React`
  - la head del sito é accessibile in questo file
  - il body del sito é definito in questo file
  - solitamente si ha un div, subito dentro il body, con `id="root"`, che funge da punto di ignezione del contenuto generato con `React`
  - contiene il collegamento al file `JS` iniziale in cui `ReactDOM` é definito ed esegue l'istruzione di rendering

- solitamente si ha un file `index.js` → file `JS` iniziale linkato dal tag script in `index.html`
  - contiene l'inizio dell'app `React`
  - contiene il primo import di `React`
  - contiene l'istruzione di rendering del componente react e l'import di `ReactDOM` → `ReactDOM.render(<App />, document.querySelector('#root'))`
  - contiene l'import al componente radice (o il componente in sé) → viene usato come primo argomento nella funzione `ReactDOM.render` (punto precedente)
    - solitamente chiamato `App`
    - a volte definito nell file `index.js` stesso
    - a volte qeusto componente viene lasciato nella root del progetto e non viene, quindi, collocato insieme agli altri componenti
    - da questo componente si diramano tutti gli alti componenti, contesti, ...

