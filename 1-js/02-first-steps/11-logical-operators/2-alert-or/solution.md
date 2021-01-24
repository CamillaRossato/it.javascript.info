La risposta: prima `1`, poi `2`.

```js run
alert( alert(1) || 2 || alert(3) );
```

La chiamata ad `alert` non ritorna alcun valore; ossia `undefined`.

1. Il primo OR `||` valuta l'operando sinistro `alert(1)`. Questo mostra il primo messaggio, `1`.
2. La funzione `alert` ritorna `undefined`, quindi OR prosegue con il secondo operando, alla ricerca di un valore vero.
3. Il secondo operando `2` è vero; quindi l'esecuzione si ferma, viene ritornato `2` e mostrato dall'alert esterno.

Non ci sarà il `3`, perchè la valutazione non arriva a `alert(3)`.
