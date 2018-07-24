Questo package è molto utile, esso mette a disposizione 34 classi e 13 interfacce che implementano alcune tra le strutture dati più comuni, alcune operazioni su date e sul calendario, e altre cose.

Inoltre **il package java.util** include altri sottopackages che sono: java.util.mime, java.util.zip,java.util.jar che servono rispettivamente per trattare files di tipo MIME, di tipo ZIP e di tipo Java Archive (JAR), che vedremo in seguito in questa lezione.

Per **struttura dati**, in informatica si intende una struttura logica atta a contenere un certo numero di dati, nella quale è possibile inserire dati, toglierli, cercarli o ordinarli. Una semplice struttura dati che abbiamo gia visto è l’array, esso contiene un cero numero di dati che possono essere qualsiasi cosa, da byte ad oggetti complessi, su questa si possono fare inserzioni ricerche e cancellazioni, volendo un array può anche essere ordinato.

Per vedere se in un array grande `N` è presente un dato `X`, occorre visitare tutto l’array ed effettuare confronti con ciascun elemento dell’array: un’operazione laboriosa. Esistono strutture dati che eseguono la stessa ricerca impiegando un solo accesso alla struttura, come le già citate tabelle hash.

La scelta delle strutture dati da usare in un programma è piuttosto importante: si sceglie in base all’uso che deve esserne fatto nel programma. Ad esempio è difficile che in un database molto grande in cui si fanno continue ricerche si usi un array come struttura dati, perché ogni ricerca costerà molto in termini di tempo (il computer è veloce, ma ha anch’esso i suoi limiti, se pensiamo ad un database di un miliardo di elementi, il computer per visitarlo tutto può impiegare parecchio), in questo caso è forse meglio usare una tabella hash opportunamente grande.

Java fornisce alcune strutture dati con le relative operazioni e a noi non resta che usarle. Nella scelta incidono considerazioni di buon senso ma anche una conoscenza dei dettagli realizzativi delle strutture, e della loro complessita computazionale. Nel descrivere le classi di Java usate per implemetare queste strutture, faremo cenno anche a questi aspetti, come i tempi di inserzioni, ricerche, etc.

Il contenuto di del package java.util
-------------------------------------

Interfacce

Collection

Comparator

Enumeration

EventListener

Iterator

List

ListIterator

Map

Map.Entry

Observer

Set

SortedMap

SortedSet

Queste interfacce stabiliscono alcune proprietà delle nostre strutture dati, esse vengono implementate in alcune delle seguenti classi.

Classi

AbstractCollection

AbstractList

AbstractMap

AbstractSequentialList

AbstractSet

ArrayList

Arrays

BitSet

Calendar

Collections

Date

Dictionary

EventObject

GregorianCalendar

HashMap

HashSet

Hashtable

LinkedList

ListresourceBundle

Locale

Observable

Properties

PropertyPermission

PropertyResourceBundle

Random

ResourceBundle

SimpleTimeZone

Stack

StringTokenizer

Timer

TimerTask

TimeZone

treeMap

treeSet

Vector

WeakHashMap

Eccezioni

ConcurrentModificationException

EmptyStackException

MissingResourceException

NoSuchElementException

TooManyListenersException

La struttura BitSet
-------------------

Iniziamo a vedere qualche struttura, vediamo **BitSet**, o in italiano Insieme di Bit. Questa classe fornisce un modo per creare e gestire insiemi bit (1,0) o meglio valori true, false.

L’insieme è in effetti un vettore di bit, però che cresce dinamicamente. All’inizio i bit hanno valore false, e il vettore è lungo 2^32-1 ((due alla trentadue) meno uno).

La classe ha due costruttori: `BitSet()`, per creare un BitSet di dimensioni standard, e `BitSet(int nbits)`, per creare un BitSet che contiene nbits bit.

Le **operazioni** che si possono effettuare sono le seguenti:

Operazioni

Descrizione

`void and(BitSet set)`

restituisce l’`and` tra il BitSet e l’altro BitSet individuato da `set`

`void andNot(BitSet set)`

azzera i bit del BitSet corrispondenti agli 1 nel BitSet `set`

`void or(BitSet set)`

