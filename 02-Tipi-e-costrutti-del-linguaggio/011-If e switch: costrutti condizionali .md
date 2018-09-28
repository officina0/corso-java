Possiamo pensare all’esecuzione del codice Java come alla lettura di un libro, che avviene dall’alto verso il basso. I costrutti condizionali sono delle espressioni che consentono di alterare l’usuale modo di esecuzione di un programma e di **eseguire una certa porzione di codice in base ad una “scelta”**.

I costrutti condizionali sono quindi semplicemente il modo di tradurre sotto forma di codice algoritmi simili al seguente:

```
SE condizione1
   codice da eseguire se condizione1 è soddisfatta  
SE INVECE condizione2
   codice da eseguire se condizione2 è soddisfatta  
...
ALTRIMENTI
      codice da eseguire se tutte le precedenti condizioni non sono  soddisfatte
```

In Java esistono sostanzialmente 2 costrutti condizionali, **if-else** (o _if-then-else_)e **switch-case**, in questa lezione li esamineremo entrambi.

Il costrutto if in Java
-----------------------

Iniziamo da **if-else**. A volte si tende a chiamare questo costrutto condizionale _if-then-else_ anche se la keyword `then` non esiste. Lo esamineremo iniziando con il tradurre in Java l’algoritmo che abbiamo scritto in precedenza, in questo modo:

```
if(condizione1) {
  // ...
} else if (condizione2) {
  // ...
} else {
  //...
}
```

Fatto ciò possiamo esplorare le caratteristiche sintattiche di questo costrutto, grazie a qualche osservazione.

### Le condizioni sono espressioni di tipo boolean

Iniziamo con il dire che tutte le “condizioni” devono essere espressioni di tipo `boolean` (quindi risultare in `true` o `false`).

Ecco qualche esempio:

```
int a = ...;
int b = ...;
// OK! Sintassi corretta!
if(a == b) {
	...
}
// NO! Sintassi errata: 'a' è un 'int' e non un 'boolean'
if(a) {
}
// mentre è valida la scrittura
boolean c = ...;
if(c) {
...
}
```

Meglio specificare questo particolare soprattutto per chi è abituato a linguaggi come C++ o JavaScript in cui espressioni di diversi tipi possono verificare le condizioni degli `if`.

### Precedenza negli if multipli

Gli `if` sono valutati in successione e sarà eseguito solamente il primo per il quale la condizione risultarà vera:

```
boolean c1 = true;
boolean c2 = true;
boolean c3 = false;
if(c1) {
	// il blocco viene eseguito
} else if(c2) {
   // il blocco non viene eseguito anche se 'c2' è 'true' 
}
if(c3) {
    // il blocco non viene eseguito perché 'c3' è 'false'
} else if(c2) {
	// il blocco viene eseguito, poiché 'c2' è 'true' mentre 'c3' è 'false'
}
```

### ‘else’ e ‘else if’ non sono obbligatori

Ci può essere un numero arbitrario di `else if` (il nostro “_SE INVECE_“), anche nessuno.

Inoltre ci può essere un solo `else` e non è obbligatorio che ci sia

### Istruzioni e blocchi di istruzioni

Al verificarsi di una condizione il costrutto `if` esegue l’istruzione oppure il blocco di istruzioni che segue. Per questo le **parentesi graffe** possono essere omesse nel caso che il blocco da eseguire sia composto da una sola istruzione.

I seguenti due snippet sono equivalenti:

```
if(condizione)
{
	System.out.println("Condizione verificata");
}
```
```
if(condizione)
	System.out.println("Condizione verificata");
```

All’interno di un blocco si possono nidificare altri costrutti condizionali, in effetti anche `if` è un’istuzione. Quindi possiamo avere casi come questo:

```
if(condizioneUno) {
	if(condizioneDue)
		System.out.println("Entrambe verificate");
	else
		System.out.println("Verificata solo condizioneUno");
}
```

In questo caso e in generale quando ci sono più condizioni annidate, può essere una buona prassi decidere di utilizzare sempre le parentesi graffe, per non creare problemi di leggibilità e interpretazione del codice.

switch-case
-----------

Pur essendo possibile usare if-else per costruire strutture condizionli arbitarie, Java come molti altri linguaggi (il C ad esempio) ammette anche una secondo costrutto condizionale: **switch-case**.

Vediamo subito un esempio che poi commenteremo esplorando le caratteristiche peculiari di questo costrutto in Java:

```
switch(c) {
	case value1:
		...
		break;
	case value2:
		...
		break;
	// eventuali altri case
	case valueN:
		...
	default:
}
```

### Tipi e valori

Prima di descrivere il funzionamento dello `switch-case`, soffermiamoci a sottolineare che **il parametro dello switch** (che abbiamo qui indicato con `c`) deve essere una espressione (o variabile) di tipo `byte`, `short`, `char`, `int` (o dei rispettivi tipi boxed) oppure `Enum` (Parleremo più avanti dei tipi `Enum`). Dalla versione 7 di Java è contemplato anche il tipo `String`Types).

Questo significa che i **valori nelle clausole case** (`value1`, `value2`, …, `valueN`) devono essere del medesimo tipo del parametro (‘`c`‘)

### Il funzionamento dello switch-case

La **semantica del costrutto switch-case** è leggermente più complicata di quella di _if-then-else_ e per comprenderla rapidamente è probabilmente comodo pensare di leggere l’esempio precedente come segue:

_“dato un valore per `c`, procedi ad eseguire le istruzioni a partire dal `case` contrassegnato dallo stesso valore”_.

La parte su cui fare attenzione della frase precedente è “a partire da” in quanto nello `switch-case` l’esecuzione letteralmente salta alla riga di codice etichettata con il valore corrispondente al valore dell’argomento di switch e _da quella posizione continua eseguendo tutte le istruzioni_ senza tener conto di eventuali `case`.

Ad esempio:

```
int c = ...;
switch (c) {
	case 1:
		System.out.print("1 ");
	case 2:
		System.out.print("2 ");
		break;
	case 3:
		System.out.println("3 ");
	case 4:
		System.out.println("4 ");
	default:
		System.out.println("4+");
}
```

Facciamo una tabella in cui mettiamo i diversi risultati stampati sulla console al cambiare del valore di `c`:

Valore di ‘c’

Stampa sulla console

`3`

`3 4 4+`

`1`

`1 2`

Nel secondo caso il comando `break` ne terminerà l’esecuzione (e quindi se `c == 2` sarà stampato solamente “2”).

### break

Il comando **break** tecnicamente parlando non fa parte del costrutto `switch-case` ma è spesso utilizzato in connessione ad esso e come abbiamo visto **è opzionale**.

Vale la pena di osservare che **l’istruzione break** serve genericamente a terminare l’esecuzione di un blocco di programma (più precisamente ogni blocco associato a un ciclo iterativo o uno switch) ed anche se il costrutto `switch-case` è di gran lunga il contesto in cui lo si vede utilizzato, è sintatticamente corretto utilizzarla anche altrove.

### default

La clausola **default è opzionale** e serve a determinare una porzione di codice che sarà comunque eseguita quando non viene verificata nessuna clausola `case`

Link utili

*   [Enum](http://docs.oracle.com/javase/tutorial/java/javaOO/enum.html "Enum Types"), qualche esempio.
*   [Break](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html "Branching Statements"), una demo