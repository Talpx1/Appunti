# Concetti Introduttivi

In questa sezione verranno brevemente esposti alcuni concetti utili o necessari al fine della comprensione del resto degli appunti.  

## Web Server  

I web server sono dei servizi che vengono installati sui server, al fine di poter gestire in maniera configurabile le richieste web (ovvero la comunicazione client/server via HTTP).  
Tra i più utilizzati, si possono citare Apache e Nginx.  

### Funzionamenti base di un Web Server  

Quando viene fatta una richiesta per una risorsa (file, cartella, ...) a un web server, questo agirà in uno dei seguenti modi (si intende senza configurazioni che ne alterano il comportamento):

- Se non viene specificato un file, il web server proverà a cercare un file index.php o index.php nella directory specificata, se questo non esiste, verrà stampata una rappresentazione grafica del filesystem.  
- Se viene specificato un file, il web server proverà a renderlo disponibile. Se questo file non dovesse esistere, verrà restituito un errore.  
- Se viene specificata una directory, il web server proverà ad aprirla, cercando il file specificato o il file index al suo interno. Se questa cartella non esiste verrà stampato un errore. Se esiste ma non ha un file index , verrà stampata una rappresentazione grafica del filesystem all'interno di quella cartella.  


### Virtual Host  

I virtual host sono file di configurazione del web server, che hanno lo scopo di gestire le richieste in entrata per permettere di risolverle, ritornando una risposta adeguata.  

È possibile avere più virtual host, spesso uno per ogni dominio o applicazione ospitata sul server.  

I virtual host configurano il web server per ascoltare una porta, alla volta di richieste in arrivo.  
Quando una di tali richieste viene rilevata, il virtual host più opportuno andrà a gestire l'instradamento di tale richiesta.  
Questo avviene secondo alcuni parametri come, e non solo, porta usata per la richiesta e uri della risorsa richiesta.  

Quando una richiesta viene intercettata da un virtual host, si eseguono le direttive definite nello stesso, per poi instradare la richiesta e ritornare una risposta.

#### Document Root  

Spesso ogni virtual host definisce una document root diversa per il dominio o applicazione per cui va a gestire le richieste.  

Una document root non è altro che la directory nella quale verranno re-instradate tutte le richieste gestite da quello stesso virtual host.

Es:
```
File system:
/var/www/html/
|____ dominio1/ --> files per il dominio 1
|____ dominio2/ --> files per il dominio 2

Virtual Host dominio1:
<VirtualHost *:80>
    ...
    DocumentRoot "/var/www/html/dominio1/"
    ...
</VirtualHost>

Virtual Host dominio2:
<VirtualHost *:80>
    ...
    DocumentRoot "/var/www/html/dominio2/"
    ...
</VirtualHost>

```  

Nell'esempio sopra, tutte le richieste per dominio1 verranno indirizzate dentro alla cartella `/var/www/html/dominio1/`, mentre quelle per dominio2 nella cartella `/var/www/html/dominio2/`.  
Se, quindi, si tenta di accedere al file `test.php`, tramite uri `dominio1.com/test.php`, il web server cercherà il file `test.php` nella cartella `/var/www/html/dominio1/`.  


### Files .htaccess  

i file .htaccess hanno lo scopo di sovrascrivere le configurazioni del web server, per la specifica cartella nella quale sono ospitati.  
Spesso vengono usati nella document root, per forzare tutte le richieste, qualunque sia la risorsa richiesta, ad eseguire il file index.php (che gestirà poi autonomamente la richiesta), creando un design pattern chiamato Front Controller.  

## Ambienti di Sviluppo

Per sviluppare in PHP, non è necessaria alcuna configurazione nè strumento specifico.  
Tecnicamente, è possibile scrivere codice PHP sul classico blocco note preinstallato su ogni distribuzione.  