restituisce l’or tra BitSet e il BitSet passato come parametro (`set`)

`void xor(BitSet set)`

restituisce l’or esclusivo tra BitSet e il BitSet passato come parametro (`set`)

`void clear(int bitIndex)`

imposta il bit specificato a false

`void set(int bitIndex)`

imposta il bit specificato a true

`Object clone()`

BitSet è dichiarato Cloneable, questo metodo ne crea una copia uguale

`boolean equals(Object obj)`

confronta l’oggetto con un altro oggetto

`boolean get(int bitIndex)`

da il valore del bit numero bitIndex

`int hashCode()`

da un codice hash per questo bitset

`int length()`

da la grandezza logica del bit set, ovvero il bit più alto impostato a `true`, più uno

`int size()`

restituisce il numero di bit attuali della bitset, è il bit più alto che può essere posto a uno senza ampliare il bit set

`String toString()`

trasforma il bitset in una stringa

Proviamo il bit set con il seguente programma `CaratteriUsati`, che possiamo scrivere in un file chiamato `CaratteriUsati.java`, il quale prende una stringa in ingresso e vede quali caratteri sono stati usati.

import java.util.BitSet;

public class CaratteriUsati
{
	public static void main (String\[\] a)
	{
		String tmp;
		
		try { tmp=a\[0\]; }
		catch (ArrayIndexOutOfBoundsException e)
		{ errore(); }
		
		elabora(a\[0\]);
	}
	
	public static void errore()
	{
		System.out.println("ERRORE, ho bisogno di una stringa:");
		System.out.println("\\tBattere:");
		System.out.println("\\t\\tjava CaratteriUsati StrINGA.");
		System.exit(0);
	}

	public static void elabora (String a)
	{
		String tmp = a;
		BitSet usati= new BitSet();
		
		for (int i = 0; i < tmp.length() ; i++)
			usati.set(tmp.charAt(i));
		
		String out="\[";
		int dim=usati.size();
		
		for (int i = 0; i < dim; i++)
		{
			if (usati.get(i))
			out+=(char )i;
		};
		
		out+="\]";
		System.out.println(out);
		
		System.out.println("Per l'elaborazione ho usato una bit set di "+usati.size()+" bit");
		System.out.println ("\\t\\t\\tPietro ");
	}
}

La classe Vector
----------------

Vediamo ora un’altra classe di `java.util`, la classe **Vector**. Questa classe implementa degli array di `Object`. La cosa interessante rispetto agli array del linguaggio, è che si può modificare la lunghezza del vettore a tempo di esecuzione.

In `Vector` avremo quindi oltre ai costruttori tre tipi di metodi, metodi per modificare il vettore, metodi per ottenere i valori presenti nel vettore e metodi per gestire la crescita del vettore.

Vector fornisce quattro costruttori:

Costruttore

Descrizione

`Vector()`

costruisce un vettore grande 10, e con possibilità di incremento pari a zero

`Vector(Collection c)`

costruisce un vettore contenente gli elementi della collezione specificata, nello stesso ordine in cui appaiono nella collezione

`Vector(int initialCapacity)`

costruisce un vettore grande quando initialCapacity, con possibilità di Incremento pari a zero

`Vector(int initialCapacity, int capacityIncrement)`

costruisce un vettore grande initialCapacity, con la possibilità di incremento pari a capacityIncrement

Alcuni metodi della classe sono i seguenti:

Metodo

Descrizione

`void add(int index, Object element)`

aggiunge un elemanto al vettore nella posizione indicata.

`boolean add(Object o)`

aggiunge un elementio alla fine del vettore.

`boolean addAll(Collection c)`

aggiunge alla fine del vettore gli elementi della collezione specificata, nello stesso ordine in cui appaiono nella collezione.

`boolean addAll(int index, Collection c)`

aggiunge gli elementi della collezione nel vettore iniziando dall’indice specificato. Questi tre metodi ritornano true se gli elementi sono stati aggiunti, false se non c’entrano

`void addElement(Object obj)`

aggiunge l’elemento al vettore, e ne incrementa la capacità di uno

`int capacity()`

da’ la capacità attuale del vettore.

`void clear()`

rimuove tutti gli elementi presenti nel vettore.

`Object clone()`

