# 001 Programmare In Java Introduzione

Proprio a cavallo tra la fine degli anni ’80 ed i primi anni ’90, quando il [Green Team](001-programmare-in-java-introduzione.md) era al lavoro per la realizzazione del “Java programming language”, era in grande evoluzione, se non dominante, l’approccio alla programmazione detto “Object Oriented” e Java nasce come il linguaggio ‘OO’ di grande diffusione per eccellenza.

Senza entrare troppo nei dettagli, alcuni dei quali potranno risultare evidenti nel corso della guida, la **metodologia orientata agli oggetti** promette di rendere possibile una migliore rappresentazione all’interno dei nostri programmi delle entità del mondo reale in modo da rendere il codice più facilmente mantenibile ed estendibile.

## Da strutturale/procedurale a OOP

Ad esempio, se immaginiamo di dover scrivere un programma per gestire conti bancari dovremo provvedere, per ciascuno dei conti gestiti, a prenderci carico di rappresentare lo stato \(un nome, `owner` ed un importo, `amount`, per semplicità\) e scrivere alcune procedure per modificarlo \(funzioni, subroutine\), oltre ad una probabilmente per crearlo. Pensiamo ad operazioni come `versamento` e `prelievo` alle quali passare ogni volta tra gli argomenti la struttura `conto` su cui operare.

Con l’**approccio OO** nel medesimo programma potremo invece pensare di avere un tipo di oggetto, ad esempio chiamato _Conto_ che contenga insieme:

* lo **stato** \(che chiameremo _fields_, i campi dell’oggetto\);
* le **procedure** per la sua gestione \(che chiameremo _methods_, metodi\).

In questo modo possiamo modellare nella medesima sezione del codice \(che chiameremo **classe**\), sia lo stato che il “behavior”, il comportamento delle entità che utilizziamo.

L’integrazione dello stato e del behavior è comunemente chiamata _encapsulation_ \(**incapsulamento**\) ed è la base di quella caratteristica della programmazione OO che viene chiamata **information hiding** che consiste nel mantenere interno a precise sezioni del codice l’accesso allo stato delle entità utilizzate.

È interessante osservare, che OOP è una metodologia ed uno stile di progettazione del software che, tranne in casi particolari, può essere impiegato indipendentemente dal linguaggio ma che risulta estremamente più facile da adottare se il linguaggio \(come Java, C++ e altri\) ne fornisce le primitive \(come le classi in Java\).

Per citare un esempio, il codice di X11, l’ambiente grafico di Unix, ha sempre avuto un rigido approccio OO ma scritto in C con l’uso di puntatori a funzione per “emulare” i metodi e dichiarazioni di strutture non pubbliche per ’emularè l’information hiding.

## Classi e oggetti in Java

Nell’approccio OO in Java non solo si definiscono gli oggetti su cui si intende lavorare ma li si organizzano in categorie: una classe è esattamente la definizione delle proprietà e dei metodi che avrà ogni elemento della categoria.

La distinzione tra oggetto e classe è enfatizzata dalle seguenti definizioni:

> **Classe** è una collezione di uno o più oggetti contenenti un insieme uniforme di attributi e servizi, insieme ad una descrizione circa come creare nuovi elementi della classe stessa \(Edward Yourdan\);
>
> Un **oggetto** è dotato di stato, behavior ed identità; la struttura ed il comportamento di oggetti simili sono definiti nelle loro classi comuni; i termini istanza ed oggetto sono intercambiabili \(Grady Booch\).

### Definire una classe in Java:

Una classe in java si definisce con la keyword `class`:

/\*\* \* Classe per rappresentare un Conto \*/ public class Conto { private double amount; private String owner;

```text
// costruttore
public Conto(String owner, double initialAmount) {
    this.owner = owner;
    this.amount = initialAmount;
}

public  void  versamento(double qty) {
    amount += qty;
}

public boolean prelievo(double qty) {
    if(amount < qty)
        return false;
    amount -= qty;
    return true;
}

/\* Getters */
public double getAmount() {
    return amount;
}
public String getOwner() {
    return owner;
}
```

}

