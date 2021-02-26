importance: 5

---

# Secondo bind

Possiamo cambiare `this` con una associazione addizionale?

Quale sarà l'output?

```js no-beautify
function f() {
  alert(this.name);
}

f = f.bind( {name: "John"} ).bind( {name: "Ann" } );

f();
```
