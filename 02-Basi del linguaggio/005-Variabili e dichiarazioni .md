In questa sezione introdurremo le basi della sintassi del Java insieme a concetti utili per la comprensione di alcuni aspetti rilevanti dell’esecuzione di un programma.

Molte delle parti sono probabilmente già nel background di chiunque abbia mai utilizzato un qualsiasi linguaggio di programmazione ma nell’introduzione della sintassi qualche breve nota di carattere generale si ritiene utile.

Variabili, identificatori e indirizzi
-------------------------------------

Formalmente potremmo definire una **variabile** in un linguaggio di programmazione come la coppia composta da:

*   un _nome simbolico_ (detto anche **identificatore**)
*   e un _indirizzo di memoria_ destinato a contenere una determinata quantità, detta comunemente **valore** della variabile.

Il nome simbolico della variabile serve nei programmi per far riferimento all’indirizzo di memoria al fine di accedere e/o modificarne il valore durante l’esecuzione.

Dichiarare le variabili
-----------------------

In Java (che appartiene alla classe dei _linguaggi tipati_) ogni variabile ha associato anche un **tipo** (come `Integer`, `String`, `boolean`, etc.) che definisce le caratteristiche che avranno i valori che la variabile potrà assumere.

Ad esempio una variabile di tipo _Integer_ potrà contenere il valore `42` ma non il testo `"quarantadue"`.

In generale in Java **il tipo di una variabile** può essere uno dei tipi predefinitti (primitivi) del linguaggio oppure un tipo definito da noi come vedremo nelle prossime sezioni.

La sintassi per la **dichiarazione di una variabile** in Java è la seguente:

\[public|protected|private\] \[static\] \[final\] Tipo identificatore \[= value\];

dove le parti tra parentesi quadre ‘`[]`‘ sono opzionali ed il simbolo _pipe_ ‘`|`‘ deve essere letto “oppure” (il significato delle keywords verrà chiarito nel seguito).

Possiamo anche inizializzare la variabile quando è presente il simbolo ‘`=`‘ al quale segue il valore che dovrà assumere.

### I nomi delle variabili

Sintatticamente l’identificatore (nome) di una variabile è una sequenza di lettere e cifre il cui primo elemento deve essere una lettera oppure il carattere _underscore_ (‘`_`‘) o ancora il carattere _dollaro_ (‘`$`‘).

Vige comunque la **convenzione** (non regola sintattica) che i nomi delle variabili inizino con una _lettera minuscola_ e, qualora formati da più parole concatenate, tutte le parole successive alla prima siano capitalizzate e non vengano usati i simboli `_` e `$` nonostante siano ammessi. Ad esempio:

int nomeDellaVariabileIntera;

Per i nomi dei tipi, ovvero per le classi, la convenzione prevede invece che la prima lettera sia maiuscola):

class LaMiaClasse

L’identificatore può essere una qualsiasi stringa ma esistono alcune parole riservare del linguaggio che non possono essere utilizzate come identificatori:

`abstract`

`continue`

`for`

`new`

`switch`

`assert`

`default`

`goto`

`package`

`synchronized`

`boolean`

`do`

`if`

`private`

`this`

`break`

`double`

`implements`

`protected`

`throw`

`byte`

`else`

`import`

`public`

`throws`

`case`

`enum`

`instanceof`

`return`

`transient`

`catch`

`extends`

`int`

`short`

`try`

`char`

`final`

`interface`

`static`

`void`

`class`

`finally`

`long`

`strictfp`

`volatile`

`const`

`float`

`native`

`super`

`while`

Tipi di variabili
-----------------

In Java si distinguono tre tipi di variabili: variabili locali, variabili di istanza e variabili di classe. Vediamo in dettaglio di che si tratta.

### Variabili Locali

Si parla di **variabili locali** quando la dichiarazione avviene all’interno di un metodo.

Le variabili locali sono create quando un metodo viene chiamato e scompaiono (vengono cancellate dalla memoria) quando il metodo termina.

Ogni variabile dichiarata all’interno del metodo può essere utilizzata solamente all’interno del metodo stesso ed in Java le variabili locali non possono essere utilizzate prima della loro inizializzazione.

Per esempio, se provassimo a compilare il seguente metodo:

void add(long i) {
	
	long j; 
	j = j + i; 
} 

il compilatore Java ci darebbe un messaggio di errore perchè la variabile `j` non è stata inizializzata prima del suo uso e non è quindi possibile aggiungere un valore alla variabile fino a che a questa non è stato assegnato un valore.

Il codice seguente sarebbe invece compilato senza alcun errore:

void add(long i) {

	long j = 1;
	j = j + i;
} 

Nell’esecuzione di questo frammento di codice la macchina virtuale Java crea in memoria lo spazio per registrare le variabili locali `i` e `j`, per poi cancellarle (liberando lo spazio in memoria) alla fine dell’esecuzione del metodo `add`.

### Scope di una variabile

