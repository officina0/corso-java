Un programma (o un sistema) realizzato secondo l’approccio Object Oriented, semplice o complesso che sia, basa il suo funzionamento sull’interazione tra oggetti: entità che possono comunicare e modificare il proprio stato.

Per questo è fondamentale approfondire i concetti di _classe_, _oggetto_ e _costruttori_, cosa che facciamo in questa lezione, anche introducendo la rappresentazione UML, ovvero il liguaggio visuale nato proprio per modellare sistemi orientati agli oggetti.

Gli oggetti
-----------

Nella vita reale siamo abituati a classificare e a vedere oggetti dalle caratteristiche tangibili e riconoscibili, nel mondo OO invece, il concetto di oggetto si amplia e un oggetto può contenere elementi concreti, ma anche entità come processi (pensiamo a quelli in una filiera manifatturiera) o concetti teorici e astratti (un modello 3D nella modellazione solida, o cose intangibili come folla, etc.).

Gli oggetti di cui stiamo parlando non hanno caratteristiche fisicamente distinguibili ma sono identificati dall’avere:

*   **stato**, l’insieme delle variabili interne che ne definiscono le caratteristiche in un certo istante dell’esecuzione;
*   “**comportamento**“, le modalità di azione o di reazione di un oggetto, in termini di come il suo stato cambia e come i messaggi passano.

### Interazione tra oggetti

In OOP i problemi si affrontano concentrandosi sulle interazioni, perciò la spedizione di messaggi risulta essere basilare per rappresentare l’evoluzione di un sistema. È utile quindi che sia ben compreso e descrivibile in maniera chiara attraverso gli stumenti di modellazione.

L’interazione tra diversi oggetti avviene attraverso lo **scambio di messaggi**: un oggetto, detto _sender_ del messaggio agisce su un altro oggetto, detto _recipient_, spedendogli uno dei messaggi che il ricevente è in grado di accettare.

In altre parole: _l’oggetto sender chiama uno dei metodi “esposti” dall’oggetto ricevente_.

In Java intendiamo per “oggetto” l’istanza particolare di una certa classe, e esso può possedere (o esporre) alcuni metodi. Quindi un oggetto può ricevere un certo messaggio se possiede un metodo che l’oggetto sender è in grado di chiamare (con la opportuna visibilità).

In UML lo scambio di messaggi è rappresentato con un diagramma (un semplice schema) chiamato sequence simile a quello riportato in figura:

