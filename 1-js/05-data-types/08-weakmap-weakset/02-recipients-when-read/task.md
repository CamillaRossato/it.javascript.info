importance: 5

---

# Memorizzare le date di lettura

Abbiamo un array di messaggi come nel [compito precedente](info:task/recipients-read). La situazione è simile.

```js
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];
```

Ora la domanda è: quale struttura di dati converrebbe utilizzare per memorizzare l'informazione: "quando è stato letto il messaggio?".

Nel compito precedente la necessità era semplicemente di memorizzare la lettura del messaggio. Ora abbiamo bisogno di memorizzare anche la data; anche in questo caso, se il messaggio viene eliminato questa dovrebbe sparire.
