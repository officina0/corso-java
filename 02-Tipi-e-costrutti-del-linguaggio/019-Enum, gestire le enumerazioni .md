A partire dalla versione 5, in Java è stato introdotto uno speciale tipo chiamato **Enum o Enumerated Type** che, almeno in prima approssimazione, può essere semplicemente pensato come un modo per vincolare una variabile a poter assumere solo un determinato (dallo sviluppatore) set di valori.

Se immaginiamo di scrivere un programma che gestisca un calendario probabilmente avremo bisogno di un modo per identificare i giorni della settimana; nulla vieta di definire, ad esempio, la variabile giornoDellaSettimana come int (byte basterebbe ma siamo a fare un esempio) e tenere a mente che 0 corrisponde a Lunedì, 1 a Martedì e così via.

Così facendo `giornoDellaSettimana` potrà certamente avere tutti i valori desiderati:

```
int giornoDellaSettimana = 4; // VEN per la nostra convenzione
```

e con quache costante (static final) potremmo anche rendere leggibile il codice:

```
public static final int LUN = 0;
// ...
public static final int VEN = 4;
int giornoDellaSettimana = VEN;
```

ma purtroppo, per il compilatore, `giornoDellaSettimana` è e resta una variabile `int` e quindi non potrà mai verificare che non ci sia mai nel nostro codice una linea in cui viene assegnato il valore `9` (o `-4` essendo gli interi signed), facendoci perdere una delle più preziose comodità di un linguaggio staticamente tipato come Java: il **type checking** al momento della compilazione.

D’altro canto, se invece di cercare di usare (male) un `int`, oppure un altro qualsiasi tipo non pensato per rappresentare un giorno della settimana, definiamo:

```
public enum Giorno {
	LUNEDI,
	MARTEDI,
	MERCOLEDI,
	GIOVEDI,
	VENERDI,
	SABATO,
	DOMENICA // opzionalmente può terminare con ";"
}
```

e poi

```
Giorno giornoDellaSettimana;
```

avremo una variabile che potrà contenere _solamente_ un valore appartenente al set specificato nella definizione dell’enum `Giorno` e contemporaneamente avremo a disposizione anche i nomi simbolici (le costanti di prima) da usare nella scrittura del programma:

```
giornoDellaSettimana = Giorno.VENERDI;
```

Ricordando la lezione sui costrutti condizionali, è interessante poter usare enumerazioni come quelle definite dal tipo `Giorno` anche con lo statement **switch**.

```
/** EnumTest.java*/
public class EnumTest {
	public enum Giorno { LUNEDI, MARTEDI, MERCOLEDI, GIOVEDI, VENERDI, SABATO, DOMENICA };
	public static void main(String[] args) {
		// scegliamo un valore
		Giorno giornoDellaSettimana = Giorno.GIOVEDI;
		// definiamo una logica
		switch(giornoDellaSettimana){
			case LUNEDI:
				System.out.println("Oggi è Lunedì");
				break;
			case MARTEDI:
				System.out.println("Oggi è Martedì");
				break;
			case MERCOLEDI:
				System.out.println("Oggi è Mercoledì");
				break;
			case GIOVEDI:
				System.out.println("Oggi è Giovedì");
				break;
			case VENERDI:
				System.out.println("Oggi è Venerdì");
				break;
			case SABATO:
				System.out.println("Oggi è Sabato");
				break;
			case DOMENICA:
				System.out.println("Oggi è Domenica");
				break;
		}
	}
}
```

Il risultato sarà:

```
Oggi è Giovedì
```

Le caratteristiche della classe enum
------------------------------------

Tecnicamente parlando in Java una `enum` è una classe come le altre ma che “implicitamente” (cioè senza che lo scriviamo noi) estende sempre la classe `java.lang.Enum`, cosa che ha l’unico inconveniente di rendere impossibile di scrivere enum che derivino da altri tipi.

Il trattamento speciale che Java riserva agli enum riserva anche qualche interessante sorpresa: il compilatore per ogni classe enum sintetizza per noi un metodo statico (**values**) che ritorna un array di tutti i possibili valori che potranno assumere le variabili cha varanno come tipo l’enum, quindi nel nostro esempio il frammento di codice:

```
for( Giorno d : Giorno.values() ) {
	System.err.println(d);
}
```

stamperebbe tutti i nomi dei giorni della settimana (anche i nomi vengono convertiti automaticamente in stringhe, con il medesimo case).

Analogamente il compilatore ci offre la possibilità di **convertire stringhe in valori** del nostro enum:

```
Giorno g = Giorno.valueOf("SABATO");
```

