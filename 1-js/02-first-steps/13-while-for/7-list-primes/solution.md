Ci sono diversi algoritmi per compiere questa attività.

Usiamo un ciclo annidato:

```js
For each i in the interval {
  check if i has a divisor from 1..i
  if yes => the value is not a prime
  if no => the value is a prime, show it
}
```

Il codice usa un etichetta:

```js run
let n = 10;

nextPrime:
for (let i = 2; i <= n; i++) { // for each i...

  for (let j = 2; j < i; j++) { // look for a divisor..
    if (i % j == 0) continue nextPrime; // not a prime, go next i
  }

  alert( i ); // a prime
}
```

Ci sono molti modi per ottimizzarlo. Ad esempio, potremmo controllare i divisori di `2` fino alla radice di `i`. In ogni caso, se vogliamo essere veramete efficenti su grandi intervalli, abbiamo bisogno di cambiare approcio ed affidarci ad algoritmi matematici più avanzati e complessi, come [Quadratic sieve](https://en.wikipedia.org/wiki/Quadratic_sieve), [General number field sieve](https://en.wikipedia.org/wiki/General_number_field_sieve) etc.
