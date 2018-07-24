# 012 Break E Continue Costrutti Di Branching

**Break e continue** sono comunemente detti **costrutti di branching** ed in Java servono a controllare l’esecuzione di un blocco di codice in un modo sostanzialmente opposto a come operano i costrutti iterativi e condizionali.

Questi ultimi infatti, focalizzano sull’esecuzione \(eventuelmente ripetuta\) di un intero blocco di codice, mentre `break` e `continue` servono per _terminare_ \(in un senso che sarà chiarito sotto\) l’esecuzione di un blocco di codice in un ciclo o uno statement switch.

## break

Lo statement **break** \(già visto nella sua forma più semplice e comune quando abbiamo parlato di `switch-case`\) serve per terminare l’esecuzione di uno o più blocchi di codice. In altre parole serve per “saltare fuori” da costrutti iterativi o `switch-case`.

La forma completa di break, detta _labeled_ per la presenza di una etichetta, è la seguente:

etichetta: loop-or-switch {

```text
    // blocco prima del break

    loop-or-switch-2 {

        // ...
        break etichetta;
    }

    // blocco dopo il break 
    // (senza etichetta il programma salterebbe qui!) 
 }
```

// fine del blocco etichettato \(il programma salta qui!\)

dove:

* `etichetta` è un identificatore \(come definito nella sezione sulle variabili\) e serve per assegnare un nome allo statement in modo che possa essere utilizzato per specificare quale blocco si vuole terminare al momento del `break`;
* `loop-or-switch` può essere un costrutto tra `for`, `while`, `do`, `switch`

Quando in un programma viene trovato un break etichettato l’esecuzione del codice letteralmente salta alla fine del blocco contrassegnato dall’etichetta \(quindi se ci sono più blocchi innestati salta alla fine del livello identificato dall’etichetta\).

Se l’etichetta è omessa, per default, il programma salta alla fine del blocco corrente.

## continue

Similmente a `break` lo statement **continue** serve per saltare alla fine di un blocco ma in questo caso non viene terminato il ciclo ma solamente **interrotta l’iterazione corrente** e l’esecuzione salta immediatamente alla valutazione della condizione di terminazione. Eventualmente poi si procederà ad una nuova iterazione.

Naturalmente non avrebbe senso utilizzare `continue` con lo statement `switch` ed infatti è sintatticamente proibito.

La peculiarità di `continue` è forse più facilmente comprensibile per mezzo dei seguenti esempi:

int sum = 0;

for\(int i = 1; i &lt; 10; i++\) {

```text
if(i%2 == 0) {

    break; // il blocco che viene interrotto con break
           // è quello "corrente" nel senso 
           // "del ciclo corrente" (il for)
}

sum++;
```

}

System.err.println\(sum\);

Questo snippet produce come output “`1`“, quindi le istruzioni dopo il break sono state eseguite solamente una volta mentre in questo esempio in cui sostituiamo `break` con `continue`:

int sum = 0;

for\(int i = 1; i &lt; 10; i++\) {

```text
if(i % 2 == 0) {    

    continue;
}
sum++;
```

} System.err.println\(sum\);

otteniamo come output “`5`“. Questa volta le istruzioni dopo `continue` sono state eseguite per tutti i numeri tra 1 e 10 \(escluso\) che non sono divisibili per 2 \(1,3,5,7,9 appunto 5 volte\).

Anche per `continue` esiste una versione _labeled_ del tutto simile a quella vista per brerak che consente di saltare alla fine della corrente iterazione del ciclo contrassegnato dall’etichetta.

Nonostante esistano contesti in cui break e continue sono molto comodi \(per alcuni si veda anche [qui](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html)\) si osserva che la leggibilità del codice viene di solito molto ridotta dal loro uso \(sicuramente fatta eccezione per l’istruzione `switch` in cui `break` è spesso chiaramente indispensabile\) e quindi prima di cimentarsi in strutture complicate da leggere \(e quindi da documentare e soprattutto mantenere\) si invita a riflettere se non esista un approccio più chiaro.

