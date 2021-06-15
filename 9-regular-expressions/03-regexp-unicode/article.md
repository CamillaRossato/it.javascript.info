
# Unicode: flag "u"

La flag unicode `/.../u` abilita il corretto supporto delle coppie surrogate.

Qui vi sono i valori unicode da comparare:

| Carattere  | Unicode | Bytes  |
|------------|---------|--------|
| `a` | 0x0061 |  2 |
| `≈` | 0x2248 |  2 |
|`𝒳`| 0x1d4b3 | 4 |
|`𝒴`| 0x1d4b4 | 4 |
|`😄`| 0x1f604 | 4 |

Dunque caratteri come `a` e `≈` occupano 2 bytes, e quelli rari ne occupano 4.

Unicode è stato fatto in modo tale che i caratteri a 4 byte abbiano un significato solo considerando l'intero insieme.

In precedenza JavaScript non ne sapeva nulla, e molti metodi delle stringhe ancora presentano problemi. Per esempio, `length` pensa che qui ci siano due caratteri:

```js run
alert('😄'.length); // 2
alert('𝒳'.length); // 2
```

...Ma possiamo vedere che ce n'è solo uno, giusto? Il punto è che `length` tratta i caratteri a 4 byte come due caratteri a 2-byte. Questo non è corretto, perché devono essere considerati solo insieme (per cui chiamati "coppie surrogate").

Usualmente, anche le espressioni regolari trattano questi "caratteri lunghi" come due caratteri a 2-byte.

Questo porta a strani risultati, ad esempio proviamo a cercare `pattern:[𝒳𝒴]` nella stringa `subject:𝒳`:

```js run
alert( '𝒳'.match(/[𝒳𝒴]/) ); // risultato strano (in realtà è una corrispondenza errata, "mezzo carattere")
```

Il risultato è errato, perché di default il motore delle regexp non comprende le coppie surrogate.

Dunque, pensa che `[𝒳𝒴]` non siano due, ma quattro caratteri:
1. la metà sinistra di `𝒳` `(1)`,
2. la metà destra di `𝒳` `(2)`,
3. la metà sinistra di `𝒴` `(3)`,
4. la metà destra di `𝒴` `(4)`.

Li possiamo elencare così:

```js run
for(let i=0; i<'𝒳𝒴'.length; i++) {
  alert('𝒳𝒴'.charCodeAt(i)); // 55349, 56499, 55349, 56500
};
```

<<<<<<< HEAD
Quindi trova solo la "metà sinistra" di `𝒳`.
=======
Here's the main character categories and their subcategories:

- Letter `L`:
  - lowercase `Ll`
  - modifier `Lm`,
  - titlecase `Lt`,
  - uppercase `Lu`,
  - other `Lo`.
- Number `N`:
  - decimal digit `Nd`,
  - letter number `Nl`,
  - other `No`.
- Punctuation `P`:
  - connector `Pc`,
  - dash `Pd`,
  - initial quote `Pi`,
  - final quote `Pf`,
  - open `Ps`,
  - close `Pe`,
  - other `Po`.
- Mark `M` (accents etc):
  - spacing combining `Mc`,
  - enclosing `Me`,
  - non-spacing `Mn`.
- Symbol `S`:
  - currency `Sc`,
  - modifier `Sk`,
  - math `Sm`,
  - other `So`.
- Separator `Z`:
  - line `Zl`,
  - paragraph `Zp`,
  - space `Zs`.
- Other `C`:
  - control `Cc`,
  - format `Cf`,
  - not assigned `Cn`,
  - private use `Co`,
  - surrogate `Cs`.


So, e.g. if we need letters in lower case, we can write `pattern:\p{Ll}`, punctuation signs: `pattern:\p{P}` and so on.

There are also other derived categories, like:
- `Alphabetic` (`Alpha`), includes Letters `L`, plus letter numbers `Nl` (e.g. Ⅻ - a character for the roman number 12), plus some other symbols `Other_Alphabetic` (`OAlpha`).
- `Hex_Digit` includes hexadecimal digits: `0-9`, `a-f`.
- ...And so on.

Unicode supports many different properties, their full list would require a lot of space, so here are the references:

- List all properties by a character: <https://unicode.org/cldr/utility/character.jsp>.
- List all characters by a property: <https://unicode.org/cldr/utility/list-unicodeset.jsp>.
- Short aliases for properties: <https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt>.
- A full base of Unicode characters in text format, with all properties, is here: <https://www.unicode.org/Public/UCD/latest/ucd/>.

### Example: hexadecimal numbers

For instance, let's look for hexadecimal numbers, written as `xFF`, where `F` is a hex digit (0..9 or A..F).

A hex digit can be denoted as `pattern:\p{Hex_Digit}`:

```js run
let regexp = /x\p{Hex_Digit}\p{Hex_Digit}/u;

alert("number: xAF".match(regexp)); // xAF
```
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

In altre parole, la ricerca funziona come `'12'.match(/[1234]/)`: solo `1` viene restituito.

## La flag "u"

La flag `/.../u` risolve questo problema.

Essa abilita le coppie surrogate nel motore delle regexp, in modo tale che il risultato sia:

```js run
alert( '𝒳'.match(/[𝒳𝒴]/u) ); // 𝒳
```

Vediamo un altro esempio.

Se dimentichiamo la flag `u` e occasionalmente usiamo le coppie surrogate, possiamo incorrere in errori:

```js run
'𝒳'.match(/[𝒳-𝒴]/); // SyntaxError: intervallo non valido nella classe di caratteri
```

Di solito, le regexp interpretano `[a-z]` come un "intervallo di caratteri con codici tra `a` e `z`.

Ma senza la flag `u`, le coppie surrogate vengono interpretate come "coppie di caratteri indipendenti", quindi `[𝒳-𝒴]` è come `[<55349><56499>-<55349><56500>]` (sostituito a ogni coppia surrogata il codice corrispondente). Ora possiamo vedere con più chiarezza che l'intervallo `56499-55349` non è accettabile, dato che il valore a sinistra dell'intervallo deve essere inferiore rispetto a quello a destra.

Usando la flag `u` tutto funziona di nuovo:

```js run
alert( '𝒴'.match(/[𝒳-𝒵]/u) ); // 𝒴
```