Più in generale si definisce **scope** di una variabile l’area del codice nel quale un identificatore resta associato ad un indirizzo di memoria (e quindi l’area di codice entro il quale una variabile mantiene il suo valore).

In Java ogni blocco (cioè ogni gruppo di linee di codice racchiuso da parentesi graffe `{}`) definisce uno scope e ogni variabile locale ha come scope l’area di codice che inizia dalla definizione della variabile stessa e termina con il blocco corrente.

### Variabili di istanza

Le variabili di istanza, anche note come _field_ o **campi**, sono dichiarate all’interno di una classe ma all’esterno di ogni metodo.

I field hanno come **scope** l’intero corpo della classe in cui sono dichiarati, compresi i metodi della classe stessa. Quindi sono visibili all’interno di tutti i metodi della classe.

Può succedere che una variabile locale in un metodo (oppure il parametro di un metodo) abbia lo stesso nome (identificatore) di una variabile di istanza. In questo caso ha la precedenza la variabile più specifica, cioè la variabile locale o il parametro.

Vediamo un esempio:

public class Scope {
	
	int var = 6;
	
	public void primoMetodo(int var) {
		
		int i = var;  
		// in questo caso ha precedenza il parametro e
		// i assume il valore che sarà passato come parametro al metodo
		
		// ...
	}
  
	public void secondoMetodo() {
		
		int var = 7;
		int i = var;   
		// qui ha precedenza la variabile locale al metodo, quindi
		// i ha il valore 7
		
		// ... 
	}
  
	public void terzoMetodo() {
	
		int i = var;    
		// qui semplicemente assegnamo ad i il valore della 
		// variabile di istanza e i prende il valore 6
		
		// ...
	}

	public void quartoMetodo(int var) {
		
		int i = this.var; 
		// in questo caso i assume il valore 6 indipendentemente 
		// dal valore del parametro poiché abbiamo utilizzato la 
		// keyword 'this', indica di utilizzare la variabile 'var' 
		// che abbiamo definito come field e che appartiene 
		// all'istanza corrente della classe.
		
		// ...
	}

}

Un’istanza di una variabile (non statica) continua ad esistere nella memoria di un programma fino a quando esiste l’oggetto che la contiene (ed un oggetto “rimane in vita” fino a quando ne esiste almeno una referenza, quindi una variabile associata ad esso).

### Variabili di classe (static)

Le variabili di classe infine, comunemente dette anche _static field_ o **campi statici**, sono variabili di istanza ma nella loro definizione viene usata la keyword ‘static’.

static int var = 6;

Una variabile di classe è una variabile **visibile da tutte le istanze** di quell’oggetto ed il suo valore non cambia da istanza ad istanza, per questo appartiene trasversalmente a tutta la classe.

Più in dettaglio mentre per le variabili di istanza viene allocata una nuova locazione di memoria per ogni istanza di una classe, per le variabili statiche esiste una unica locazione di memoria legata alla classe e non associata ad ogni singola istanza.

Una variabile di classe, statica, vive (cioè mantiene occupata la memoria e continua a mantenere il suo valore) fino al termine del programma.

Modificatori di visibilità: public, private, protected, default
---------------------------------------------------------------

Le variabili di istanza e di classe possono essere ulteriormente qualificate per mezzo delle keywords **public e private** che ne determinano la visibilità all’esterno della classe in cui sono dichiarate.

*   Se utilizziamo la keyword **private** una variabile sarà visibile (accessibile, utilizzabile per far riferimento al suo indirizzo di memoria e quindi al suo valore) solamente all’interno della classe;
*   mentre se qualificheremo la variabile come **public** indicheremo al compilatore che la variabile potrà essere utilizzata da qualsiasi parte del codice in cui ci sia una istanza della classe (con la notazione `idIstanzaClasse.nomeVariabile`).
*   **Protected** significa che la variabile sarà accessibile da ogni altra classe che appartiene al medesimo package della classe che contiene la variabile e da ogni classe che ne deriva (la estende).
*   Se non specifichiamo un qualificatore di visibilità, la variabile sarà lasciata con la visibilità di **default** che in Java signifca che sarà accessibile solo da tutte le classi nel medesimo package.

Variabili final e static final
------------------------------

Infine si usa la keyword **final** per dichiarare una variabile che potrà essere inizializzata una sola volta, sia nella fase di dichiarazione o attraverso una successiva assegnazione.

Al contrario delle costanti, il valore delle variabili `final` non è necesariamente noto a _compile-time_ ma il loro indirizzo di memoria può essere inizializzato una sola volta rendendone possibile l’utilizzo in alcuni contesti in cui sarebbe impossibile utilizzare le normali variabili locali.

In Java si definiscono **costanti** le variabili che vengono qualificate contempraneamente come **final e static**: è convenzione che i nomi delle variabili `final` siano in maiuscolo e se il nome è costituito da più parole, queste vengano separate dal carattere underscore (‘`_`‘);

Link utili

*   [Java Language Keywords](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html) dal sito Oracle
*   [List of Java keywords](http://en.wikipedia.org/wiki/List_of_Java_keywords) su WikiPedia