assegnerà alla variabile `g` il valore `Giorno.SABATO` (attenzione, la stinga deve avere il medesimo case e non può contenere spazi, infatti l’esecuzione (non la compilazione) di:

```
Giorno g = Giorno.valueOf("Sabato");
```

genererebbe un errore di tipo “java.lang.IllegalArgumentException”.

In generale infine, il fatto che le `enum` siano a tutti gli effetti classi, apre la possibilità di aggiungere dentro di essi metodi e field e, e questo è più sorprendente, anche costruttori.

Esaminimo un pò di sintassi. Se la definizione dell’enum contiene metodi e/o field:

*   la lista dei field deve essere terminata da ‘`;`‘ (che altrimenti è ammesso ma non obbligatorio).
*   Gli eventuali costruttori devono essere privati (o package private) e non possono essere chiamati esplicitamente, sono unicamente a disposizione del compilatore.
*   La lista dei valori ammissibili nel caso in cui siano definiti dei costruttori non deve essere considerata, come abbiamo fatto fino ad ora, come una lista di etichette ma come una forma compatta per istruire il compilatore a costruire determinate istanze della classe ed assegnare loro un nome simbolico.
*   Non ci sono restrizioni circa i metodi e i field che possono essere inclusi nel corpo di un enum.

Enumerazioni: creare oggetti specifici
--------------------------------------

Oltre alla sintassi, che tutto sommato non presenta particolari aspetti interessanti, circa la versatilità delle enum vale la pena di provare ad immaginarne un contesto d’uso.

Immaginiamo di dover gestire in un programa gli elementi chimici (fare riferimento ad esempio alla [tavola periodica degli elementi](http://it.wikipedia.org/wiki/Tavola_periodica_degli_elementi)) potremmo probabilmente costruire un enum (grosso, sono oltre 100) che li contenga tutti.

Poiché il numero di elementi è finito e prefissato, un enum potrebbe essere una scelta opportuna ma ci dovremmo di sicuro preoccupare di associare ad ogni elemento alcune informazioni aggiuntive, diciamo numero atomico e massa atomica per fissare le idee, e predisporre opportuni metodi per accederli, l’enum dovrebbe quindi contenere qualcosa del tipo:

```
private int numeroAtomico;
private double massaAtomica;
private String simbolo;
public int getNumeroAtomico() {
	return numeroAtomico;
}
// ... altri getter/setter
```

e un costruttore per permettere (costringere sarebbe il verbo giusto) l’inizializzazione dei field, quindi:

```
private Elemento(String simbolo, int numeroAtomico, double massaAtomica) {
	this.simbolo = simbolo;
	this.numeroAtomico = numeroAtomico;
	this.massaAtomica = massaAtomica;
}
```

Se a questo punto tentassimo di definire i valori dell’enum come abbiamo fatto per i giorni

```
public enum Elemento {
	IDROGENO,
	ELIO,
	// ... ;
}
```

Otterremmo un errore di compilazione che ci avviserebbe che il costruttore default (quello senza argomenti) della classe `Elementi` non esiste. Infatti il costruttore lo abbiamo definito noi e prevede che vangano specificati alcuni parametri e così siamo obbligati a costruire i nostri valori nell’enum, con il costruttore che abbiamo fornito, la definizione giusta sarà quindi:

```
/** Elemento.java */
public enum Elemento {
		IDROGENO("H", 1, 1.008),
		ELIO("He", 2, 4.003),
		// ... altri elementi
		LITIO("Li", 3, 6.491);
		private int numeroAtomico;
		private double massaAtomica;
		private String simbolo;
		public int getNumeroAtomico() {
			return numeroAtomico;
		}
		public String getSimbolo() {
			return simbolo;
		}
		private Elemento(String simbolo, int numeroAtomico, double massaAtomica) {
			this.simbolo = simbolo;
			this.numeroAtomico = numeroAtomico;
			this.massaAtomica = massaAtomica;
		}
}
```

Copiamo questa definizione in un file `Elemento.java` e la utilizziamo per una prova su strada all’interno di un “main”:

```
/** main.java */
public class main {
	public static void main(String[] args) {
		for( Elemento e : Elemento.values() ) {
			System.out.printf("%s\t|\t%d|\t%s\n", e.getSimbolo(), e.getNumeroAtomico(), e);
		}
	}
}
```

Lanciamo il programma e otteniamo:

```
H	|	1|	IDROGENO
He	|	2|	ELIO
Li	|	3|	LITIO
```

Le variabili di tipo `Elemento` per costruzione, saranno sempre elementi validi e con le proprietà che ci interessano sempre inizializzate e disponibili.

Enumerazioni: sintesi delle caratteristiche
-------------------------------------------

Enum risolve quindi un’esigenza reale, quella di definire un insieme di valori predefiniti, senza ricorrere alla mediazione di costanti intere, con la possibilità di avere una classe. Come abbiamo visto anche le enumerazioni possono essere utilizzate nei cicli for-each (for-in) e nei costrutti `switch`. Il metodo `toString()`, di default, è uguale al nome assegnato alla variabile, ma vedremo come poterlo modificare.

Ecco una breve lista delle caratteristiche delle enumerazioni che ci aiuta a comprenderne la logica per utilizzarle al meglio:

*   Una enumerazione è una classe, in particolare l’estensione della classe _java.lang.Enum_, quindi come tale ha tutte le attenzioni sul controllo dei tipi in fase di compilazione.
*   I tipi definiti in una enumerazione sono istanze di classe, non tipi interi.
*   I valori di una enumerazione sono `public final static`, quindi immutabili.
*   Il metodo `==` è sovrascritto, quindi può essere usato in maniera intercambiabile al metodo equals.
*   Esiste la coppia di metodi `valueOf()/toString()` che possono essere sovrascritti.

Enum, utilizzo avanzato
-----------------------

Possiamo finire approfondendo alcuni aspetti avanzati che possono aiutarci nella produzione di codice. Lo facciamo con un esempio di enumerazione che rappresenti un player di tracce audio prese da un archivio. Vogliamo che il player sappia riconoscere tre formati diversi (mp3, pcm e dolby digital) e che ognuno di essi concretamente sia riprodotto in base alle proprie specifiche.

Creiamo quindi il tipo enum `Audio` e il relativo file (`Audio.java`) e lo definiamo con la stessa sintassi che utilizziamo per una classe (alla fine sempre di una classe si tratta), quindi: variabili di istanza, costruttori e metodi. In particolare definiamo un **metodo astratto** _reproduce_ (questa parte si capirà meglio dopo aver parlato della programmazione orientata agli oggetti).

```
/** Audio.java*/
public enum Audio {
	// Lasciamo lo spazio per gli elementi dell'enumerazione
	private final String channel;
	private final int bitrate;
	Audio(String channel,int bitrate) {
		this.channel=channel;
		this.bitrate=bitrate;
	}
	Audio(String channel) {
		this.channel=channel;
		bitrate = -1;
	}
	public abstract String reproduce(String archive);
	// getter e setter
	public String getChannel() {
		return channel;
	}
	public int getBitrate() {
		return bitrate;
	}
}
```

Per realizzare quanto detto sopra, ogni tipo concreto creato (ogni definizione) dovrà implentare concretamente il metodo _reproduce_ ed eventualmente potrà definire una sua propria logica nel suo corpo di classe:

```
MP3("mp3",128) {
		@Override
		public String reproduce(String archive) {
			return archive+" > file MP3";
		}
	},
	PCM("PCM"){
		@Override
		public String reproduce(String archive) {
			return archive+" > file PCM";
		}
	},
	DD("Dolby Digital",256){
		@Override
		public String reproduce(String archive){
			return archive+" > file Dolby Digital";
		}
		@Override
		public String toString(){
			//Override del metodo toString()
			return "Dolby";
		}
	};
	// qui seguono le definizioni della classe
	private final String channel;
	private final int bitrate;
	Audio(String channel,int bitrate) {
	// Etc...
```

Gli elementi, così definiti, li inseriamo nella prima parte della dichiarazione dell’enum.

Esaminando gli elementi vediamo che abbiamo creato ogni singolo tipo concreto sfruttando i costruttori (l’uno o l’altro, badate bene), sempre però identificando il tipo con un identificativo (MP3, PCM, DD). Ognuno dei metodi implementa concretamente il player scelto (qui simulato con il ritorno di una stringa particolare), inoltre l’ultimo player effettua l’override del metodo `toString()`.

Vediamo un main di esempio e l’utilizzo dell’enumerazione appena creata:

```
public class main {
	public static void main(final String[] args) {
		for( Audio a : Audio.values() ) {
			System.out.printf("%s\t| %d\t| %s\t | %s\n", a, a.getBitrate(), a.getChannel(), a.reproduce("myFile"));
		}
	}
}
```

Ecco il risultato:

```
MP3   | 128 | mp3           | myFile > file MP3
PCM   | -1  | PCM           | myFile > file PCM
Dolby | 256 | Dolby Digital | myFile > file Dolby Digital
```

Qui c’è da notare che, per default, il `toString()` del tipo enum è l’identificativo assegnato (`MP3`, `PCM`) mentre, laddove abbiamo effettuato l’override, ovviamente, è il valore che noi abbiamo assegnato (`Dolby` e non `DD`).