solito metodo usato per clonare l’oggetto Vector, che è chiaramente cloneable.

`boolean contains(Object elem)`

controlla se l’elemento specificato è presente nel vettore.

`boolean containsAll(Collection c)`

controlla se l’intera collezione specificata è presente nel vettore.

`void copyInto(Object[] anArray)`

copia il contenuto dell’oggeto di tipo Vector in un array di oggetti del linguaggio.

`Object elementAt(int index)`

restituisce l’elemento del vettore che si trova nella posizione specificata.

`boolean equals(Object o)`

controlla l’uguaglianza tra il vettore e l’oggetto specificato.

`Object firstElement()`

da il primo elemento del vettore, quello in posizione 0.

`Object get(int index)`

da l’elemento del vettore che si trova nella posizione specificata, e lo cancella dal vettore.

`int hashCode()`

da il codice hash del vettore.

`int indexOf(Object elem)`

cerca la prima occorrenza del dato elemento nel vettore, e ne restituisce l’indice. Per prima occorrenza intendo ovviamente quella di indice più basso.

`int indexOf(Object elem, int index)`

cerca la prima occorrenza del dato elemento nel vettore iniziando dall’indice specificato, e ne restituisce l’indice

`void insertElementAt(Object obj, int index)`

inserisce l’oggetto specificato nella posizione voluta del vettore.

`boolean isEmpty()`

controlla se il vettore è vuoto.

`Object lastElement()`

da l’ultimo componente del vettore.

`int lastIndexOf(Object elem)`

da l’indice dell’ultima occorrenza dell’oggetto specificato nel vettore.

`int lastIndexOf(Object elem, int index)`

come prima iniziando dall’indice specificato e procedendo all’indietro.

`Object remove(int index)`

cancella l’elemento che si trova nella posizione indicata nel vettore.

`boolean remove(Object o)`

cancella la prima occorrenza nel vettore dell’oggetto specificato.

`boolean removeAll(Collection c)`

cancella dal vettore tutti gli elementi contenuti nella collezione specificata.

`void removeAllElements()`

cancella tutti gli elementi del vettore.

`boolean removeElement(Object obj)`

cancella la prima occorrenza dell’oggetto specificato nel vettore.

`void removeElementAt(int index)`

cancella l’elemento nella posizione specificata.

`boolean retainAll(Collection c)`

cancella tutti gli elementi del vettore che non sono contenuti nella collezione specificata

`Object set(int index, Object element)`

rimpiazza l’elemento nella posizione specificata del vettore con l’elemento passatogli come parametro (mette element all’indirizzo index).

`void setElementAt(Object obj, int index)`

mette l’oggetto specificato nella posizione voluta.

`void setsize(int newsize)`

setta la nuova dimensione del vettore.

`int size()`

da il numero di posizioni nel vettore.

`List subList(int fromIndex, int toIndex)`

crea una lista con gli elementi del vettore che si trovano dalla posizione specificata come inizio a quella come fine. List è un’altra struttura dati molto importante, che Java ha già implementata.

`Object[] toArray e Object[] toArray(Object[] a)`

creano un array del linguaggio con gli elementi contenuti nel vettore.

`toString()`

da una rappresentazione sotto forma di stringa del vettore, contenente le rappresentazioni sotto forma di stringa di tutti gli oggetti contenuti nel vettore, in pratica invoca il toString di tutti gli oggetti.

Chi è abituato a programmare in altri linguaggi di programmazione potrà apprezzare tutte le funzionalità dei vettori, funzionalità che spesso bisogna implementare da soli, spesso in modo sommario. Altra caratteristica apprezzabile è la possibilità di creare vettori con elementi eterogenei: questi sono vettori di oggetti e gli oggetti in Java possono essere qualsiasi cosa, anche programmi. Ad esempio è possibile creare un vettore contenente numeri interi, valori booleani e stringhe, cosa che non è possibile fare con gli array del linguaggio.

Per capire meglio facciamo un esempio. Pensate al seguente vettore:

pippo = { true , 10 , "Ciao"};

Di che tipo sarà `pippo`? `int[]`? `boolean[]`? O `String[]`? Nessuno dei tre, in Java è impossibile creare un array così fatto, usando però le famose classi involucro, che fin’ora ci sembravano abbastanza inutili, possiamo creare un array così fatto, scriveremo qualcosa del tipo.

