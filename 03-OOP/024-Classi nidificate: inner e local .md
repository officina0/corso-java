In Java vige la regola generale che ogni classe (ed interfaccia) deve essere definita in un file che deve avere il medesimo nome della classe. Inoltre per ottenere istanze di oggetti, deve essere usata la sintassi:

TypeNameA  var = new TypeName(...);

dove `TypeName` può essere solamente il nome di una classe (non astratta) mentre `TypeNameA` (il tipo della variabile) deve essere il nome di una delle classi da cui TypeName deriva (quindi la super-classe, la super-classe della super-classe e così via, detti ancestors) oppure il nome di una delle interfacce implementate da TypeName (oppure un ancestor di queste).

In realtà esistono tre importanti eccezioni a questa regola, chiamate:

1.  classi ed interfacce **inner**;
2.  classi **local** o locali;
3.  classi **anonymous** (spesso chiamate anche anonime), classi ed interfacce cioè che possono essere sia definite nel corpo di un’altra classe e che addirittura possono essere definite ed istanziate (immediatamente dopo, o forse sarebbe giusto dire contemporaneamente alla loro definizione), all’interno di classi e metodi.

Oltre a consentire uno stile di programmazione più conciso le classi inner, locali e anonime hanno anche alcune interessanti caratteristiche circa la visibilità dei metodi e dei fields della classe “ospite” alle quali vale la pena di fare attenzione.

In questo articolo ci occuperemo in particolare di classi inner e local e dedicheremo alle classi anonime un articolo a parte.

Classi Inner
------------

Una **classe inner** in Java è una classe definita in modo analogo ad ogni altra classe ma la cui definizione non si trova in un file separato ma viene riportata all’interno di un’altra classe (chiamata solitamente ‘_enclosing_‘ o, raramente per la verità, “ospite”).

Una classe inner in Java può essere definita ovunque nel corpo di una classe ma all’esterno del corpo dei metodi. Se la definizione si trova all’interno di un metodo, più precisamente sarebbe giusto dire di un **blocco** (codice racchiuso tra parentesi graffe) la classe si dice locale.

class Enclosing  {
   
	// ...
	
	class Inner {
		//...
	}
   
	//...
   
	static class InnerStatic {
		//...
	} 

	interface InnerInterface {
		//...
	}
	
	// ...
	
	public void metod(...) {
		
		//...
		
		class LocalClass {
			// ...
		}
		// ...
	}
} 

La sintassi per la creazione di classi inner (ed interfaccie inner, come si vede dall’esempio) è del tutto identica a quella per la definizione di una classe “regolare” tranne per il fatto che per le classi è possibile specificare la keyword static con una peculiare funzionalità.

Se per una classe inner è specificata la keyword `static` la inner class può a tutti gli effetti essere considerata equivalente ad una classe regolare che solo per motivi stilistici o di comodità è stata definita all’interno di un’altra classe.

Se invece nella definizione di una classe inner non compare la keyword static questa deve essere considerata “parte” della classe enclosing ed ha quindi accesso diretto ai field ed ai metodi dell’ospite. Una inner class non può definire membri statici essendo essa non statica tranne che nel caso che si tratti di costanti (i.e. contemporaneamente `final` che `static`).

Le interfacce sono per definizione statiche quindi la keyword `static` per esse è implicita.

Classi local
------------

Le classi locali sono la variante delle classi inner definite direttamente all’interno di blocchi di codice, per esse non è prevista la possibilità di essere statiche e devono essere considerate **locali al blocco in cui sono definite**. Per l’assenza della possibilità di essere statiche non è possibile definite interfacce locali, essendo come detto poco sopra le interfacce sempre statiche.

Le classi locali sono simili alle classi inner non statiche ed hanno accesso alle variabili definite nel blocco che le contiene ma solo a quelle definite ‘final’.

Java8 ha introdotto un nuovo concetto di variabile detto **effectively final** (effettivamente final) che consiste nel rendere automaticamente ‘final’ tutte le variabili (ed i parametri dei metodi) che non cambiano mai il loro valore dopo l’assegnamento iniziale. Anche le variabili (ed i parametri) effectively final sono accessibili dalle classi locali dalla versione 8 di Java.

Per fare riferimento ad una classe inner si deve usare il nome della classe ospitante seguito da un punto e dal nome della classe:

Enclising.Inner
Encolsing.InnerInterface
Enclosing.InnerStatic

ma mentre per le static inner classes è possibile utilizzare la comune sintassi dell’operatore new:

Enclosing.InnerStatic  v = new Enclosing.InnerStatic(...);

per istanziare una classe inner non statica deve essere prima ottenuta una istanza della classe enclosing, con una sintassi piuttosto sorprendente:

Enclosing enclosing = new Enclosing(...);
Enclosing.Inner  v = enclosing.new Inner(...);