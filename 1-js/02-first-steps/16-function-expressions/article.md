# Espressioni di funzione

In JavaScript, una funzione non è una "struttura magica del linguaggio", ma un valore speciale.

La sintassi che abbiamo utilizzato prima vien chiamata *Dichiarazione di Funzione*:

```js
function sayHi() {
  alert( "Hello" );
}
```

E' disponibile un'altra sintassi per creare una funzione chiamata *Espressione di Funzione*.

Ed appare cosi:

```js
let sayHi = function() {
  alert( "Hello" );
};
```

Qui la funzione viene creata ed assegnata ad una variabile esplicitamente proprio come qualsiasi altro valore. Non ha importanza come la funzione viene definita, è solo una valor salvato nella variabile `sayHi`.

Il significato di questo esempio è lo stesso di: "creare una funzione e metterla dentro la variabile `sayHi`".

Possiamo anche stamparne il valore usando `alert`:

```js run
function sayHi() {
  alert( "Hello" );
}

*!*
alert( sayHi ); // mostra il codice della funzione
*/!*
```

Da notare che l'ultima riga di codice non esegue la funzione, perchè non ci sono parentesi dopo `sayHi`. Ci sono linguaggi di programmazione in cui la semplice menzione del nome di funzione ne causa l'esecuzione, JavaScript non si comporta cosi.

In JavaScript, una funzione è un valore, quindi possiamo trattarla come un valore. Il codice sopra ne mostra la sua rappresentazione in stringa, cioè il codice contenuto dentro la funzione.

E' chiaramente un valore speciale, poichè possiamo richiamarla con `sayHi()`.

Ma rimane comunque un valore. Quindi possiamo trattarlo come un qualsiasi altro valore.

Possiamo copiare la funzione in un'altra variabile:

```js run no-beautify
function sayHi() {   // (1) creazione
  alert( "Hello" );
}

let func = sayHi;    // (2) copia

func(); // Hello     // (3) esegue la copia (funziona)!
sayHi(); // Hello    //     anche questa continua a funzionare (ed è giusto che sia cosi)
```

Quello che succede nel dettaglio è:

1. La dichiarazione di funzione `(1)` crea la funzione e la inserisce nella variabile denominata `sayHi`.
2. La linea `(2)` la copia nella variabile `func`.

    Ancora una volta: non ci sono parentesi dopo `sayHi`. Se ci fossero state, allora `func = sayHi()` avrebbe scritto *il risultato della chiamata* `sayHi()` in `func`, non *la funzione* `sayHi`.
3. Adesso la funzione può essere richiamata sia con `sayHi()` che con `func()`.

Avremmo anche potuto utilizzare un espressione di funzione per dichiarare `sayHi`, nella prima riga:

```js
let sayHi = function() {
  alert( "Hello" );
};

let func = sayHi;
// ...
```

Tutto funzionerebbe ugualmente. Risulta anche più chiaro cosa sta succedendo, giusto?


````smart header="Perchè c'è un punto e virgola alla fine?"
Vi starete chiedendo perchè con l'espressione di funzione bisogna mettere `;` alla fine, mentre con la dichiarazione di funzione non serve:

```js
function sayHi() {
  // ...
}

let sayHi = function() {
  // ...
}*!*;*/!*
```

La risposta è semplice:
- Non c'è bisogno di `;` alla fine dei blocchi di codice che utilizzano una sintassi del tipo `if { ... }`, `for {  }`, `function f { }` etc.
- Un espressione di funzione viene utilizzata all'interno di un istruzione: `let sayHi = ...;`, come valore. Non è un blocco di codice. Quindi il `;` è consigliato al termine dell'istruzione, indipendentemente dal valore. Quindi il punto e virgola non è collegato all'espressione di funzione, più semplicemente termina un istruzione.
````

## Funzioni richiamate

Diamo un occhiata ad ulteriori esempi di passaggio di funzione come valore e utilizzo di espressioni di funzione.

Scriveremo una funzione `ask(question, yes, no)` con tre parametri:

`question`
: Il testo della domanda

`yes`
: Funzione da eseguire se la risposta è "Yes"

`no`
: Funzione da eseguire se la risposta è "No"

La funzione dovrebbe richiedere la `question` e, in base alla risposta dell'utente, chiamare `yes()` o `no()`:

