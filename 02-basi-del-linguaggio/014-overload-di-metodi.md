# 014 Overload Di Metodi

Come abbiamo detto parlando di **signature** dei metodi, tra le caratteristiche note di Java possiamo citare quella dell’**overloading di metodi**, che ci consente di “sovraccaricare” un metodo di una classe con diverse varianti, in base ai parametri passati come riferimento. Dal punto di vista dell’utilizzo della classe ciò ci consente di definire in maniera dinamica il metodo che meglio si adatta alla nostra esigenza.

Poniamo come esempio la classe _Prodotto_ che gestisce un prodotto e le relative caratteristiche. Un prodotto può contenere diverse note allegate, una serie di caratteristiche che lo descrivono. Il codice di una semplice classe con relativo costruttore sarebbe qualcosa del genere.

/\*\* \* Esempio di una classe con un costruttore \*/ package it.html;

public class Prodotto { private int id; // ...

```text
public Prodotto(int id, String desc)
{
    // ...
}

public Prodotto(int id, String desc1, String desc2)
{
    // ...
}

public Prodotto(int id, String desc1, String desc2, String desc3)
{
    // ...
}
```

}

In questo caso l’overload dei metodi ci permette di avere una serie di costruttori in base a quante informazioni vogliamo passare. Se pensiamo di proseguire con l’introduzione di nuovi parametri intuiamo la limitazione di dover inserire un numero crescente di dichiarazioni.

## Varargs

A partire dalla versione 1.5 di Java 1.5, abbiamo però il cosiddetto **varargs** \(Variable Arguments\), un meccanismo per definire un numero di argomenti indefinito.

Utilizzando il modificatore **…** \(puntini sospensivi o _ellipsis_\) è possibile definire la presenza di un numero variabile di argomenti. Vediamo subito la cosa applicata al nostro caso:

/\*\* \* Esempio di utilizzo di varargs \*/ package it.html;

public class Prodotto { private int id; // ...

```text
public Prodotto(int id, String... desc)
{
    // ...
}
```

}

Il metodo si aspetta `0` o `N` parametri di tipo `String` da utilizzare in dipendenza della logica del metodo \(o del costruttore, come in questo caso\). Dal punto di vista della macchina virtuale è come definire un metodo che si aspetta una array di `String`:

public Prodotto\(int id, String\[\] desc\) { // ... }

È molto importante ricordare questo per capire il funzionamento e utilizzare i parametri all’interno del metodo. A questo punto ci si potrebbe chiede a che giova utilizzare i varargs: il vantaggio sta nel poter passare parametri ai medodi in modo più sintetico:

// chiamata con varargs metodoX\(1,"uno","due","tre"\);

// chiamata senza varargs metodoX\(1,new String\["uno","due","tre"\]\);

Il codice risulta evidentemente più elegante, nonché chiaro, nel primo esempio, mentre del secondo non capiamo bene il perché sia necessaria una presenza di un array. Ecco un semplice programma per testare il funzionamento del costrutto:

/\*\* \* Test del varargs \*/

public class VarargsTest {

```text
public void testVarargs(String... list) {

    System.out.println("Metodo 1");

    for (String tmp:list)
    {
        System.out.println("#" + tmp);
    }
}

public static void main(String\[\] args)
{
    System.out.println("Test varargs");
    VarargsTest va=new VarargsTest();

    va.testVarargs("do","re","mi","fa","sol","la","si");
    va.testVarargs("1","2","3","4");
}
```

}

Come si vede dal corpo del metodo utilizziamo il costrutto for-each e di fatto utilizziamo il parametro `list` come un array.

Per verificare che alla fine la struttura dati utilizzata è proprio l’array, inseriamo e mandiamo in esecuzione la seguente riga di codice:

va.testVarargs\(new String\[\]{"do","re","mi","fa","sol","la","si"}\);

Il risultato sarà lo stesso di prima. Unica cosa a cui bisogna fare attenzione è il caso in cui l’overload possa causare ambiguità. Proviamo ad aggiungere il seguente metodo alla classe:

/\*\* \* Test di ambiguità sul varargs \*/ public void testVarargs\(String arg1,String... list\) { // ... }

Il compilatore procederà correttamente ma ci darà un errore al momento di utilizzare il metodo, in quanto si trova nell’impossibilità di segliere il metodo corretto \(non avendo indicazione su quale scegliere\). Infatti, in questo caso, non possiamo sapere se la prima stringa fa parte della lista di parametri o è un parametro a sé stante.

In queste situazioni, utilizzare un array potrebbe essere un modo per risolvere.

