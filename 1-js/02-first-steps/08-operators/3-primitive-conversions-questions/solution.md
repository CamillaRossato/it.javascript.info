
```js no-beautify
"" + 1 + 0 = "10" // (1)
"" - 1 + 0 = -1 // (2)
true + false = 1
6 / "3" = 2
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
7 / 0 = Infinity
"  -9  " + 5 = "  -9  5" // (3)
"  -9  " - 5 = -14 // (4)
null + 1 = 1 // (5)
undefined + 1 = NaN // (6)
" \t \n" - 2 = -2 // (7)
```

1. L'addizione con una stringa `"" + 1` converte `1` a stringa: `"" + 1 = "1"`, applichiamo la stessa regola a `"1" + 0`.
2. La sottrazione `-` (come molte altre operazioni matematiche) funziona solamente con i numeri, quindi una stringa vuota come: `""` viene convertita in `0`.
3. La somma con una stringa appende in coda alla stringa il numero `5`.
4. La sottrazione converte sempre in numero, quindi `"  -9  "` diventa `-9` (gli spazi vuoti vengono ignorati).
5. `null` diventa `0` dopo la conversione numerica.
6. `undefined` diventa `NaN` dopo la conversione numerica.
7. Gli spazi all'inizio e alla fine di una stringa vengono rimossi quando questa viene convertita ad numero. In questo caso l'intera stringa è composta da spazi, come `\t`, `\n` ed uno "spazio" normale tra di essi. Quindi allo stesso modo di una stringa vuota, diventa `0`.
