In questa lezione esaminiamo l’**Autoboxing in Java**: si tratta della caratteristica del linguaggio che, a partire dalla versione 1.5, ci consente di lavorare con tipi primitivi e tipi wrapper in maniera intercambiabile.

Boxing
------

In Java generalmente ci occupiamo di utilizzare e definire oggetti come istanze di una classe, quindi con metodi e attributi. Ma per motivi pratici abbiamo spesso a che fare con tipi primitivi (`int`, `double`, `boolean`, …) che non sono oggetti, ma “tipi semplici”.

Prima dell’introduzione dell’autoboxing programmando in Java ci trovavamo nella necessità di convertire un tipo primitivo nella sua corrispondente classe wrapper. Lo spezzone di codice che segue potrebbe non risultarvi nuovo:

Integer x = new Integer (10);
Double y = new Double (5.5);

Boolean z = Boolean.parseBoolean("true");

Queste operazioni sono note come operazioni di boxing, cioè “inscatolamento” del tipo primitivo nel relativo tipo wrapper al fine di utilizzare un oggetto e tutte le sue proprietà (ad esempio porre un intero in una lista o operazioni che hanno necessità di maneggiare oggetti).

L’autoboxing
------------

Veniamo alla novità introdotta da Java 1.5 con un esempio pratico:

Integer x = 10;
Double y = 5.5f;

Boolean z = true;

Number n = 0.0f;

Attraverso autoboxing gli oggetti scritti nello spezzone di codice vengono automaticamente creati con i valori di riferimento dettati, senza generare errori. Questo permette di scrivere codice più leggibile e maneggevole.

Chiaramente alla funzione di boxing è associata l’operazione di unboxing che trae gli stessi vantaggi della precedente:

/\*\* Esempio di operazione di unboxing */

int x = -1;
Integer y = x;

Il linguaggio si arricchisce, permettendo allo sviluppatore di non preoccuparsi delle operazioni di conversione (boxing e uboxing, appunto) lasciandole al compilatore del bytecode che si occuperà di gestirle per noi (autoboxing).

A basso livello la situazione non è affatto cambiata, la macchina virtuale che esegue il bytecode non ha avuto cambiamenti; ciò che cambia è a livello di compilazione. Infatti le operazioni di conversione a livello di bytecode sono quelle che avremmo svolto manualmente, solo che ci viene in aiuto il compilatore preoccupandosi di effettuarle lui in automatico per noi.

Il comportamento dell’operatore di uguaglianza
----------------------------------------------

Ovviamente ciò significa che le regole osservate in ambiente pre Java 1.5 continuano a valere. Dal punto di vista della sintassi comunque la cosa produce notevoli vantaggi in quanto ora è possibile associare gli operatori aritmetici e le strutture condizionali ai tipi wrapper (ricordandoci sempre che verrà fatto unboxing automatico):

/\*\* Esempio di utilizzo dei costrutti come operatori aritmetici 
 \*  e statement condizionali
 */
 
Integer x = 0;
Boolean verify = true;
while (verify)
{
  x++;
  if (x > 10)
    verify = false;
}

Il codice scritto mostra come sia possibile utilizzare i costrutti sia con operatori aritmetici sia in statement condizionali (if, while, for, …). Unico caso a cui fare attenzione è il caso dell’uguaglianza tra due istanze di wrapper che puntano al medesimo valore:

/\*\* Esempio di uguaglianza tra istanze di wrapper */

Integer x = 1000;
Integer y = 1000;

x==y ??

La condizione precedente dovrebbe risultare falsa, anche se i valori sono uguali. In pratica di fronte all’operatore di uguaglianza, il compilatore si comporta normalmente, facendo comparazione tra due istanze di oggetto senza effettuare unboxing. L’operazione, tradotta sarebbe:

Integer x = new Integer (1000);
Integer y = new Integer (1000);

x == y; // false!

Qui forse è più intuitivo capire perché il risultato è falso. Tutto ciò risulta verificato a meno di alcune situazioni dove la macchina virtuale utilizza delle ottimizzazioni. Nei seguenti casi la JVM crea delle istanze immutabili che vengono riutilizzate (per motivi di performance):

*   Valori boolean true e false;
*   Valori del tipo byte;
*   Primo byte del tipo int (valori tra -127 e 127);
*   Primo byte del tipo char;

Ad esempio, comparare due Integer che hanno come valore 100 ci darebbe `true` (fate qualche tentativo).

Impatto sull’overload dei metodi
--------------------------------

Altro comportamento a cui dobbiamo porre attenzione è nel caso dell’**overload di metodi**, dove c’è la possibilità di confusione in fase di compilazione:

/\*\* Esempio di overload di metodi */

public void metodoA(Integer x);
public void metodoA(double y);

public void usaA()
{
	int d = 0;
	metodoA(d);
}

In questo caso il compilatore non effettua alcuna operazione di unboxing, proprio perché non c’è modo di sapere dinamicamente come comportarsi. In generale, di fronte a situazioni di ambiguità, per mantenere anche la compatibilità con le precedenti versioni, il comportamento è quello che ci sarebbe con la versione di Java 1.4. In questo specifico caso la chiamata sarebbe fatta al metodo che accetta come tipo il `double`.