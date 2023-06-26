# REACT

<p> Fonti:

- https://www.youtube.com/watch?v=bMknfKXIFA8
</p>

## Vantaggi

- genera codice componibile e riutilizzabile: vengono definiti molti componenti che si occupano di una piccola cosa

  - questi piccoli componenti possono essere combinati tra loro per creare componenti piú grandi e funzionali
  - questi piccoli componenti possono essere utilizzati piú volte in piú posti diversi rendendoli riutilizzabili (non si ripete lo stesso codice)

- é dichiarativo:

  - basta dire cosa si desidera fare, l'implementazione di come farlo non dev'essere specificata (é gestita da `React`)
  - opposto di imperativo → bisogna descrivere ogni passaggio necessario per compiere un'operazione (`vanilla JS` é imperativo)

- é componibile:
  - é possibile definire piccoli pezzi di interfaccia che possono essere assemblati tra loro per creare porzioni di interfaccia piú grandi
  - → questi pezzetti di interfaccia possono essere riutilizzati piú vole e in piú posti

## Import e Setup

- é possibile importare `React` da CDN:
  - inserire tags script del CND in head
  - inserire script tag con `type="text/babel"`
    - → punta a un file js locale dove si puó scrivere usando `React` e `Babel` (`Babel` abilita `JSX`)
  - la variabile `ReactDOM` é gia disponibile nello script js
  - non é la maniera ottimale, é facile e veloce ma limita la potenza dello strumento
- si importa `React` da `npm` (o altri package managers) e si usa un tool come `CreateReactApp`, `Vite`, ...:
  - usando un tool come `CreateReactApp`, `Vite`, ... si esegue il setup di `Babel` e di un bundler come `Webpack` o simili
  - in questa maniera la variabile `ReactDOM` non é definita e `JSX` non viene interpretato, quindi bisonga:
    - importare `React` per `JSX` → `import React from "react"`
    - importare `ReactDOM` → `import ReactDOM from "react-dom"`
  - maniera migliore e consigliata per creare un progetto, soprattutto se andrá in produzione

### Create React App (CRA)

- installare `Node.js` → opzionalmente usare `nvm` (Node Version Manager)
- installare `npm`
- inizializzare `nvm` → `nvm install --lts`
- creare il progetto → `npx create-react-app app-name`
- entratre nella cartella creata → `cd app-name`
- avviare il progetto → `npm start`
  - l'output della console dovrebbe fornire il link di sviluppo su cui é possibile navigare l'applicazione
    - solitamente `http://localhost:3000` per la macchina corrente e `http://<current.machine.lan.ip>:3000` per il network locale (LAN)

→ verrá creato un setup boilerplate con i principali file necessari per iniziare ad usare `React`

## Basi

- istruzione `ReactDOM.render(param1, param2)`:
  - `param1`: cosa voglio renderizzare (es: espressione `JSX`)
  - `param2`: dove voglio renderizzarlo
    - → il contenuto di `param1` verrá messo all'interno dell'elemento indicato da `param2` → `param2` dev'essere un `DOMNode`
      - es: `document.getElementById('root')` → di solito si usa un elemento con `id="root"` come 'punto di ignezione'
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

- per usare delle variabili o espressioni `JS` in `JSX`, queste vanno wrappate in parentesi graffe (`{}`)

  - es: `<p>{text}</p>` o `<input type="{type}" />` o `<input value="{val.toFixed(2)}" />` o `<input value="{parseFloat(val)}" />`

- per usare l'attributo `HTML` `style` in `JSX`, va passato un oggetto
  - siccome un oggetto é un espressione `JS`, questo deve essere wrappato in parentesi graffe (`{}`) → ci sará un 'doppio wrapper di graffe'
  - le proprietá dell'oggetto vanno scritte in `camelCase`
  - il valore dell'attributo NON va messo tra virgolette o apici (`"" o ''`)
  - es: `<p style={{ color: blue, marginBotton: 3px }}> ... </p>`
- siccome un solo tag di primo livello puó essere ritornato, é possibile usare un `fragment`

  - → `<> JSX_HERE </>` → non renderizza nessun tag `HTML` (al contrario di un `div` wrapper che renderizzerebbe un `div`, appunto)