Object\[\] pippo={ new Boolean(true), new Integer(10), new String("Ciao") };

Se inoltre usiamo la classe `Vector`, abbiamo anche parecchi metodi per gestire questo vettore di elementi eterogenei.

Facciamo un piccolo esempio che usa i vettori, definiamo uno spazio geometrico tridimensionale di punti, linee e facce, così definiti:

class punto
{
	String nome;
	int x,y,z;
	
	public punto(String n, int X,int Y,int Z)
	{
		x=X; y=Y; x=Z; nome=n;
	}

	public String toString()
	{
		return "\\n"+nome+"="+x+","+y+","+z+"\\n";
	}
}

class linea
{
	String nome;
	punto inizio;
	punto fine;
	
	public linea(String n, punto i,punto f)
	{
		inizio=i;
		fine=f;
		nome=n;
	}

	public String toString()
	{
		return "\\n"+nome+"=("+inizio.toString()+","+fine.toString()+")\\n";
	}
}

class faccia
{
	String nome;
	punto x1;
	punto x2;
	punto x3;
	linea l1;
	linea l2;
	linea l3;
	
	public faccia(String n, punto X1,punto X2,punto X3)
	{
		x1=X1;
		x2=X2;
		x3=X3;
		l1=new linea(n+"-linea1",x1,x2);
		l2=new linea(n+"-linea2",x3,x2);
		l3=new linea(n+"-linea3",x1,x3);
		nome=n;
	}
	
	public String toString()
	{
		return "\\n"+nome+"={"+l1.toString()+","+l2.toString()+","+l3.toString()+"}\\n";
	}
}

Da notare che le classi contengono dei metodi `toString()`, che sovrascrivono i metodi standard, questo per creare l’output su file del programma. Queste classi le metteremo in un file chiamato `Geometria.java`, ove metteremo anche le import necessarie al programma e la classe Geometria contenente il main sotto definite:

import java.util.*;
import java.io.*;

// Definizione della class punto
// Definizione della class linea
// Definizione della class faccia
public class Geometria
{
	public static int NUMERO = 3000;

