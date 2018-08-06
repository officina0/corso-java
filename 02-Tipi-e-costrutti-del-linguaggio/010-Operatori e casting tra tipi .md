Gli **operatori in Java** sono simili a quelli che si trovano in altri linguaggi di programmazione: il core di Java prevede per i tipi primitivi operatori algebrici, logici, etc. In questa lezione esaminiamo questi operatori di base, ma oltre a questi possiamo definire, per ogni tipo, metodi che ne implementino di nuovi.

Iniziamo con l’**operatore “punto”** (`.`) che serve per l’accesso a campi e metodi di classi e oggetti. Per accedere al campo `simpleField` dell’oggetto `myObject` scriviamo semplicemente:

myObject.simpleField

Se `myObject` espone anche un metodo `myMethod(int value)`, possiamo involarlo scrivendo:

myObject.myMethod(100)

Casting
-------

Prima di proseguire con gli operatori bisogna ricordare che Java è un linguaggio “fortemente tipato”, perciò i tipi sono fondamentali e c’è uno stretto controllo sull’utilizzo dei tipi: ad esempio non è possibile assegnare un carattere ad un intero, cosa possibile nel linguaggio C, ma è anche impossibile assegnare un `double` ad un `float`, senza un esplicito **casting**.

Il **casting** consiste nel forzare un valore di un tipo ad assumere un tipo diverso: se scriviamo `5` ad esempio abbiamo un numero intero, ma se ne facciamo un cast a `float` indenderemo la versione di 5 “reale”, per farlo basta scrivere:

(float) 5;

In questo modo possiamo ottenere i famosi assegnamenti di interi a reali e di reali double a reali float, prendiamo ad esempio queste variabili:

double v1 = 10.0;
float v2;
int v3 = 5;

Gli assegnamenti che seguono sono errati:

v2 = v1; // non si può assegnare un double a un float
v1 = v3; // non si può assegnare un intero a un double

Per rendere possibili questi assegnamenti, quindi “leciti” per Java, ci dobbiamo servire dei cast:

v2 = (float) v1;
v1 = (double) v3;

Operatori artitmetici
---------------------

Come dicevamo abbiamo a disposizione i classici **operatori aritmetici** (`+`, `-`, `*`, `/` e `%`), ma è importante sottolineare che anche **le operazioni sono tipate**, quindi se sommiamo due interi otteniamo un valore intero, così per tutte le operazioni, in questo modo possiamo sempre prevedere il tipo del risultato di una certa espressione.

A questo proposito è importante ricordare che la **divisione tra interi**, a differenza di altri linguaggi, ritorna un valore intero. Quindi se vogliamo ottenere il numero reale derivato dalla divisione dei due numeri interi `x` e `y`, bisogna prima trasformarli in reali:

float risultato= (float) x  / (float) y;

Operatori abbreviati
--------------------

Abbiamo ad un certo punto usato un assegnamento particolare, ovvero `+=`, questa è una abbreviazione, serve ad incrementare il valore di una variabile senza doverla riscrivere. Se ad esempio vogliamo aggiungere alla variabile `a` il valore `5`, sommandolo al suo valore attuale scriveremmo:

a = a + 5;

oppure, in modo abbreviato:

a += 5;

Vale lo stesso per ogni operatore binario (es. `-`, `*`, `/`).

Java inoltre offre altri quattro operatori che sono delle abbreviazioni, due di incremento di variabili e due di decremento. Sia X una variabile, possiamo scrivere:

X++; // valuta X, poi incrementa X di 1 
X--; // valuta X, poi decrementa X di 1

++X; // incrementa X di 1, poi valuta X
--X; // decrementa X di 1, poi valuta X

L’espressione `X++` è un comando di assegnamento, ma anche una espressione che restituisce un risultato. Il comportamento cambia a seconda che i simboli di incremento o decremento precedano o seguano la variabile:

*   se l’operatore segue la variabile, l’espressione restituisce il valore attuale della variabile prima di modificarlo;
*   se l’operatore precede la variabile, l’espressione restituisce il valore della variabile già modificato.

Ad esempio:

X = 10;
Y = X++;
// risultato: X=11 e Y=10

Ecco un esempio più complesso da poter provare:

class incdec { 
	
	public static void main(String \[\] a)
	{
		int X,Y,Z,W,V;
	
		X=10;
		System.out.println("X="+X);
	
		Y=X++;
		System.out.println("Y=X++: ho X="+X+",Y="+Y);
	
		Z=++Y;
		System.out.println("Z=++Y: ho Z="+Z+",Y="+Y);
	
		W=Z--;
		System.out.println("W=Z--: ho W="+W+",Z="+Z);
	
		V=--W;
		System.out.println("V=--W: ho V="+V+",W="+W);
	}
}