```js run
*!*
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}
*/!*

function showOk() {
  alert( "You agreed." );
}

function showCancel() {
  alert( "You canceled the execution." );
}

// utilizzo: funzioni showOk, showCancel vengono passate come argomenti ad ask
ask("Do you agree?", showOk, showCancel);
```

Prima abbiamo visto come riscrivere le funzioni in maniera più breve, da notare che nel browser (e in alcuni casi anche lato server) queste funzioni risultano molto popolari. La principale differenza tra un implementazione realistica e gli esempi sopra è solo che le funzioni reali utilizzano modalità più complesse per interagire con l'utente, non un semplice `confirm`. In ambiente browser, queste funzioni spesso mostrano delle interrogazioni molto carine. Ma questo è un altro discorso.

**Gli argomenti `showOk` e `showCancel` della `ask` sono chiamati *funzioni di richiamo* o semplicemente *callbacks*.**

L'idea è che passiamo una funzione e ci aspettiamo di "richiamarla" più tardi se necessario. Nel nostro caso `showOk` diventa la callback per la risposta "yes", e `showCancel` per la risposta "no".

Possiamo utilizzare un espressione di funzione per scrivere la stessa funzione più in breve:s

```js run no-beautify
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

*!*
ask(
  "Do you agree?",
  function() { alert("You agreed."); },
  function() { alert("You canceled the execution."); }
);
*/!*
```


Qui la funzione viene dichiarata dentro alla chiamata di `ask(...)`. Queste non hanno nome, e sono denominate *anonime*. Queste funzioni non sono accessibili dall'esterno di `ask` (perchè non sono assegnate a nessuna variabile), ma è proprio quello che vogliamo in questo caso.

Questo tipo codice apparirà nei nostri script molto naturalmente, è nello spirito del JavaScript.


```smart header="Una funzione è un valore che rappresenta un \"azione\""
I valori regolari come le stringhe o i numeri rappresentano *dati*.

Una funzione può anche essere vista come un *azione*.

Possiamo passarla tra le variabili ed eseguirla quando vogliamo.
```


## Espressione di Funzioni vs Dichiarazione di Funzioni

Cerchiamo di elencare le differenze chiave tra Dichiarazioni ed Espressioni di Funzione.Expressions.

Primo, la sintassi: come capire cosa è cosa nel codice.

- *Dichiarazione di funzione:* una funzione, dichiarata come un istruzione separata, nel flusso principale del programma.

    ```js
    // Dichiarazione di funzione
    function sum(a, b) {
      return a + b;
    }
    ```
- *Espressione di funzione:* una funzione, creata all'interno di un espressione o all'interno di un altro costrutto. Qui, la funzione è creata alla destra dell' "espressione di assegnazione" `=`:
    
    ```js
    // Espressione di funzione
    let sum = function(a, b) {
      return a + b;
    };
    ```

La differenza più subdola è *quando* una funzione viene creata dal motore JavaScript engine.

**Un espressione di funzione viene creata quando l'esecuzione la raggiunge ed è utilizzabile da quel momento in poi.**

Quando il flusso di esecuzione passa alla destra dell'operatore di assegnazione `let sum = function…` -- , la funzione viene creata e può essere utilizzata (assegnata, chiamata, etc...) a partire da quel momento.

La dichiarazione di funzione si comporta diversamente.

**Una dichiarazione di funzione è utilizzabile nell'intero script/blocco di codice.**

In altre parole, quando JavaScript si *prepara* ad eseguire lo script o un blocco di codice, come prima cosa guarda le dichiarazioni di funzione contenute e le crea. Possiamo pensare a questo processo come uno "stage di inizializzazione".

E dopo che tutte le dichiarazioni di funzione sono state processate, l'esecuzione potrà procedere.

Come risultato, una funzione creata come dichiarazione di funzione può essere richiamata anche prima della sua definizione.

Ad esempio il seguente codice funziona:

```js run refresh untrusted
*!*
sayHi("John"); // Hello, John
*/!*

function sayHi(name) {
  alert( `Hello, ${name}` );
}
```

La dichiarazione di funzione `sayHi` viene creata quando JavaScript si sta preparando ad eseguire lo script ed è visibile in qualsiasi punto.

