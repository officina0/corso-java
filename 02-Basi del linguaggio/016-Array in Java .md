Un **array in Java** è un contenitore che permette di gestire una sequenza di lunghezza fissa di elementi tutti del medesimo tipo. Il numero di elementi in un array, detto _lunghezza dell’array_, deve essere dichiarato al momento della sua _allocazione_ e non può essere cambiato.

Dichiarare un array in Java
---------------------------

La sintassi per la dichiarazione di una variabile di tipo array è la seguente:

Tipo\[\] nome;

nella quale si deve osservare l’uso delle parentesi quadre ‘`[]`‘ tra il tipo ed il nome della variabile (anche se la documentazione riporta sempre questa forma della dichiarazione è ammissibile anche postporre le parentesi quadre dopo l’identificatore:

Tipo nome\[\];

`Tipo` può essere sia un tipo primitivo di java, sia il nome di una classe.

### Allocare l’array

Per default le variabili di tipo array sono inizializzate con il valore `null`. Quindi, prima di poterle usare, dovremo inizializzarle allocando per esse la memoria per mezzo della consueta keyword ‘new’:

nome = new Tipo\[n\];

che riserva (o ‘allocà) la memoria necessaria per contenere `n` elementi di tipo `Tipo`.

Tutti gli elementi dell’array sono inizializzati con il valore di default previsto dal tipo che abbiamo indicato. Ad esempio, se `Tipo` è una classe tutti gli elementi verranno inizializzati a `null` e quindi andranno a loro volta allocati.

Usare gli array
---------------

Una volta creato l’array, possiamo **accedere ai singoli elementi** indicandone la posizione (detta _indice_) all’interno contenitore, grazie all’operatore ‘`[]`‘ (parentesi quadre). Ad esempio:

nome\[3\]; // indica il quarto elemento della collezione

Bisogna ricordare che gli elementi sono numerati _a partire da zero_, quindi `nome[0]` farà riferimento al primo elemento, `nome[1]` al secondo e così via fino all’ultimo indicato da `nome[lunghezza-1]`.

Come detto sopra la lunghezza di un array è fissata in fase di creazione e si può in qualsiasi momento accedere al suo valore utilizzando la proprietà **length** che è presente in ogni array java con la sintassi: `nome.length`.

Gli array sono spesso utilizzati per mantenere informazioni statiche all’interno di un programma; ad esempio per determinare quanti giorni abbia un mese dato il suo numero potremmo pensare di scrivere un metodo del tipo:

int giorniPerMese(int numeroMese) {

    return numeroGiorniPerMese\[numeroMese -1\]; 
}

che per funzionare ha bisogno di un array che contenga, mese per mese, il numero di giorni. Quindi:

// dichiariamo un array che utilizzeremo nel metodo
int\[\] numeroGiorniPerMese = new int\[12\];  // dichiarazione e allocazione dell'array;

numeroGiorniPerMese\[0\] = 31; // gennaio, primo mese, posizione 0
numeroGiorniPerMese\[1\] = 28; // febbraio (non funziona per i bisestili ma non ci interessa)

// etc ...
numeroGiorniPerMese\[11\] = 31; // dicembre

Inizializzare array con liste (literals)
----------------------------------------

L’inizializzazione degli elementi dell’array è prolissa, poco leggibile e decisamente scomoda da collocare nel codice (soprattutto se vogliamo dichiarare `numeroGiorniPerMese` come `static`, provate per fare pratica !!). Ma Java ammette una sintassi per allocare ed inizializzare gli array in modo più diretto:

int \[\] numeroGiorniPerMese = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

Questa modalità prevede che i valori degli elementi dell’array possano essere elencati in una lista racchiusa tra parentesi graffe e separati da virgole. Naturalmente tutti i valori della lista devono essere del tipo specificato per l’array (o ad esso assegnabile, ma questa affermazione risulterà probabilmente più chiara quando avremo preso confidenza con con il concetto di derivazione di una classe da un’altra).

Errori con gli array
--------------------

Quando utilizziamo gli array è nostra responsabilità non tentare di accedere ad elementi esterni al range definito. Ad esempio:

int l = 5;
int \[\] a = new int\[l\];

a\[9\] = 10;

Quando mandiamo in esecuzione di questo pezzo di codice, esso genera un **errore di runtime** (non di compilazione): la JVM, quando chiudiamo di accedere al decimo elemento dell’array, emette una eccezione di tipo [ArrayIndexOutOfBoundsException](http://docs.oracle.com/javase/6/docs/api/java/lang/ArrayIndexOutOfBoundsException.html%20).

Array multidimensionali
-----------------------

Java permette anche l’utilizzo di array di array (detti anche **array multimensionali**) di profondità arbitraria (o con numero di dimensioni arbitrario) e la sintassi per la loro dichiarazione ed allocazione si ottiene semplicemente ripetendo le parentesi quadre tante volte quante il numero di dimensioni, per esempio:

int \[\]\[\]\[\] arrayConDimensione3 = new int\[4\]\[5\]\[6\];

Analogamente a quanto detto per gli array unidimensionali, Java prevede una sintassi speciale per l’inizializzazione degli array multidimensionali:

float\[\]\[\] mat = new int\[\]\[\] = { 
			{ 1, 2, 3 }, 
			{ 4, 5, 6 },
			{ 7, 8, 9, 10, 11 }
		  };

L’ultima riga mostra anche come sia possible creare array multidimensionali ‘_ragged_‘, cioè nei quali i sotto-array non abbiano tutti la medesima lunghezza.

È possibile creare array ragged anche senza inizializzarli immediatamente, ad esempio con la sintassi che segue:

int \[\]\[\] ar = new int\[10\]\[\];

for(int i =0; i< 10; i++) {

	ar\[i\] = new int\[(i%3) +1\];

}

che risulta chiara se si pone attenzione al fatto che dato un array n-dimensionale possiamo sempre ottenerne una sezione k dimensionale (con `k < n`) semplicemente specificando le parentesi quadre solo per le `n-k` dimensioni che vogliamo fissare ed omettendole per quelle che si vogliono lasciare libere:

int \[\]\[\]\[\] cubo = new int\[10\]\[10\]\[10\];

int\[\]\[\] quadrato1 = cubo\[0\];
int\[\]\[\] quadrato2 = cubo\[1\];
int\[\] segmento = quadrato1\[0\];

int i = segmento\[4\];

int j = quadrato2\[3\]\[5\];

int k = cubo\[3\]\[5\]\[7\];

java.util.Arrays
----------------

Gli array sono un costrutto classico di praticamente ogni linguaggio di programmazione e, nonostante il limite notevole di non poter cambiare dimensione (size) dopo la creazione (e qualche complicanza sintattica degli array multidimensionali), il loro utilizzo è estremamente comune in molti ambiti. Perciò Java mette a disposizione la classe **java.util.Arrays** con numerosi algoritmi per operare sugli array:

*   ricerca;
*   sorting (ordinamento);
*   copia;

e altri strumenti per la manipolazione, tutti sotto forma di metodi statici.