Operatori su stringhe
---------------------

Per le stringhe esiste un operatore di concatenamento:

String a = "Pietro " ;
String b = a + "Castellu";
String b += "cci"

Il risultato sarà `"Pietro Castellucci"`. L’operatore `+` appende ad una stringa anche caratteri e numeri, ad esempio possiamo costruire una stringa scrivendo:

System.out.println("Numero complesso:"+ parteReale + " +i" + parteImmaginaria + " ha modulo " + modulo);

Sembra strano questo, anche in considerazione del fatto che abbiamo detto che le espressioni Java sono ben tipate a differenza del C, questo avviene però perché il sistema effettua delle conversioni implicite di tipi primitivi e oggetti in stringhe. Ad esempio definendo un qualsiasi oggetto, se ne definisco un metodo **toString()**, il sistema che si troverà a lavorare con stringhe e con quest’oggetto ne invocherà automaticamente il metodo `toString()`.

Operatori di confronto
----------------------

Altri operatori sono quelli di confronto, ovvero:

Operatore

Descrizione

espressione

Cosa restituisce

**>**

maggiore di

`x>y`

`true` se `x` è strettamente maggiore di `y`, `false` altrimenti

**>=**

maggiore o uguale di

`x>=y`

`true` se `x` è maggiore o uguale di `y`, `false` altrimenti

**<**

minore di

`x<y`

`true` se `x` è strettamente minore di `y`, `false` altrimenti

**<=**

minore o uguale di

`x<=y`

`true` se `x` è minore o uguale di `y`, `false` altrimenti

**==**

uguale

`x==y`

`true` se `x` è uguale a `y`, `false` altrimenti

**!=**

diverso

`x!=y`

`true` se `x` è diverso da `y`, `false` altrimenti

Operatori logici
----------------

Vi sono gli operatori logici che ci aiutano a definire **espressioni booleane** , che in altre parole mettono in relazione il verificarsi di più condizioni. Vediamo, attraverso questa tabella come possiamo mettere in relazione due valori (o espressioni) `x` e `y` di tipo booleano:

Operatore

Descrizione

espressione

Cosa restituisce

**&&**

and

`x&&y`

`true` se `x` e `y` sono entrambe vere, `false` altrimenti (almeno una delle due false)

**||**

or

`x||y`

`true` se almeno una tra `x` e `y` è vera (o entrambe), `false` altrimenti (entrambe false)

**!**

not

`!x`

`true` se `x` è falsa e `false` se `x` è vera.

Operatori Bitwise
-----------------

Inoltre vi sono gli **operatori binari orientati ai bit**, che effettuano le operazioni logiche confrontando, i bit degli operandi:

Operatore

Descrizione

espressione

Cosa restituisce

**&**

and ‘bit a bit’

`x&y`

il valore determinato dall’operazione di `and` tra i bit di `x` e `y`

**|**

or ‘bit a bit’

`x|y`

il valore determinato dall’operazione di `or` tra i bit di `x` e `y`

**^**

xor (o or esclusivo) ‘bit a bit’

`x^y`

il valore determinato dall’operazione di `xor` tra i bit di `x` e `y` (lo xor ritorna `true` (o il bit 1) solo se uno degli operandi è vero e l’altro è falso)

**>>**  
**<<**

shift destro (o sinistro) con segno

`x>>y`

sposta i bit della variabile `x` verso destra (o sinistra) di `y` posizioni, preservando il bit di segno. Lo slittamento a sinistra prevede l’inserimento di bit 0 in coda, e non presenta particolari problemi. Con lo slittamento a destra le cose cambiano perché coinvolge l’ultimo bit che è speciale, in quanto rappresenta il segno

**>>>**

shift destro senza segno

`x>>>y`

sposta i bit della variabile `x` verso destra di `y` posizioni, senza considerare il bit di segno (vengono aggiunti sempre 0 a sinistra)

**~**

complemento

`~x`

inverte tutti i bit della variabile `x` (gli 0 diventano 1 e viceversa)

Queste operazioni binarie sono molto utili nella costruzione di maschere.

Operatore condizionale
----------------------

L’ultimo operatore da vedere è l’operatore condizionale, che permette di definire espressioni con due differenti risultati a seconda del valore assunto da una espressione booleana. Ecco la sintassi:

(espressione boolana) ? expVero : expFalso;

a seconda che il valore dell’espressione risulti vero o falso, viene eseguita la prima espressione dopo il punto interrogativo (`?`) o quella dopo i due punti (`:`).

Si consideri ad esempio l’espressione seguente:

int ValoreAssoluto = (a<0) ? -a : a;

`ValoreAssoluto` assumerà il valore `-a` se a è negativo oppure a se è positivo o nullo.