Per eseguire il codice, invece, serve un interprete PHP installato sul server.  
Per scopi di sviluppo, il server può essere simulato in più modi:
- usando un tool pre-configurato come [Laragon](https://laragon.org/index.html), [XAMPP](https://www.apachefriends.org/it/index.html) o [EasyPHP](https://www.easyphp.org/).  
- Usando Docker.  

Come editor per la stesura di codice si possono usare, per esempio, [Visual Studio Code](https://code.visualstudio.com/) con le estensioni elencate sotto, oppure [PHPStorm](https://www.jetbrains.com/phpstorm/),  

### Estensioni utili allo sviluppo PHP per Visual Studio Code

- [Composer](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.composer-php-vscode): per integrare [composer](https://getcomposer.org/) direttamente in VSCode.  
- [Better Pest](https://marketplace.visualstudio.com/items?itemName=m1guelpf.better-pest): per il supporto alla suite di testing [PestPHP](https://pestphp.com/).  
- [IntelliPHP](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.intelli-php-vscode): per suggerimenti di auto completamento usando l'IA (a pagamento).  
- [PHP](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.phptools-vscode): per garantire diverse funzioni utili e un vasto supporto del linguaggio (esiste sia la versione free che premium). 
- [PHP DocBlocker](https://marketplace.visualstudio.com/items?itemName=neilbrayfield.php-docblocker): per offrire un migliore supporto ai DocComment/PHPDoc/DocBlock.  
- [PHP Profiler](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.profiler-php-vscode): per il supporto di vari tool, tra cui [xDebug](https://xdebug.org/) e [PHPUnit](https://phpunit.de/), direttamente da VSCode.  
- [phpstan](https://marketplace.visualstudio.com/items?itemName=SanderRonde.phpstan-vscode): per il supporto di [PHPStan](https://phpstan.org/) (tool di analisi statica) direttamente da VSCode.  
- [phpcs](https://marketplace.visualstudio.com/items?itemName=shevaua.phpcs): per il supporto di [PHPCS](https://github.com/squizlabs/PHP_CodeSniffer) direttamente da VSCode
- [php cs fixer](https://marketplace.visualstudio.com/items?itemName=junstyle.php-cs-fixer): per il supporto di [PHP CS Fixer](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer) direttamente da VSCode

### Altre estensioni utili per Visual Studio Code
- [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag): rinomina automaticamente i tag HTML o XML di apertura e chiusura, quando uno dei due viene modificato.  
- [Better Comments](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments): aggiunge colori personalizzati a particolari tipi di commento come i TODO, FIXME, ...  
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker): controlla errori grammaticali nel codice (di default solo in inglese).
- [Italian - Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker-italian): aggiunge il supporto alla lingua italiana per Code Spell Checker (vedi estensione precedente).  
- [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers): permette di accedere e sviluppare nei container direttamente da VSCode.  
- [GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens): aggiunge molte funzioni utili relative a git e GitHub, integrandole direttamente in VSCode.  
- [Highlight Matching Tag](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag): evidenzia i tag HTML di apertura e chiusura, utile quando un tag ha molto contenuto.    
- [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow): permette di aggiungere una colorazione all'indentazione per migliorare il feedback visivo dei blocchi di codice nestati.  
- [IntelliCode](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode): autocomplete intelligente per Python, TypeScript/JavaScript and Java (utile quando si sviluppa il frontend).  
- [IntelliCode API Usage Examples](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.intellicode-api-usage-examples): fornisce esempi reali per l'utilizzo di alcune API di Python, JavaScript e TypeScript (utile quando si sviluppa il frontend).  
- [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense): autocomplete intelligente per i percorsi, le directory e i files.  
- [Phind.com - Code faster with AI.](https://marketplace.visualstudio.com/items?itemName=phind.phind): IA [phind.com](https://phind.com) integrata in VSCode, con rilevamento del contesto di sviluppo.  
- [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss): intellisense per Tailwind CSS.  
- [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight): evidenzia i commenti TODO ecc. per renderli più appariscenti.   
- [Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree): elenca i TODO, HACK, FIXME, ... inseriti nel codice, raggruppandoli in un apposita tab di VSCode.  
- [VS Sequential Number](https://marketplace.visualstudio.com/items?itemName=neptunedesign.vs-sequential-number): permette di inserire sequenze numeriche quando si hanno più cursori attivi.  
- [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl): permette di accedere e sviluppare direttamente su WSL, da VSCode.  
- [Peacock](https://marketplace.visualstudio.com/items?itemName=johnpapa.vscode-peacock): permette di modificare i colori dell'istanza di VSCode, utile quando si lavora su un progetto con più ambienti/servizi e si hanno diverse finestre di VSCode aperte.  