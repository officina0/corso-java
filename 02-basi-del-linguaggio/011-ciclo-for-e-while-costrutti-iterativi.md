# 011 Ciclo For E While Costrutti Iterativi

Se i costrutti condizionali visti nella precedente sezione rispondono all’esigenza di eseguire diverse parti di un codice subordinatamente ad una determinata condizione, i costrutti iterativi ci consentono di **eseguire ripetutamente un determinato blocco di codice** \(che, al solito, potrà essere una singola linea oppure un intero blocco racchiuso tra parentesi graffe ‘{}’\).

In Java i costrutti iterativi sono sostanzialmente 3, comunemente denominati in base alle keywords che li contraddistinguono: **while**, **do-while** e **for**.

## while

Il **ciclo while** esegue una istruzione o un blocco di codice finché rimane verificata una certa condizione. In italiano diremmo: “fino a quando la condizione è vera esegui il blocco” ecco un esempio:

while\(condizione\) {

```text
// ...
```

}

dove `condizione` deve essere una variabile o una _espressione di tipo booleano_. In questo caso quindi esprimiamo la volontà di eseguire tutte le istruzioni del blocco ripetutamente fino a quando `condizione` ha valore `true`.

Interessante capire cosa succede la prima volta che si accede al ciclo. Se `condizione` è `false` quando l’esecuzione arriva per la prima volta allo statement `while` il blocco di codice non sarà eseguito neanche una volta.

## do-while

il **ciclo do-while** è simile al `while` tanto da poter essere considerato una sua variante, ed infatti il frammento di codice:

do {

```text
// ...
```

} while\(condizione\);

analogamente a quello sopra presentato, causa l’esecuzione del blocco tra graffe fino a quando la condizione è vera ma con la importante differenza che in questo caso condizione viene presa in considerazione alla fine del blocco. Quindi l’esecuzione del blocco di istruzioni viene effettuata almeno una volta, anche se la condizione risulta da subito `false`.

Continuando il parallelo con la lingua italiana questo codice andrebbe letto: “esegui il blocco di codice e, poi, se condizione è vera fallo di nuovo, altrimenti smetti”.

## for

Il **ciclo for** è un costrutto tra i più conosciuti, comune praticamente a tutti i linguaggi e, pur servendo come i precedenti ad eseguire ripetutamente un blocco, fornisce una semplice sintassi per accomodare:

* una espressione di inizializzazione eseguita solo una volta prima di iniziare il ciclo;
* una espressione di ‘aggiornamento’ \(tipicamente un incremento\) da eseguire al termine di ogni esecuzione del blocco;
* una condizione di terminazione \(o uscita\) dall’esecuzione iterativa.

Con un codice simile al seguente:

for\(inizializzazione; condizione; incremento\) {

```text
// ...
```

}

si ottiene un programma che esegue esattamente una volta inizializzazione, esegue poi il blocco, quindi effettua l’incremento, valuta condizione e, se questa risulta vera \(`true`, come al solito condizione deve essere di tipo `boolean`\), esegue di nuovo blocco, alla fine del quale ripete il test e così via.

È semplice convincersi con un esempio che un ciclo for ed uno while sono facilmente intercambiabili:

int i=0;

while\(i &lt; 10\) {

```text
// ...

i++;
```

}

è identico a:

for\(int i=0; i&lt;10; i++\) {

```text
// ...
```

}

dove si vede anche la comune pratica di definire le variabili di iterazione \(`i` nell’esempio\) direttamente all’interno della sezione di inzializzazione rendendole locali al blocco \(ed alle sezioni di incremento e terminazione\).

### Cicli infiniti

Si osserva che essendo `inizializzazione`, `incremento` e `condizione` opzionali non è strano trovare casi in cui vengano omesse fino all’estremo:

for\(;;\) {

```text
// ...
```

}

letto anche “_for-ever_” o **ciclo infinito**, poiché la condizione di terminazione è omessa e per default considerata come `true` \(il medesimo comportamento si otterrebbe naturalmente con “`while(true) {}`“\).

## for each

Una importante variante del ciclo `for` è quella che potremmo definire **for-each** \(detta anche _for-in_\) che serve nel caso particolare \(ma comune\) in cui si voglia eseguire un determinato blocco di codice per ogni elemento di una data collezione \(o array\).

Pur dovendo rimandare una completa descrizione di questa variante a quando parleremo di _Collection_ e _array_ possiamo comunque mostrarne la sintassi:

for\( Type item : itemCollection \) {

```text
// ...
```

}

che, continuando i paralleli con la lingua italiana si legge come:

“prendi uno ad uno gli elementi della collezione `itemCollection`, assegna ciascuno di essi alla variabile `item` ed esegui per ciascun elemento il blocco \(che potrà quindi usare `item` al suo interno\)”.

### for-each, le Collection e i tipi generics

Anche se parleremo più avanti di “Collection” e “generics”, in questa fase ci basta sapere che si tratta di collezioni di oggetti alle quali si applicano i cosiddetti iteratori per effettuare una scansione di tutti gli elementi.

Ecco un esempio pratico di una tipica routine di iterazione degli elementi di una Collection che utilizzi i tipi generics:

Queue queue = new LinkedList\(\);

for\(Iterator it = queue.iterator\(\); it.hasNext\(\); \) {

String tmp = it.next\(\); // ... qui fa qualcosa }

Senza l’ausilio dei tipi generics la cosa diventa ancora più ardua, poiché bisogna effettuare il cast \(su `it.next()`\) con il rischio di un’eccezione a runtime. In realtà, seppure con la miglioria dei tipi generics, questa iterazione non è pulita in quanto ci obbliga a utilizzare una struttura dati \(Iterator\) che di fatto non utilizziamo.

Come abbiamo visto invece, il ciclo **for-each** permette una definizione automatica di tutto ciò in un solo comando integrato:

for\(String tmp:queue\) {

//... }

L’utilizzo della struttura nel secondo esempio verrà tradotto con la codifica definita nel primo, facendoci però perdere qualsiasi riferimento all’iteratore che lavorerà dietro le quinte. Si tratta quindi di una semplificazione che sicuramente dà dei benefici in termine di migliore codifica ma assolutamente non intacca le prestazioni né in positivo né in negativo.

public class ForeachTest {

```text
public static void main(String\[\] args) {

    Collection
```

coll = new ArrayList\(\);

```text
    // utilizziamo l'array degli argomenti con for-each
    // e popoliamo la collezione
    for(String tmp:args){
        coll.add(tmp);
    }

    // stampiamo la collezione
    for(String tmp:coll) {
        System.out.println(tmp);
    }
}
```

}

Per semplicità lavoriamo con un array di String \(l’array degli argomenti, `args`, del main\) e una `Collection<String>` dove ci metteremo il contenuto del primo array. Come si vede dal codice nel primo caso facciamo una semplice iterazione, utilizzando l’oggetto `tmp` \(che ha ciclo di vita all’interno del for\). Nel secondo iteriamo sulla collezione stampando il risultato.