	public static void main(String\[\] arg)
	{
		Vector spazio=new Vector(1,1);
		System.out.println("Geometria nello spazio:");
		int pti = NUMERO;
		System.out.println();
		System.out.println("Genero "+pti+" oggetti a caso per ogni specie");
		System.out.println("Cambiare la costante NUMERO in Geometria per generarne un numero diverso.");
		int lin=NUMERO;
		int fac;
		fac=NUMERO;

		System.out.println();
		int d1=spazio.capacity();
		System.out.println ("Capacità del vettore:"+spazio.capacity());
		System.out.println("Genero i punti...");
		int i=0;
		
		for ( i = 0 ; i<pti ; i++ )
		{
			float a=(float ) Math.random();
			float b=(float ) Math.random();
			float c=(float ) Math.random();
			int x=Math.round(a*1000);
			int y=Math.round(b*1000);
			int z=Math.round(c*1000);
			String nome="Punto"+(i+1);
			punto tmpP=new punto(nome,x,y,z);
			spazio.addElement(tmpP);
		};

		System.out.println ("Capacità del vettore:"+spazio.capacity());
		int d2=spazio.capacity();
		System.out.println("Genero le linee...");
		
		for ( i = 0 ; i<lin ; i++ )
		{
			float a=(float ) Math.random();
			float b=(float ) Math.random();
			float c=(float ) Math.random();
			float d=(float ) Math.random();
			float e=(float ) Math.random();
			float f=(float ) Math.random();

			int x=Math.round(a*1000);
			int y=Math.round(b*1000);
			int z=Math.round(c*1000);
			int x1=Math.round(d*1000);
			int y1=Math.round(e*1000);
			int z1=Math.round(f*1000);
			String nome="Linea"+(i+1);
			punto P1=new punto ("Punto 1 della "+nome,x,y,z);
			punto P2=new punto ("Punto 2 della "+nome,x1,y1,z1);
			linea tmpL=new linea(nome,P1,P2);
			spazio.addElement(tmpL);
		};

		System.out.println ("Capacità del vettore:"+spazio.capacity());
		int d3=spazio.capacity();

		System.out.println("Genero le facce...");
		for ( i = 0 ; i<lin ; i++ )
		{
			float a=(float ) Math.random();
			float b=(float ) Math.random();
			float c=(float ) Math.random();
			float d=(float ) Math.random();
			float e=(float ) Math.random();
			float f=(float ) Math.random();
			float g=(float ) Math.random();
			float h=(float ) Math.random();
			float j=(float ) Math.random();
			int x=Math.round(a*1000);
			int y=Math.round(b*1000);
			int z=Math.round(c*1000);
			int x1=Math.round(d*1000);
			int y1=Math.round(e*1000);
			int z1=Math.round(d*1000);
			int x2=Math.round(g*1000);
			int y2=Math.round(h*1000);
			int z2=Math.round(j*1000);
			String nome="Faccia"+(i+1);
			punto P1=new punto ("Punto 1 della "+nome,x,y,z);
			punto P2=new punto ("Punto 2 della "+nome,x1,y1,z1);
			punto P3=new punto ("Punto 1 della "+nome,x2,y2,z2);
			faccia tmpF=new faccia(nome,P1,P2,P3);
			spazio.addElement(tmpF);
		};
	
		System.out.println ("Capacità finale del vettore:"+spazio.capacity());
		int d4=spazio.capacity();
		File FileOut=new File("Geometria.txt");
		FileWriter Output;
		try {Output=new FileWriter(FileOut);}
		catch (IOException e) {Output=null;};
		try {
			Output.write("Geometria.txtnnumero oggetti="+3*NUMERO+"\\n");
			Output.write("Dimensioni del Vector:nall'inizio:"+\\d1+"n dopo l'inserzione dei punti:"+d2);
			Output.write("\\ndopo l'inserzione delle linee:"+\\d3+"ndopo l'inserzione delle facce:"+d4);
			Output.write("\\n\\nContenuto:n");
			Output.write(spazio.toString());
			Output.write("\\n\\n\\n\\t\\tPietro Castellucci");
			Output.flush();
		} catch (IOException e) {};
		System.out.println("\\nGuarda il file Geometria.txt.\\n");
	};
}

Se mandiamo in esecuzione il programma vediamo come il Vector spazio cresce dinamicamente, con gli oggetti eterogenei punto, linea e faccia. Alla fine avremo un file chiamato Geometria.txt che conterrà la descrizione del vettore, il file con soli 5 elementi per ogni specie sarà qualcosa del tipo:

Geometria.txt
numero oggetti=15 Dimensioni del Vector: all'inizio:1
dopo l'inserzione dei punti:5
dopo l'inserzione delle linee:10
dopo l'inserzione delle facce:15