...Se fosse stata un espressione di funzione, non avrebbe funzionato:

```js run refresh untrusted
*!*
sayHi("John"); // errore!
*/!*

let sayHi = function(name) {  // (*) nessuna magia
  alert( `Hello, ${name}` );
};
```

Le espressioni di funzione sono create quando l'esecuzione le incontra. In questo esempio avverà solo alla riga `(*)`. Troppo tardi.

**Quando una dichiarazione di funzione viene fatta all'interno di un blocco di codice, sarà visibile ovunque all'interno del blocco, ma non al suo esterno.**

Qualche volta è comodo dichiarare funzioni locali utili in un singolo blocco. Ma questa caratteristica potrebbe causare problemi.

Ad esempio, immaginiamo di aver bisogno di dichiarare una funzione `welcome()` in base ad un parametro `age` che valuteremo a runtime. E abbiamo intenzione di utilizzarlo più avanti.

Il codice sotto non funziona:

```js run
let age = prompt("What is your age?", 18);

// dichiarazione di funzione condizionale
if (age < 18) {

  function welcome() {
    alert("Hello!");
  }

} else {

  function welcome() {
    alert("Greetings!");
  }

}

// ...utilizzo successivo
*!*
welcome(); // Errore: welcome non è definita
*/!*
```

Questo accade perchè una dichiarazione di funzione è visibile solamente all'interno del blocco di codice in cui è scritta.

Un altro esempio:

```js run
let age = 16; // prendiamo 16 come esempio

if (age < 18) {
*!*
  welcome();               // \   (esegue)
*/!*
                           //  |
  function welcome() {     //  |  
    alert("Hello!");       //  |  Dichiarazione di funzione disponibile
  }                        //  |  ovunque nel blocco in cui è stata dichiarata
                           //  |
*!*
  welcome();               // /   (esegue)
*/!*

} else {

  function welcome() {     
    alert("Greetings!");
  }
}

// Qui siamo all'esterno delle parentesi graffe, 
// quindi non possiamo vedere le dichiarazioni di funzione fatte al suo interno.

*!*
welcome(); // Errore: welcome non è definita
*/!*
```

Cosa possiamo fare per rendere visibile `welcome` all'esterno del blocco `if`?

Il giusto approccio è quello di utilizzare un espressione di funzione ed assegnarla ad una variabile `welcome` che viene dichiarata all'esterno di `if` ed ha quindi la corretta visibilità.

Ora funziona:

```js run
let age = prompt("What is your age?", 18);

let welcome;

if (age < 18) {

  welcome = function() {
    alert("Hello!");
  };

} else {

  welcome = function() {
    alert("Greetings!");
  };

}

*!*
welcome(); // ora funziona
*/!*
```

Oppure possiamo semplificarla con l'utilizzo dell'operatore `?`:

```js run
let age = prompt("What is your age?", 18);

let welcome = (age < 18) ?
  function() { alert("Hello!"); } :
  function() { alert("Greetings!"); };

*!*
welcome(); // ora funziona
*/!*
```


```smart header="Quando conviene scegliere un dichiarazione di funzione vs espressione di funzione?"
Come regola fondamentale, quando abbiamo la necessita di dichiarare una funzione, la prima opzione da considerare è la dichiarazione di funzione. Fornisce maggiore libertà sul come organizzare il codice, poichè possiamo utilizzare quella funzione anche prima della sua dichiarazione.

Risulta anche più facile vedere `function f(…) {…}` nel codice, piuttosto che `let f = function(…) {…}`. La dichiarazione di funzione è più facile da "notare".

...Ma se per qualche ragione la dichiarazione di funzione non si applica bene al caso in questione (abbiamo visto degli esempi sopra), allora l'espressione di funzione può essere utilizzata.
```

## Summary


Nella maggior parte dei casi quando abbiamo bisogno di dichiarare una funzione, una dichiarazione di funzione è preferibile, poichè è visibile anche prima della riga di dichiarazione. Questo ci fornisce più flessibilità nell'organizzazione del codice, e solitamente risulta più leggibile.

Quindi dovremmo utilizzare un espressione di funzione solo quando una dichiarazione di funzione non è adatta al caso specifico. Abbiamo visto un paio di esempi in questo capitolo, e ne vederemo altri in futuro.
