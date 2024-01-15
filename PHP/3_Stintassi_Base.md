# Sintassi Base

In questo capitolo, verranno illustrate le basi della sintassi di PHP.  

**NOTA:** per far sì che l'interprete consideri un file, questo deve avere estensione `.php`.  

## Il Tag PHP

Per iniziare a scrivere codice PHP, è necessario dar modo all'interprete di capire quale porzione di un file è, appunto, da interpretare come PHP. Vedremo perché poco più avanti.  
Per fare questo, dobbiamo aprire il tag PHP, in questo modo:
```php
<?php
```
Possiamo ora scrivere il nostro codice PHP.  

Vi è anche il rispettivo tag di chiusura, che però in certi casi è bene omettere. Vedremo in seguito quando.  
Ecco il tag di chiusura:
```php
?>
```
Tutto ciò che è all'interno dei due tag, verrà considerato codice PHP da interpretare, al contrario qualunque cosa al di fuori verrà ignorata dall'interprete.

## PHP in HTML

Qualcuno potrebbe chiedersi perché sia necessario un tag apposito per poter scrivere codice PHP.  

Il motivo dietro a tale quesito è il seguente: PHP può essere scritto dentro a del codice HTML.  
Da questa peculiarità nasce la necessità di far sapere all'interprete quale sezione del nostro file è effettivamente da considerare PHP e quale invece è da ignorare.  

Esempio:
```php
<h1> hello <?php echo $name ?> </h1>
```
In questo caso solo `echo $name` verrà considerato dall'interprete, mentre il resto del codice sarà ignorato da quest'ultimo.

### HTML in PHP in HTML
Questo stana 'frase' sta a significare che, siccome il codice PHP viene interpetato e 'sostituito' dal proprio risultato, è possibile generare HTML valido e leggibile dal browser tramite PHP.  

Esempio:
```php
<section>
    <?php
        $tag = '<h1>Hello!</h1>';
        echo $tag;
    ?>
</section>
```
dopo l'interpretazione dà come risultato
```html
<section>
    <h1>Hello!</h1>
</section>
```
che è codice HTML valido!  

 **BEST PRACTICE:** se il file è puramente PHP, il tag di chiusura va omesso, per evitare che tab, spazi, caratteri di carriage return (a capo) generino l'errore `Warning: Cannot modify header information - headers already sent`.  
 Per approfondire clicca [qui](https://stackoverflow.com/questions/8028957/how-to-fix-headers-already-sent-error-in-php).  

## Sintassi Alternativa per le Control Structures

In PHP, normalmente, le control structures vengono create con le parentesi graffe.  
Esempio:
```php
if(2 > 1) {
    echo "That's true!";
}
```

Ma è facile vedere come questa sintassi possa essere piuttosto disordinata e dispersiva quando si scrive PHP in HTML.  
Esempio:
```php
<div>
    <?php if(2 > 1) { ?>
        <p>True!</p>
    <?php } else { ?>
        <p>False!</p>
    <?php } ?>
</div>
```
Come si può notare, la leggibilità, già in un esempio così semplice, non è delle migliori: le parentesi non sono allineate con la control structure stessa, si fa fatica a capire dove inizia e finisce un blocco, l'indentazione è poco intuitiva, ...  

Ad ovviare questo problema vi è la sintassi alternativa per le control structures, che rimuove l'utilizzo delle parentesi graffe in favore di una sintassi più verbosa ma più ordinata.  
Rivediamo lo stesso esempio con la sintassi alternativa:
```php
<div>
    <?php if(2 > 1): ?>
        <p>True!</p>
    <?php else: ?>
        <p>False!</p>
    <?php endif; ?>
</div>
```
Certo, non è ancora l'ideale (e da qui la nascita dei templating engine come [Twig](https://twig.symfony.com/), [Latte](https://latte.nette.org/it/), [Blade](https://laravel.com/docs/10.x/blade), ...), ma è sicuramente meno disordinato.

**CONVENZIONE:** la sintassi alternativa viene usata solo quando si scrive PHP in HTML, mai quando si scrive PHP a sè stante.

## Commenti
TODO

## Apici Singoli vs Doppi
TODO