Contenuto:
\[
Punto1=653,932,0, 
Punto2=100,273,0, 
Punto3=855,210,0, 
Punto4=351,702,0, 
Punto5=188,996,0, 

Linea1=(
Punto 1 della Linea1=680,454,0, 
Punto 2 della Linea1=69,16,0
), 
Linea2=(
Punto 1 della Linea2=116,651,0,
Punto 2 della Linea2=371,15,0
), 
Linea3=(
Punto 1 della Linea3=947,335,0,
Punto 2 della Linea3=477,214,0
), 
Linea4=(
Punto 1 della Linea4=391,671,0,
Punto 2 della Linea4=692,725,0
), 
Linea5=(
Punto 1 della Linea5=762,283,0,
Punto 2 della Linea5=582,192,0
), 

Faccia1={
Faccia1-linea1=(
Punto 1 della Faccia1=826,235,0,
Punto 2 della Faccia1=13,378,0
),
Faccia1-linea2=(
Punto 1 della Faccia1=12,950,0,
Punto 2 della Faccia1=13,378,0
),
Faccia1-linea3=(
Punto 1 della Faccia1=826,235,0,
Punto 1 della Faccia1=12,950,0
)}, 

Faccia2={
Faccia2-linea1=(
Punto 1 della Faccia2=382,30,0,
Punto 2 della Faccia2=224,597,0
),
Faccia2-linea2=(
Punto 1 della Faccia2=277,361,0,
Punto 2 della Faccia2=224,597,0
),
Faccia2-linea3=(
Punto 1 della Faccia2=382,30,0,
Punto 1 della Faccia2=277,361,0
)},
 
Faccia3={
Faccia3-linea1=(
Punto 1 della Faccia3=139,802,0,
Punto 2 della Faccia3=880,935,0
),
Faccia3-linea2=(
Punto 1 della Faccia3=643,921,0,
Punto 2 della Faccia3=880,935,0
),

Faccia3-linea3=(
Punto 1 della Faccia3=139,802,0,
Punto 1 della Faccia3=643,921,0
)}, 

Faccia4={
Faccia4-linea1=(
Punto 1 della Faccia4=516,614,0,
Punto 2 della Faccia4=429,210,0
),
Faccia4-linea2=(
Punto 1 della Faccia4=979,860,0,
Punto 2 della Faccia4=429,210,0
),
Faccia4-linea3=(
Punto 1 della Faccia4=516,614,0,
Punto 1 della Faccia4=979,860,0
)}, 

Faccia5={
Faccia5-linea1=(
Punto 1 della Faccia5=152,663,0,
Punto 2 della Faccia5=828,101,0
),
Faccia5-linea2=(
Punto 1 della Faccia5=651,761,0,
Punto 2 della Faccia5=828,101,0
),
Faccia5-linea3=(
Punto 1 della Faccia5=152,663,0,
Punto 1 della Faccia5=651,761,0
)}
\]

		Pietro Castellucci

Provate a mettere la costante NUMERO a 4000 e ad eseguire il programma. Come avete visto abbiamo tante strutture dati in java.util, descriverle tutte sarebbe un lavoraccio. La cosa da sottolinare è che tutte hanno metodi per inserire, togliere, recuperare e metodi per gestire la struttura stessa.

Le strutture dati implementate sono tabelle hash, liste, pile, code, alberi ecc… e a seconda della vostra esigenza ne userete una o l’altra, per i dettagli però vi consiglio di vedere la documentazione del Java Development Kit, dove sono descritte tutte queste classi.

Per ora vi basti sapere che le **tabelle hash** sono velocissime nella ricerca di un oggetto, tipicamente basta un solo accesso alla struttura dati per beccare l’oggetto cercato.

Le **liste** sono come i vettori, gli oggetti sono legati tra di loro, e da un oggetto è possibile raggiungere il seguente, o il precedente o tutti e due, a seconda della realizzazione della lista.

Le **Pile** sono selle liste particolari, dove gli oggetti vengono inseriti sempre in testa alla struttura, e da qui vengono prelevati, quindi nella pila l’ultimo oggetto entrato è il primo ad uscire, immaginate gli oggetti come delle pratiche da sbrigare da un ufficio, esse vengono poste in ordine di come arrivano sulla scrivania dell’addetto, una sull’altra. L’addetto alle pratiche prende sempre la pratica in testa alla pila e la elabora. Questa può sembrare una struttura dati stupida, ma vi assicuro che è la struttura dati più importante di tutte, essa è utilizzata da tutti i linguaggi di programmazione per la chiamata di funzioni e di procedure (anche Java la usa per l’invocazione di dei metodi), per il passaggio dei parametri e il recupero dei risultati, e senza di questa struttura sarebbe impossibile programmare in modo ricorsivo, il quale è un modo di programmare molto potente.

Le **code** funzionano invece al contrario, sono paragonabili alle code in banca, gli oggetti sono le persone che fanno la fila allo sportello, esse arrivano e si mettono alla fine della fila, intanto l’operatore serve le persone che sono in testa alla fila, anche questa struttura è importantissima, essa e alcune sue varianti (le code a priorità, dove gli oggetti entrano con una priorità, ed in base a questa vengono serviti, ovvero saltano la fila) sono molto usate nei sistemi operativi come Windows, Linux, Unix, eccetera, ad esempio per gestire le richieste di stampa verso una sola stampante da parte di tutti gli utenti del sistema.

Ricerca di un oggetto, differenze tra vettore e hash
----------------------------------------------------

