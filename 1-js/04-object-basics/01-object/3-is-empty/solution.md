E' sufficiente eseguire un ciclo e ritornare `false` se l'oggetto contiene almeno una proprietà.

```js
function isEmpty(obj) {
  for (let key in obj) {
    return false;
  }
  return true;
}
```
