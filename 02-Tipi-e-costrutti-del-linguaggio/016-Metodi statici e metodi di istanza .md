Tra tutti i qualificatori utilizzabili nella dichiarazione di un metodo quello che più di ogni altro ne modifica il funzionamento (nel senso che chiariremo tra poco) è **static**.

Per comprendere l’uso della keyword `static` in Java bisogna ricordare innanzi tutto che, come detto, ogni metodo appartiene ad una classe ed una classe è, in qualche modo, un “pacchetto” di dati e metodi.

Sappiamo che da una classe possiamo ottenere molteplici istanze e per ciascuna istanza si hanno variabili dai nomi identici ma dai valori distinti (forse “che puntano a locazioni di memoria diverse” sarebbe una definizione più chiara). Se poi vogliamo che una variabile sia la medesima per tutte le istanze di una classe sappiamo che la dobbiamo invece definire come `static`.

Per i metodi avviene sostanzialmente la medesima cosa: possiamo pensare che dei metodi definiti in una classe ne esista normalmente (cioè se non si specifica `static`) una “copia” per ogni istanza della classe, mentre dei metodi statici ne esista una sola copia associata alla classe stessa. Per scendere più in dettaglio:

*   i **metodi non statici** sono associati ad ogni singola istanza di una classe e perciò il loro contesto di esecuzione (quindi l’insieme delle variabili cui possono accedere) è relativo all’istanza stessa: possono accedere e modificare le variabili dell’istanza e modificarne lo stato;
*   in contapposizione i **metodi statici** non sono associati ad una istanza ma solo ad una classe. Quindi non potranno interagire con le variabili di istanza, ma solamente con quelle statiche.

Questa distinzione tra metodi statici e metodi di istanza si riflette anche in una diversa sintassi che si deve utilizzare per eseguire i 2 tipi di metodi:

Tipo di metodo

Sintassi

**Statico**

`NomeClasse.nomeMetodo(...)`

**Non statico** (di istanza)

`nomeIstanza.nomeMetodo(...)`

Per `NomeClasse` si intende il nome di una classe e non di una istanza (come si potrebbe intuire anche dalla convenzione per cui le istanze non iniziano mai con una lettera maiuscola).

Per la precisione anche i metodi statici possono essere richiamati utilizzando una istanza invece che il nome della classe, ma questa è considerata una cattiva pratica e segnalata dal compilatore con un warning.

Cerchiamo di chiarire ulteriormente la differenza tra i diversi tipi di metodi utilizzando il codice seguente in cui si presenta la struttura di base di una micro-libreria per la rappresentazione di figure piane (solo i parallelogrammi sono definiti naturalmente).

Innanzi tutto definiamo una classe di metodi di servizio utilizzabili dovunque e non legati ad una specifica istanza, quindi tutti statici:

```
public class Geometry2DUtils {
	public static final double PiGreco = 3.141592;
	public static double areaParallelogramma(double base, double altezza) { }
	public static double areaParallelogramma(double c1, double c2, double alpha) { }
	// ...
	public static double areaCerchio(double r) {
		// anche se static potrà usare la variabile statica PiGreco
		return PiGreco * r * r;
	}
}
```

continuiamo poi con una classe che rappresenti i parallelogrammi

```
public class Parallelogramma {
	private double base, altezza;
	public Parallelogramma(double base, double altezza) { 
		// inzializza le variabili di istanza
		this.base = base;
		this.altezza = altezza;
	} 
	// questo metodo area non ha bisogno dei
	// parametri in quanto utilizza le variabili della classe
	public double area() {
		// utilizza il metodo (pubblico e statico) nella classe  di utility
		// chiamandolo usando il nome della classe (poi il compilatore
		// sceglie quello giusto sulla base dei parametri)
		return Geometry2DUtils.areaParallelogramma(base, altezza);
	}
	// metodi per l'accesso (getters) delle variabili private della classe,
	// garantiscono, tra l'altro, che i field possano essere utilizzati ma non
	// modificati
	public double getBase()    { return base; }
	public double getAltezza() { return altezza; }
}
```

A questo punto potremo utilizzare le nostre classi in un immaginario metodo main (static)

```
public class Programma {
	public static void main(String[] args) {
		// creiamo una istanza della classe Parallelogramma
		// e la assegnamo alla variabile p (che ha il tipo opportuno)
		Parallelogramma p = new Parallelogramma(4.0, 8.0);
		// creiamo una seconda istanza di Parallelogramma che avrà
		// metodi e variabili con i medesimi nomi di p ma distinti
		// quanto a valore e contesto di esecuzione
		Parallelogramma p1 = new Parallelogramma(11.0, 18.0);
		double pArea  = p.area();  // assegna a pArea 4*8
		double p1Area = p1.area(); // assegna a p1Area 11*18
		// il medesimo risulato di pArea si avrebbe naturalmente con
		double spArea = Geometry2DUtils.areaParallelogramma(p.getBase(), p.getAltezza());
		// ma con una minore chiarezza circa l'oggetto dell'operazione effattuata. 
		//...
	}
}
```