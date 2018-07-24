Le **classi astratte in Java** sono utilizzate per poter dichiarare caratteristiche comuni fra classi di una determinata gerarchia. Pur definendo il nome di un tipo, la classe astratta non può essere instanziata, analogamente a quanto accade per le interfacce; ma a differenza di una interfaccia può avere field non statici, metodi non pubblici, un costruttore, insomma una classe a tutti gli effetti ma non istanziabile.

La sua dichiarazione è caratterizzata dall’utilizzo della keyword **abstract**:

public abstract class A {
	//campi 
	//metodi
}

Una classe astratta può contenere o meno metodi astratti, ma una classe che contiene metodi astratti deve necessariamente essere dichiarata come astratta, i.e. facendo uso della keyword abstract.

Esempi, classi e metodi astratti
--------------------------------

Il seguente esempio mostra una classe `A` che dichiara al suo iterno un metodo astratto. Questo codice dà errore di compilazione, in quanto, in base a quanto detto sopra, una classe che contiene metodi astratti deve necessariamente essere dichiarata come astratta.

public class A {
	public abstract void metodo();
}

L’esempio che segue invece è corretto dal punto di vista sintattico, infatti la classe ha dichiarato al suo interno un metodo abstract e la classe è di tipo astratto.

public abstract class A {
    //campi
    private boolean visible=true;
    //metodi
    public boolean isVisible() { return this.visible; }
    public abstract void metodo();
}

Vale la pena di osservere che come in ogni altra classe, una classe astratta può dichiarare dei campi (che ne descrivono lo stato) e dei metodi (non astratti) che ne specificano il funzionamento. Un metodo astratto è un metodo che può essere dichiarato ma per il quale non viene fornita una implementazione; la sintassi da utilizzare in generale è la seguente:

abstract <return type> nomeMetodo(lista dei parametri);

A cosa serve una classe astratta
--------------------------------

_Una classe astratta non può in alcun modo essere istanziata_, quindi può essere utilizzata esclusivamente come classe base. Quando estendiamo (o deriviamo) da una classe astratta, la **classe derivata deve fornire una implementazione per tutti i metodi astratti** dichiarati nella classe genitrice; se così non dovesse essere, anche la sotto-classe deve essere dichiarata come _abstract_.

Lo scopo e l’utilità delle classi astratte è di gestire il comportamento di base delle classi che la derivano e che può essere ampliato e specializzato da queste ultime.

Per esempio, immaginiamo che un certo processo necessita di tre fasi:

1.  Inizializzazione del processo
2.  Azione principale
3.  Step successivo all’azione

Se le due fasi di inizializzazione e lo step successivo all’azione rimangono invariati, il terzo passo (cioè l’azione principale) può essere implementato e considerato come una classe astratta (in modo da essere specializzato dalle classi derivate).

public abstract class AbstractProcess {

	public void process() {
		init();
		baseAction();
		stepAfter();
	}
	
	public void init() {
		
		// metodo dichiarato direttamente 
		// all'interno della classe astratta
	}
	
	public abstract void baseAction();
	
	// metodo astratto la cui implementazione 
	// è demandata alla sottoclasse
	
	public void stepAfter() {
	
		// metodo dichiarato direttamente
		// all'interno della classe astratta
	}
}

class MyProcess extends AbstractProcess {

	@Override
	public void baseAction() {
		// implementazione del metodo baseAction()
	}
}

Notiamo che il metodo `baseAction()` nella classe `AbstractProcess` è astratto, quindi la sottoclasse che la estende deve effettuarne l’Overidde e darne l’implementazione (altrimenti anch’essa dovrebbe essere dichiarata `abstract`).

A questo punto la chiamata al metodo `process()`:

MyProcess myProcess = new MyProcess();

// AbstractProcess myProcess = new MyProcess(); 

// equivalente alla precedente
myProcess.process();

definito nella classe astratta esegue il processo, chiamando il metodo `baseAction` come definito in `MyProcess`.

Interfacce e classi astratte, differenze
----------------------------------------

Come accennato sopra, le interfacce e le classi astratte sono elementi molto simili, e la loro smilitudine è aumentata in Java8 che ha introdotto la possibilità di definire una implementazione, detta default, dei metodi dichiarati nelle interfaccie.

 

Interfacce

Classi astratte

Istanziabile

no

no

Fields

solo `static final`

sì

Costruttore

no

sì

Metodi statici

Java8+

sì

Dichiarazione metodi (virtual)

no

sì

Implementazione metodi

java8+ (con il qualificatore `default`)

sì

Entrambe non possono essere instanziate e possono dichiarare al loro interno metodi con o senza implementazione.

Tuttavia ci sono delle caratteristiche che le differenziano e che fanno la differenza, come per esempio le classi astratte possono dichiarare campi che sono non `static` e `final`, e dichiarare metodi `public`, `protected` e `private`.

Invece con le interfacce tutti i campi sono automaticamente public, static e final e tutti i metodi che vengono dichiarati o definiti sono public.

Un’altra importante caratteristica che le differenzia e che spesso è punto focale nella scelta di uno o dell’altro approccio, è il fatto che si può estendere una sola classe astratta, mentre di possono implementare tutte le interfacce che si vuole.

### Quando utilizzare l’interfaccia e quando la classe astratta?

In base a quanto detto, cosa è meglio utilizzare? Ed in quali circostanze?

*   Si usa una **classe astratta** per condividere codice fra più classi, se più classi hanno in comune metodi e campi o se si vogliono dichiarare metodi comuni che non siano necessariamente campi `static` e `final`.
*   Si decide di utilizzare una **interfaccia** se ci si trova nella situazione in cui alcune classi (assolutamente non legate fra di loro) si trovano a condividere i metodi di una interfaccia, se si vuole _specificare il comportamento di un certo tipo di dato_ (ma non implementarne il comportamento) o se si vuole avere la possibilità di sfruttare la “multiple inheritance”.

Un esempio di utilizzo di classi astratte nella JDK è rappresentato dalla “[AbstractMap](http://docs.oracle.com/javase/8/docs/api/java/util/AbstractMap.html "AbstractMap")“, che fa parte delle _Collections_: tutte le sue sottoclassi (incluse le `HashMap`, `TreeMap`, …) hanno metodi in comune e che sono definiti all’interno della classe “`AbstractMap`” (come i metodi `put`, `get`, `isEmpty`, `containsKey`, etc.).

Un esempio di classe che implementa molte interfacce nella JDK, invece, è rappresentato da [HashMap](http://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html "titolo - link esterno"), che implementa le interfacce `Serializable`, `Clonable` e `Map<K,V>` (che usa la sintassi dei generics che sarà affrontata più avanti nel nostro percorso su Java).

Quando una classe astratta implementa un’interfaccia
----------------------------------------------------

Nelle sezioni precedenti abbiamo visto che se una classe implementa un’interfaccia deve necessariamente implementare tutti i metodi che sono stati dichiarati nell’interfaccia stessa.

È counque possibile definire una classe che non implementa tutti i metodi dell’interfaccia, questo comporta che la classe deve necessariamente essere astratta.

Per esempio se abbiamo una interfaccia `InterfaceVideo`:

public interface InterfaceVideo {

	static String format="xyz";
	
	public void run();
	public void start();
	public void stop();
	public void rewind();
}

La classe `BaseVideo` può estenderla ed implementare uno solo dei suoi metodi `run(`) e non tutti gli altri, ma deve essere dichiarata abstract altrimenti otteniamo un errore dal compilatore:

public abstract class Video implements BaseVideo {
    
    @Override
    public void run() {
		// definizione del funzionamento del metodo
    }
}