- usando un tool di setup, spesso si avrá la necessitá di importare gli assets nei file `JS`
  - si puo fare con uno statement di import → `import image from image.png`
    - per gli assets va specificata l'estensione
  - verrá creata una variabile nominata con il nome dell import (image nell'esempio precedente)
    - puó essere usata come una qualsiasi variabile `JS` in `JSX` → `<img src={image} />`
  - MAI inserire la path di un asset direttamente nel codice (hardcoded)
    - non funzionerebbe dopo la build in quanto il bundler 'romperebbe' il collegamento durante la creazione del bundle
    - usare gli import per prevenire questo problema
    - si potrebbero mettere gli assets in `/public/` e non incappare in questo problema (public non é soggetta al bundle) → é fortemente sconsigliato

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

### Props

- i componenti possono accettare dei dati in ingresso (cosí da essere data-driven) detti props

  - per passare dati (es variabili) a un componente come props, sará sufficente passarli come fossero attributi `HTML`
  - questo approccio garantisce la riusabilitá e la dinamicitá del componente
    - es: `<Componente titolo="esempio 1" />`
    - in questo modo ho passato la prop titolo con valore 'esempio 1' a Componente
    - potrei utilizzare di nuovo Componente passando un titolo diverso → dinamicitá
  - é possibile passare espressioni `JS` o `JSX` passando il valore tra `{}`, senza `""`
    - es: `<Componente titolo={variabile_o_espressione} />`
  - per leggere le props nel componente basta far sí che la funzione che definisce il componente accetti un parametro
    - es: `export default funcion Componente(proprieta){ ... }`
    - questo parametro sará un oggetto con proprietá uguali a quelle che abbiamo passato al componente
    - nell'esempio precedente per leggere 'titolo' ci basterá la seguente espressione → `proprieta.titolo`
    - es:
    ```js
    <Componente titolo="titolo esempio" />

    export default function Componente(proprieta){
      console.log(proprieta.titolo) //printerá 'titolo esempio'
      return ( <h1>Il titolo é {proprieta.titolo}</h1> )
    }
    ```  
    Risultato:  
    Il titolo é titolo esempio  
  
  - di solito si usa il destructuring degli oggetti per leggere le prorietá
    - per fare questo, nella dichiarazione del componente, come parametro, al posto di mettere una sola variabile, si dovrá mattere un oggetto con i nomi delle proprietá passate (devono essere uguali!)
    - es: `export default function Componente({prop1, prop2})`
    - in questo modo non sará piú necessario leggere le proprietá dall'oggetto ma si potranno usare le proprietá come una variabile a sé
    - es:
    ```js
    <Componente titolo="titolo esempio" descrizione='descrizione esempio' />

    export default function Componente({titolo, descrizione}){
      console.log(titolo) //printerá 'titolo esempio'
      console.log(descrizione) //printerá 'descrizione esempio'
      return (
        <>
          <h1>Il titolo é {titolo}</h1>
          <p>La descrizione é {descrizione}</p>
        </>
      )
    }
    ```

    Risultato:  
     Il titolo é titolo esempio  
     La descrizione é descrizione esempio  
    

## Filesystem

é possibile suddvidere i file (componenti, ...) creati in sottocartelle.  
Spesso ci sono diversi modi per organizzare il progetto, quindi la scelta é anche personale.

- si ha sempre un file `index.html` → contiene la parte 'al di fuori' di `React`

  - a seconda del tool di setup usato potrebbe trovarsi in `/public/`
  - la head del sito é accessibile in questo file
  - il body del sito é definito in questo file
  - solitamente si ha un div, subito dentro il body, con `id="root"`, che funge da punto di ignezione del contenuto generato con `React`
  - contiene il collegamento al file `JS` iniziale in cui `ReactDOM` é definito ed esegue l'istruzione di rendering

- solitamente si ha un file `index.js` o `main.js` → file `JS` iniziale linkato dal tag script in `index.html`

  - a seconda del tool di setup usato potrebbe trovarsi in `/src/`
  - contiene l'inizio dell'app `React`
  - contiene il primo import di `React`
  - contiene l'import dello stile → `index.css`, `style.css`, ...
    - import ./path_to_css/index.css
    - il bundler si occuperá di far funzionare il tutto
  - contiene l'istruzione di rendering del componente react e l'import di `ReactDOM` → `ReactDOM.render(<App />, document.querySelector('#root'))`
  - contiene l'import al componente radice (o il componente in sé) → viene usato come primo argomento nella funzione `ReactDOM.render` (punto precedente)
    - solitamente chiamato `App`
    - a volte definito nell file `index.js` stesso
    - a volte qeusto componente viene lasciato nella root del progetto e non viene, quindi, collocato insieme agli altri componenti
    - da questo componente si diramano tutti gli alti componenti, contesti, ...

- a seconda del tool di setup usato si ha un file css (`index.css`, `style.css`, ...)

  - solitamente (ma dipende dal tool usato) é situato in `/src/`
  - in questo file possono essere aggiunti gli stili o fatte le integrazioni di css framework come `Tailwind`

- a seconda del tool di setup usato si ha una cartella `src/assets/` (o simile) che conterrá gli assets che verranno importati nei file `JS`