Prima di passare al prossimo package però esaminiamo un piccolo paragone sulla ricerca di un oggetto tra tanti oggetti, racchiusi in strutture diverse, in particolare in strutture di tipo vettore e di tipo tabelle hash.

Vedremo come le performances della ricerca cambiano in modo evidente al crescere delle dimensioni delle strutture.

Immaginiamo degli oggetti fatti così:

class O 
{
	String nome=new String();
	int valore;
	
	public O(String a, int v)
	{
		nome=a;
		valore=v;
	}
}

Creiamo un vettore di `100000` di questi elementi, ne cerchiamo uno e vediamo quanto tempo impieghiamo:

import java.util.*;

class O
{
	String nome=new String();
	int valore;
	
	public O(String a, int v)
	{
		nome=a;
		valore=v;
	}
}

public class Rvettore
{
	public static int NUMERO = 100000;
	
	public static void main (String\[\] s)
	{
		Vector V = new Vector(NUMERO);
		int numnome=128756;
		O O_30000 = null;

		System.out.println("Genero il vettore, inserisco "+NUMERO+" oggetti di tipo O con valori scorrelati rispetto all'indice.");
		
		System.out.print("Inizio: O_"+numnome);
		
		for (int i=0; i< NUMERO;i++)
		{
			O tmp = new O("O_"+numnome, numnome);
			
			if (numnome==30000) O_30000 = tmp;
			
			numnome--;
			V.add(i, tmp);
		};

		System.out.println(" Fine: O_"+numnome);
		System.out.println("Inizio la ricerca di O_30000");
		
		long Inizio=System.currentTimeMillis();
		int index=V.indexOf(O_30000);
		
		O tmp = (O) V.get(index);
		
		long Fine=System.currentTimeMillis();

		System.out.println("Oggetto O_30000 trovato in "+(Fine-Inizio)+" millisec. all'indice "+index);
		System.out.println("Esso vale:\\nNome:"+tmp.nome+"\\nValore:"+tmp.valore+"\\n");
	}
}

Il programma cercherà l’oggetto `O_30000`, l’output del programma è:

Genero il vettore, inserisco 100000 oggetti di tipo O con valori scorrelati rispetto all'indice.

Inizio:  O\_128756 Fine:  O\_28756
Inizio la ricerca di O_30000
Oggetto O_30000 trovato in 50 millisec.   .....

Adesso scriviamo un programma simile usando come struttura una tabella hash:

import java.util.*;

class O
{
	String nome=new String();
	int valore;
	public O(String a, int v)
	{
		nome=a;
		valore=v;
	}
}

public class Rhash
{
	public static int NUMERO = 100000;
	
	public static void main (String\[\] s)
	{
		Hashtable H = new Hashtable(NUMERO+1);
		int numnome=128756;
		O O_30000=null;
		System.out.println("Genero la tabella, inserisco "+NUMERO+" oggetti di tipo O.");
		System.out.print("Inizio: O_"+numnome);
		
		for (int i=0; i< NUMERO;i++)
		{
			O tmp = new O("O_"+numnome,numnome);
			if (numnome==30000) O_30000 = tmp;
			numnome--;
			H.put(tmp,tmp);
		};

		System.out.println(" Fine O_"+numnome);
		System.out.println("Inizio la ricerca di O_30000");
		long Inizio=System.currentTimeMillis();
		O tmp=(O ) H.get(O_30000);
		long Fine=System.currentTimeMillis();
		System.out.println("Oggetto O_30000 trovato in "+(Fine-Inizio)+" millisec. ");
		System.out.println("Esso vale:\\nNome:"+tmp.nome+"\\nValore:"+tmp.valore+"\\n");	
	}
}

Ancora una volta il programma cercherà l’oggetto `O_30000`, l’output del programma questa volta sarà:

Genero il vettore, inserisco 100000 oggetti di tipo O.
Inizio:  O\_128756 Fine:  O\_28756
Inizio la ricerca di O_30000
Oggetto O_30000 trovato in 0 millisec.   .....

Il package java.util non comprende solo queste strutture dati, esso comprende anche delle classi per facilitare la gestione di date e orari, per l’interazionalizzazione, ed altre classi di utilità, come lo string tokenizer, che prende da una stringa tutte le parole, e dei generatori di numeri casuali.