![Sequence Diagram](https://tbm-html.s3.amazonaws.com/app/uploads/2014/11/java21_01.png)

che va letta come: l’oggetto `Sender` spedisce il messaggio corrispondente al metodo `methodXYZ` all’oggetto `Recipient`, il rettangolo vicino alla freccia (sotto `Recipient`) è comunemente detto _activation_ e rappresenta l’elaborazione necessaria a `Recipient` per operare sul suo stato e poter reagire al messaggio ricevuto.

È naturalmente possibile che, come reazione al messaggio `methodXYZ` (ad esempio al fine di reperire informazioni per una eventuale risposta al messaggio ricevuto) `Recipient` operi su altri oggetti generando una cascata di messaggi verso altre entità del sistema (`OtherObj` nello schema d’esempio).

Classi
------

Per ogni oggetto è necessario sapere qual è il set dei messaggi che è in grado di ricevere. Non possiamo infatti inviare messaggi ad un oggetto se essi non sono ammessi nell’insieme delle azioni previste.

_Una classe è esattamente il “blueprint” (il prototipo) di un oggetto_ in cui vengono definiti tutti i messaggi che ciascuna istanza sarà in grado di ricevere. Nella rappresentazione di una classe sarà quindi importante evidenziare:

*   l’insieme dei **metodi** che la classe supporta (messaggi ricevibili)
*   l’insieme delle variabili di stato (**attributi**) che ne rappresentano lo stato.

Grazie all’UML possiamo utilizzare il **Class Diagram** come notazione e disegnare una classe in modo simile allo schema che segue:

![Sequence Diagram](https://tbm-html.s3.amazonaws.com/app/uploads/2014/11/java21_02.png)

Lo schema mostra sia la notazione per la rappresentazione di una classe (con riportati attributi e metodi) sia quella per rappresentare una istanza. Nel secondo caso i medodi non hanno ragione di essere riportati (sono disponibili per il fatto che `aa` è istanza di `ClasseXYZ`). Sono riportati invece i valori degli attributi che sono significativi per ogni singola istanza (mentre il loro valore non avrebbe senso, tranne che per l’inizializzazione, se associato alla definizione di classe).

### Aggiungere dettagli alla rappresentazione

Come l’analisi di ogni problema deve essere affrontata in maniera iterativa, allo stesso modo la rappresentazione UML delle entità di un sistema va immaginato come un processo dinamico che non pretende da subito di catturare ogni dettaglio ma procede da idee generali verso quelle più specifiche e dettagliate.

Perciò gli schemi sopra riportati vanno condiderati solo di primo livello e potranno essere dettagliati aggiungendo per ogni attributo il relativo tipo e valore iniziale:

```
typeAttribute: data_type = initial_valueClass
```

mentre per ogni messaggio (metodo) si dovrà procedere a dettagliare la firma (signature):

```
Metod ( arg: type, …, arg: type ) : return_type
```

Analogamente sia per gli attributi che per i metodi andrà specificata in una fase più avanzata di analisi la **visibilità** che, nella notazione UML, consiste nell’anteporre al nome dei metodi e degli attributi:

Segno

Tipo di visibilità

**+**

public

**–**

private

**#**

protected

**~**

Java’s package visibility

mentre si usa sottolineare (o prefissare con `$`) i metodi e gli attirbuti _static_.

### Un esempio

La ragione di cercare di rappresentare in UML ogni aspetto delle classi (e delle loro relazioni e dinamiche, come avremo modo di vedere più avanti) deriva da un lato dal fatto che per avere una buona manutenibilità di un codice occorre che ogni sua parte sia documentata ed anche i dettagli siano quanto più possibile definiti prima che il codice venga scritto ma anche dal fatto che con gli opportuni [strumenti](http://it.wikipedia.org/wiki/Computer-aided_software_engineering) è possibile che l’implementazione venda generata automaticamente a partire dalla descrizone formale salvando tempo di sviluppo a favore di quello di analisi e design.

A titolo di esempio riprendiamo schema della classe `Conto`, esaminato nella lezione introduttiva sulla classi in Java ed il cui codice riportiamo anche di seguito.

```
/**
* Classe per rappresentare un Conto
*/
public class Conto { 
	private double amount;
	private String owner; 
	// costruttore
	public Conto(String owner, double initialAmount) {
		this.owner = owner;
		this.amount = initialAmount;
	} 
	public void versamento(double qty) {
		amount += qty;
	} 
	public boolean prelievo(double qty) {
		if(amount < qty)
			return false;
		amount -= qty;
		return true;
	}
	public double getAmount() {
		return amount;
	} 
	public String getOwner() {
		return owner;
	}
}
```

![Sequence Diagram](https://tbm-html.s3.amazonaws.com/app/uploads/2014/11/java21_03.png)

Nel quale è mostrato anche come sia possibile in UML qualificare elementi per mezzo di quelli che sono comunemente chiamati **strereotype** `(<< ... >>`) e che servono per aggiungere significati alle entità non descritti direttamente in UML e rappresentano un naturale sistema di estenzione per l’UML stesso.

Costruttori
-----------

Il **costruttore** è una delle componenti di una classe che merita una trattazione accurata. Partiamo da queste due eminenti definizioni:

*   “Una classe è una collezione di uno o più oggetti contenenti un insieme uniforme di attributi e servizi, insieme ad una descrizione circa come creare nuovi elementi della classe stessa” _(E. Yourdan)_
*   “An object has state, behavior, and identity” _(G. Booch)_

Il costruttore è quel metodo di una classe il cui compito è proprio quello di **creare nuove istanze**, oltre ad essere il punto del programma in cui un nuovo elemento (quindi una nuova identità) viene creato ed è reso disponibile per l’interazione con il resto del sistema.

Come nell’esempio `Conto` sopra riportato il costruttore è spesso usato per effettuare le inizializzazioni dello stato delle nuove istanze. In Java possono esserci molteplici costruttori per una medesima classe (ognuno con parametri di diversi) e ne esiste sempre almeno uno. Se infatti per una data classe non viene specificato alcun costruttore, il compilatore ne sintetizza automaticamente uno senza argomenti, detto _costruttore default._