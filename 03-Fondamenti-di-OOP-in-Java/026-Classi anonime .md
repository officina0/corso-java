Una classe anonima è una classe “locale” senza un nome assegnato, si tratta di una classe definita e instanziata un’unica volta attraverso una singola espressione caratterizzata da una versione estesa della sintassi dell’operatore `new`.

Quindi, mentre le classi inner sono definite in tutto e per tutto in maniera identica a come si definiscono le comuni classi, con la classica dichiarazione che fa uso della keyword `class`), le classi anonime sono definite ed istanziate in un’unica espressione.

### Sintassi delle classi anonime

La sintassi per definire una classe anonima è idetica a quella utilizzata per la chiamata di un costruttore, eccetto per il fatto che la chiamata al costuttore è seguita da un blocco di codice che estende la classe stessa (o implementa metodi nel caso si crei una istanza anonima di una intefaccia).

A seconda che la classe anonima estenda una classe o implementi un’interfaccia si possono avere due varianti:

```
new ClassName ( [ argument-list ] ) { class-body }
```

oppure:

```
new InterfaceName () { class-body }
```

in cui la differenza consiste nel fatto che quando si istanzia anonimamente una classe si deve usare uno dei costruttori disponibili, al quale vanno dunque passati gli opportuni argomenti, mentre per istanziare una classe anonima basata su una interfaccia (che quindi la implementerà) è possibile fare uso solamente del costruttore ‘default’ senza argomenti.

Le classi anonime consentono di rendere il codice molto più conciso, dando la possibilità di dichiarare ed instanziare una classe al tempo stesso e devono essere considerate in tutto e per tutto identiche alle classi (inner), eccetto per il fatto che non hanno un nome.

Possiamo utilizzare le classi anonime quando abbiamo la necessità di utilizzare una classe localmente ed una volta sola. Consideriamo il seguente esempio, dove è data una interfaccia:

```
public interface TitledName {
	public String femaleTitle(String name);
	public String maleTitle(String name);
}
```

il modo “classico” di instanziare un oggetto sarebbe:

```
BaseTitle frenchTitle = new FrenchTitle();
```

che dovrebbe essere corredato dalla definizione di una opportuna classe che implementi l’interfaccia

```
public class FrenchTitle implements TitledName {
	@Override
	public String femaleTitle(String name) {
		return "Mademoiselle "+name;
	}
	@Override
	public String maleTitle(String name) {
		return "Monsieur "+name;
	}
}
```

mentre una Instanza di un oggetto anonimo può essere ottenuta semplicemente ed in un sol colpo come:

```
BaseTitle englishTitle= new TitledName() {
	@Override
	public String femaleTitle(String name) {
		return "Ms "+name;
	}
	@Override
	public String maleTitle(String name) {
		return "Mister "+name;
	}
};
```

Nell’esempio possiamo notare che le classi anonime hanno caratteristiche precise:

*   dichiarazione attraverso l’_operatore new_;
*   _interfaccia da implementare o classe da estendere_. L’interfaccia `TitledName` nel codice;
*   _parentesi con gli argomenti per il costruttore_, proprio come una classica espressione di istanziazione di una classe. Visto però che nell’esempio viene implementata un’interfaccia, quindi non ci sono costruttori,e questo significa che potrà essere chiamato solamente il costruttore di default;
*   il corpo della classe; in particolare, all’interno del corpo di definizione della classe anonima sono consentiti metodi, ma non istruzioni singole; naturalmente quando si istanzia anonimamente una interfaccia tutti i metodi devono essere implementati e allo stesso modo: tutti i metodi virtuali di una classe astratta andrebbero implementati.

Poiché la definizione di una classe anonima è un’espressione, deve essere parte di un’istruzione; nell’esempio sopra si tratta dell’istruzione di dichiarazione ed inizializzazione dell’oggetto `englishTitle` (il che spiega anche la presenza del “`;`” dopo l’ultima parentesi graffa chiusa).

Per le classi anonime valgono sostanzialmente le medesime regole delle classi locali per quanto riguarda l’accesso alle variabili locali:

*   una classe anonima ha accesso a tutti i membri della classe che la racchiude (anche a quelli privati)
*   Una classe anonima può essede definita anche dentro al corpo di un metodo, in tal caso sono visibile per la classe anonima solo le variabili final (ed effectively final da java 8)

In una classe anonima si possono dichiarare campi, metodi (anche se non implementano nessun metodo astratto della classe che si estende o dell’interfaccia che si implementa), classi locali ma non si possono definire costruttori, e per quanto riguarda i membri si applicano le medesime regole delle classi inner e non si possono avere membri statici tranne che le costanti.

Le classi anonime sono una caratteristica del linguaggio Java utilizzata in numerosi contesti ma probabilmente il loro uso più comune è quando ci si trovano a realizzare interfacce grafiche dove il predominante approccio “[a eventi](http://en.wikipedia.org/wiki/Event-driven_programming "event driven programming")” richiede la creazione di numerosi ogetti tutti simili (cioè tutti derivati di una determinata classe o interfaccia).

Per esempio per realizzare una intefaccia che utilizzi un bottone con [JavaFX](http://www.html.it/articoli/introduzione-a-javafx-1/ "JavaFX, introduzione") o con altre librerie per la creazione di interfacce grafiche (Swing, Java.awt), si dovrebbe scrivere qualcosa del tipo:

```
public void start(Stage primaryStage) {
	Button btn = new Button();
	btn.setText("Click");
	btn.setOnAction(new EventHandler<ActionEvent>() {
			@Override
			public void handle(ActionEvent event) {
				// instructions
			}
		});
}
```

dove l’effetto del “click” sul bottone è controllato da un oggetto (chiamato in questo caso `EventHandler`, altre liberire usano, con poche differenze da questo punto di vista, il termine _Listener_) che viene creato “creato anonimamente” con notevole risparmio di codice rispetto alla creazione di una classe non anonima ed anche con un incremento della leggibilità (non appena si prenda dimestichezza con questo stile di programmazione).

In dettaglio nell’esempio si specifica l’azione che verrà effettuata al click sul bottone “Click” usando il metodo `setOnAction` che richiede come parametro un oggetto di tipo `EventHandler<ActionEvent>` e questa interfaccia ha un unico metodo handle che viene implementato direttamente in linea.

Vale la pena di osservare che a causa del fatto che l’interfaccia `EventHandler` contiene solamente un metodo, con Java 8 potremmo ottenere lo stesso risultato utilizzando una di quelle che si chiamano **lambda expression** e che usata al posto della classe anonima renderebbe il codice ancora più conciso e fluente. Le _Lambda espression_ verranno affrontate in una futura lezione.

Per tutte le classi nested vale la regola che la eventuale dichiarazione di una variabile, membro o paramentro che abbia il medesimo nome di una variabile, metodo o parametro della classe (o metodo) ospitante “shadows” (copre) la dichiarazione più esterna. In un esempio:

```
public class Ospite {
	int variabile = 5;
	ClasseInner classeInner = new NomeInterfaccia() {
		int variabile = 6;
		// ...
		@Override
		public metodoOverridden(...) {
			System.out.println(variabile);  // STAMPA 6
		}
		@Override
		public altroMetodoOverridden(...,int variabile,...) {
			System.out.println(variabile); // STAMPA il valore passato come parametro
			System.out.println(this.variabile); // STAMPA 6, field di questa classe
			System.out.println(Ospite.this.variabile); // STAMPA 5, field della classe Ospite
		}
	}
}
```