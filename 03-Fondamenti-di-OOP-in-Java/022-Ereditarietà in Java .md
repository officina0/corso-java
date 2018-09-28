Tra gli elementi più importanti che caratterizzano il paradigma di sviluppo OOP c’è la _Gerarchia_. Tenendo a mente questo fatto non è difficile convincersi di quanto il concetto di **ereditarietà** sia centrale nella programmazione Object Oriented.

Definizione di ereditarietà
---------------------------

Si dice che una classe **A è una sottoclasse di B** (e analogamente che _B è una superclasse di A_) quando:

*   _A_ eredita da _B_ sia il suo stato che il suo behavior (comportamento)
*   e quindi un’istanza della classe _A_ è utilizzabile in ogni parte del codice in cui sia possibile utilizzare una istanza della classe _B_.

Questa ultima parte della definizione va sotto il nome di “[Principio di sostituzione di Liskov](http://www.html.it/pag/32393/l-liskov-substitution-principle/ "Principio di sostituzione di Liskov")” ed è un invariante di importanza capitale che va tenuto presente ogni volta che si pensa a come strutturare una gerarchia di classi.

Ereditatiertà strutturale (sub-typing)
--------------------------------------

In questa sezione analizziamo la più semplice forma di ereditarietà disponibile in Java (che potremmo definire strutturale o sub-typing) ma vale la pena di tenere da subito presente che la definizione di ereditarietà che abbiamo sopra dato è realizzabile in almeno 3 modi fondamentalmente dissimili:

*   Il primo consiste nell’esprimere il fatto che un elemento della classe A “**è anche**” un elemento della classe B (un cerchio è anche una figura piana);
*   il secondo consiste nel dire che gli elementi della classe A **si comportano come** elementi della classe B (il cerchio ha un’area ed un bounding-rectangle proprio come tutte le figure piane);
*   il terzo, infine, sintatticamente sensibilmente più complicato, quando si arriva al principio di sostituzione, esprime la relazione di ereditarietà per incapsulamento ed è leggibile come gli elementi della classe A hanno un elemento della classe B che può quindi essere usato quando ce ne fosse l’esigenza.

Nel seguito ci occuperemo esclusivamente della prima forma di ereditarietà (che va sotto il nome di ereditarietà strutturale ed anche sub-typing) lasciando la seconda alla discussione sulle interfacce mentre la terza verrà in qualche modo affrontata quando parleremo di OO Design e Patterns.

Extends, estendere (o derivare da) una classe in Java
-----------------------------------------------------

In Java la relazione di derivazione viene resa con la keyword **extends** che deve essere usata nella dichiarazione della classe:

```
class A extends B {
	// ...
}
```

Implica che ogni istanza della classe _A_ sarà anche di tipo _B_ ed avrà a disposizione tutti i metodi della classe _B_ (potrà ricevere tutti i messaggi che può ricevere la classe _B_, usando la terminologia delle precedenti sezioni) e nel suo stato saranno presenti tutte le variabili che si trovano nella super classe _B_.

Nella **notazione UML** si esprime la relazione di ereditarietà fra _A_ e _B_ mediante una freccia che va da _A_ a _B_, come mostrato dalla seguente figura.

![IS A](https://tbm-html.s3.amazonaws.com/app/uploads/2014/12/java22_01.png)

Esempio di ereditarietà in Java
-------------------------------

Un esempio concreto di ereditarietà potrebbe essere il seguente, nel quale esempio intendiamo dare una bozza di modello per un generico magazzino in cui parte dei colli sono per la vendita.

```
/**
 * Collo è la classe "base"
 */
public class Collo {
	// dati
	private int x_size, y_size, z_size;
	protected int weight;
	// funzione getter di Weight
	public int getWeight() { return weight; }
	// Costruttore
	public Collo(int w, int xs, int ys, int zs) {
		this.weight = w;
		this.x_size = xs;
		this.y_size = ys;
		this.z_size = zs;
	}
	public int getVolume() {
		return x_size * y_size * z_size;
	}
}
/**
 * ColloInVendita è la classe "derivata"
 */
public class ColloInVendita extends Collo {
	// dati (oltre quelli di Collo)
	private int price;
	// coefficienti da applicare alla vendita
	private static final float A0 = 1;
	private static final float B0 = 1.2;
	private static final float C0 = 1.5; 
	public int getPrice() {
		return price;
	}
	// Costruttore della classe derivata
	public ColloInVendita(int w, int xs, int ys, int zs, int price) {
		// richiama il costruttore della classe base
		super(w, xs, ys, zs);
		this.price = price;
	}
	public float getDeliveryCost() {
		return A0*weight + B0*getVolume() + C0*price;
	}
}
```

È utile osservare come, nella dichiarazione di `ColloInVendita`, siano utilizzabili sia i metodi che i field (a patto che siano `public` o `protected`) della super-classe `Collo`, che possono essere eventualmente prefissati con la keyword `super` utile per eventuali disambiguazioni.

Una nota speciale merita il costruttore, infatti il costruttore della classe `ColloInVendita` (derivata) deve essere in grado di costruire una istanza della classe `Collo` e quindi se per quest’ultima non è previsto un costruttore ‘default’ (senza argomenti) la classe derivata lo dovrà chiamare esplicitamente passandogli gli argomenti necessari con la sintassi **super(…)** che deve essere obbligatoriamente il primo statment del costruttore della classe figlia.

Leggendo la dichiarazione della classe `Collo`, dove non compare la keyword `extends`, potrebbe nascere la convinzione che questa classe non derivi da nessuna “super-class”; per la verità vale la pena di osservare che **in Java ogni classe deriva da almeno una classe genitrice** che, se la keyword `extends` viene omessa, è per default la classe [`java.lang.Object`](http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html "Java Object"), dalla quale ogni oggetto Java eredita, per esempio, i metodi `hashCode()`, `getClass()`, `toString()`.

Strutturare la gerarchia delle classi
-------------------------------------

Scegliere le gerarchie di oggetti in una analisi OO non è operazione semplice e difficilmente priva di ambiguità e dalla cui buona riuscita dipende l’efficacia dell’intera struttura ad oggetti che stiamo costruendo.

Quando ci si accinge a modellare uno specifico problema con il paradigma ad oggetti è probabilmente importante tenere presente _che non esiste una gerarchia di classi giusta in assoluto_, ma conta lo specifico problema che si intende risolvere.

A tal proposito è interessante riflettere sul classico paradosso della modellazione di cerchi ed ellissi riassunto dalla domanda _“is a circle a kind of ellipse?”_ da cui prende il titolo anche una sezione della [C++ faq](http://www-igm.univ-mlv.fr/~dr/CPP/c++-faq/).

Domandandosi infatti se nel contesto della realizzazione di una gerarchia di classi per la modellazione di figure piane sia opportuno ottenere la classe `cerchio` come derivata della classe _ellisse_ ci si trova a dover concludere che benchè matematicamente non ci possano essere dubbi che la relazione è ben definita (un cerchio è di sicuro una ellisse con assi della medesima lunghezza), dal punto di vista OO se la classe ellisse fornisce un metodo per cambiare asimmetricamente le proprie dimensioni (ad esempio `setSize(size_x, size_y)`) la relazione di sub-typing non può essere utilizzata in quanto ogni eventuale utilizzo di questo metodo richiederebbe che la classe cerchio si trasformasse nella classe ellisse; cosa che, almeno in Java, non è possibile.

Link utili

Una interessante discussione sul problema del cerchio/ellisse è reperibile su [Wikipedia](http://en.wikipedia.org/wiki/Circle-ellipse_problem)) dove sono discusse anche le contromisure adottabili in diversi contesti e con diversi linguaggi.