dove si può osservare che:

* il corpo \(dichiarazioni di variabili e metodi\) della classe è racchiuso tra _parentesi graffe_;
* le linee sono terminate da punto-e-virgola \(`;`\);
* le linee che iniziano per doppio slash \(`//`\) sono **commenti** così come tutto quello che è racchiuso tra ‘`/*`‘ e ‘`*/`‘;
* il _doppio asterisco_ all’inizio di un commento \(`/**`\) ha un significato speciale in relazione al comando `javadoc` che sarà introdotto in seguito e serve per la generazione di documentazione a partire dai commenti nel codice.

### Qualificatori di visibilità

I qualificatori di fronte alle dichiarazioni di tipi o metodi \(`public` e `private`\) servono per determinare la visibilità degli elementi cui sono applicati:

Qualificatore

Descrizione

**private**

Significa che sarà visibile \(accessibile se si tratta di un field, chiamabile se si tratta di un metodo\) solo all’interno della classe che lo contiene.

**public**

significa invece che sarà accessibile anche dall’esterno della classe stessa.

Esistono altri 2 possibilità circa la visibilità:

Qualificatore

Descrizione

**protected**

accessibile solo dalle classi derivate, ne capiremo il significato in seguito

_default_

Non è una keyword: si ha la visibilità di default se nessuna delle precedenti viene specificata e implica visibilità per tutte le classi che si trovano all’interno del medesimo package \(qualche dettaglio più avanti\).

### Costruttore

Di particolare rilevanza è il metodo contrassegnato con il commento `// costruttore`: è diverso dagli altri metodi \(`versamento` e `prelievo`\) in quanto non dichiara un valore di ritorno e serve \(come richiesto dalla definizione di classe di Yourdan\) per creare nuove istanze di elementi di quella classe. Il costruttore delle classi deve essere utilizzato con la keyword **new**, la scrittura:

Conto myConto = new Conto\("Francesca", 1.0e8\);

Significa che vogliamo costruire \(allocare in memoria\) una nuova istanza di un oggetto della categoria `Conto` ed inizializzarlo con `owner` “Francesca” e `amount` 1 miliardo.

Qualora non avessimo specificato nessun costruttore nella definizione della classe ne sarebbe stato creato uno di default senza argomenti. Si possono aggiungere quanti costruttori si vogliono a patto che abbiano tutti diversa _signature_ \(firma, ovvero tipo e numero degli argomenti\). I costruttori devono avere nome uguale al nome della classe.

### this

All’interno del costruttore si può osservare anche l’uso della keyword **this** che in ciascuna classe rappresenta sempre l’istanza corrente.

Si osservi come nel costruttore si usi `this` nella prima riga per disambiguare la variabile `owner`: senza `this` l’assegnazione non avrebbe effetti \(ed il compilatore la segnalarebbe anche con uno warning\) in quanto significherebbe che si assegna il contenuto della variabile owner ad owner stessa, mentre con la qualificazione della variabile significa che stiamo assegnando il contenuto della variabile owner, che è stata passata come argomento al metodo, al field owner della classe `Conto` \(o meglio dell’istanza corrente dell’elemento della classe Conto che stiamo costruendo\).

### Notazione ‘.’

Una volta costruito l’oggetto conto sarà possibile utilizzare i suoi metodi con la notazione:

conto.versamento\(10\); conto.prelievo\(100\);

L’accesso ai field sarebbe possibile con la medesima notation \(e.g. `conto.amount = 5`\) ma è proibito per scelta della nostra classe che definisce i field come `private` e quindi il compilatore ce lo segnalerebbe come errore.

## Ereditarietà: estendere una classe

L’utilità della strutturazione OO diventa palese, a mio parere, se si pensa a questo punto che potrebbe nascere la necessità di estendere l’immaginario programma per la gestione di conti con una banalizzazione di un conto in cui alcune somme vengano vincolate \(e.g. nel tempo: sono nel conto ma disponibili solo dopo una certa data\); in tal caso potremmo “estendere” direttamente la classe Conto:

public class ContoVincolato extends Conto {

```text
private double importoVincolato;
private Date fineVincolo;

public ContoVincolato(String  o, double a) {
    super(o,a);
    importoVincolato=0;
}

public boolean vincola(double qty, Date fineVincolo) {
    if(importoVincolato != 0 || getAmount() < qty) 
        return false;
    else {
        prelievo(qty);
        importoVincolato = qty;
        this.fineVincolo = fineVincolo; 
        return true;
    }
}

/\*\* 
 \* svincola l'importo vincolato se è passata la data di vincolo
 */
public boolean svincola() {
    if(importoVincolato > 0) {
        Date adesso =  new Date();
        if( adesso.after(fineVincolo) ) {
            this.versamento(importoVincolato);
            this.importoVincolato = 0;
            return true;
        }
    }
    return false;
}

//overloaded getAmount
public double getAmount(Date quando) {

    if(importoVincolato == 0 || quando.before(fineVincolo)) 
        return getAmount();

    return getAmount() + importoVincolato;
}

public double getAmount() {
    svincola();
    return super.getAmount();
}
```

}

Questo codice serve a mostrare come estendere \(cioè aggiungere funzionalità\) alla classe `Conto` senza doverne riscrivere il codice. In questo caso si è aggiunta la funzionalità di vincolare un importo fino ad una certa data e inserito un metodo che renderà di nuovo disponibile l’importo vincolato se chiamato dopo la data di scadenza del vincolo.

In casi come questi si dice che la classe `ContoVincolato` **deriva** da `Conto` da cui **eredita** i field `conto` ed `owner` ed i metodi `versamento` e `prelievo` \(che infatti sono usati all’interno di `ContoVincolato`\).

La keyword **super** usata nel costruttore di `ContoVincolato` serve per riferirsi alla classe _padre_ i.e. quella da cui si deriva. In casi come questo nei quali la classe padre ha un costruttore con parametri la classe `figlia` \(derivata\) ha l’obbligo di chiamare esplicitamente il costruttore del padre che invece è implicitamente chiamato nel caso che sia presente un costruttore vuoto \(senza argomenti\).

Il metodo `getAmount(Date arg)` è invece un esempio di metodo ‘**overloaded**‘ cioè un metodo definito più volte \(in questo caso una volta nelle classe padre ed una volta nella classe derivata, ma non è un obbligo\), ma con argomenti diversi; il compilatore capirà quale chiamare sulla base degli argomenti che gli vengono passati.

Infine il metodo `getAmount()` è un esempio di **ridefinizione di un metodo** da parte della classe derivata; quando chiamato su una istanza di un oggetto di tipo `ContoVincolato` il metodo `getAmount` non eseguirà quello definito in `Conto` ma prima chiamerà la funzione svincola e solo dopo chiamerà la `getAmount` di `Conto`; si noti l’uso della keyword `super` in questo caso necessaria per evitare un loop infinito di chiamate ricorsive.

## Package

Un ultimo concetto interessante in Java per quanto riguarda le classi è quello dei **package**

Ogni classe appartiene ad un package \(dichiarato all’inizio del file in cui è definita una classe con la keyword `package` appunto\) e serve a definire a qualche gruppo di classi essa appartiene.

Un package è del tutto simile ad un ‘path’ in un filesystem solo che al posto degli slash \(`/`\) le componenti sono separate da punti \(`.`\). Nelle prossime lezioni ci sarà occasione di approfondire questo ed altri concetti legati alla struttura delle classi in Java ed alla programmazione OO in generale.

Approfondimenti

* [Guida programmazione orientata agli oggetti](http://www.html.it/guide/guida-programmazione-orientata-agli-oggetti/)
* [Java 101: Object-oriented language basics, Part 1: Classes and objects](http://www.javaworld.com/article/2075202/core-java/object-oriented-language-basics-part-1.html)
* [Object Oriented Programming Fundamentals](http://www.jamesbooth.com/OOPBasics.htm)
* [Object-Oriented Programming Concepts](http://docs.oracle.com/javase/tutorial/java/concepts/index.html)

