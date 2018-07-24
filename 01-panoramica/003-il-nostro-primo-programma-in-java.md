# 003 Il Nostro Primo Programma In Java

In questa sezione scriveremo il primo programma in Java, “from scratch” e lo compileremo con gli strumenti base messi a disposizione dal JDK. Quindi per seguire questo tutorial è previsto che abbiate installato il compilatore Java ed il JRE, scaricabili da [qui](http://www.oracle.com/technetwork/java/javase/downloads/index.html) la cui installazione è stata descritta nella lezione precedente.

Va premesso che nella vita di uno sviluppatore, non accade praticamente mai di scrivere un programma Java interamente a mano: tutti usiamo sempre un **IDE** \(Eclipse ad esempio, che vedremo più avanti\), che provvede i tool necessari ad una più semplice gestione dei file e alla manutenzione dei progetti.

È comunque molto utile e istruttivo imparare a organizzare tutto “a mano”, passo dopo passo, anche commettendo volutamente i tipici errori in cui si può incorrere la prima volta.

Il primo programma che scriveremo è breve ma già permette di osservare molte cose:

package my.first.project;

public class Primo { public static void main\(String\[\] args\) { System.err.println\("ciao mondo"\); } }

Iniziamo copiando il codice in un file in un qualsiasi editor di testi. Salviamo e chiamiamo l’esempio `PrimoProgramma.java`, poi eseguiamo il compilatore:

javac PrimoProgramma.java

Il risultato, spiacevole, sarà qualcosa di simile a quello seguante:

PrimoProgramma.java:3: error: class Primo is public, should be declared in a file named Primo.java public class Primo { ^ 1 error

Il compilatore ci avverte che abbiamo commesso il primo errore \(ahimè il primo di una lunga serie\).

## I nomi delle classi in Java, convenzioni

In Java infatti ogni classe pubblica \(`public class`\) deve essere contenuta in un file il cui nome sia identico al nome della classe stessa: ad esempio `Classe` dovrà stare nel file `Classe.java` e `Primo` dovrà quindi stare nel file `Primo.java`.

In generale si usa assegnare ad ogni classe il relativo file per convenzione, anche quando non è obbligatorio \(se togliamo la keyword `public` e lasciamo solo `class` possiamo compilare il programmino senza errori\). Si ritiene infatti che questa pratica aiuti ad una buona organizzazione del codice.

Prima di procedere a rinominare il file ed a fare la prova successiva vale la pena di soffermarsi alla scelta del nome della classe \(che noi abbiamo chiamato `Primo`\).

In Java le classi possono avere i nomi più disparati: `primo`, `PRIMO`, `PrImo123`, `primo_`, `_primo` sarebbero stati tutti nomi validi \([documentazione ufficiale](http://docs.oracle.com/javase/specs/jls/se7/html/jls-3.html#jls-3.8)\) ma esiste una convenzione che prevede che **i nomi delle classi** inizino con un carattere maiuscolo, continuino con caratteri minuscoli e, se composti da più parole, siano capitalizzate le prime lettere di tutte le componenti \(la scrittura di parole composte capitalizzando tutte le prime lettere è comunemente detta CamelCase\).

Quindi se volessimo creare una classe che si chiami “prima classe del tutorial” la convenzione \(non la sintassi\) ci indicherebbe di chiamarla `PrimaClasseDelTutorial` \(nei nomi delle classi la sintassi prevede che non si possano usare gli spazi\).

Rinominiamo a questo punto `PrimoProgramma.java` in `Primo.java` e tentiamo ancora la compilazione:

javac Primo.java

Questa volta non otterremo nessun errore e, accanto al file `Primo.java`, troveremo un secondo file chiamato `Primo.class`. Il compilatore `javac` genererà dei files `.class` ogni volta che lo utilizzaremo su dei file `.java`.

Il file `Primo.class` contiene il bytecode del nostro programma java \(non provate a leggerlo in quanto non c’è molto al momento da capirci, è un file binario con un sacco di caratteri incomprensibili per noi\) che possiamo pensare di eseguire, o più precisamente di chiedere alla macchina virtuale java \(JVM\) di eseguire:

java Primo

Qui il nome “Primo” che passiamo come argomento dell’eseguibile `java` non si riferisce al nome del file `.class`, ma è proprio il nome della classe da eseguire.

## Rispettare il namespace

L’esecuzione anche questa volta darà un risultato inatteso, che potrebbe essere un semplice:

Errore: impossibile trovare o caricare la classe principale Primo

oppure un più articolato messaggio di errore:

Exception in thread "main" java.lang.NoClassDefFoundError: Primo \(wrong name: my /first/project/Primo\) at java.lang.ClassLoader.defineClass1\(Native Method\) at java.lang.ClassLoader.defineClass\(ClassLoader.java:792\) at java.security.SecureClassLoader.defineClass\(SecureClassLoader.java:14 2\) ...

In ogni caso l’errore è dovuto al fatto che la JVM cerca la nostra classe nella directory `my/first/project/`; cosa che a una prima osservazione sembrerebbe strana ma che è giustificata dal fatto che nel nostro programma la prima linea dice che la classe Primo appartiene al package “my.first.project”.

Quindi in fase di esecuzione la JVM si aspetta di trovare la classe in un directory che è formata dal nome del package dove i punti \(‘`.`‘\) sono sostituiti da slash \(‘`/`‘\).

Vale la pena di osservare che il risultato inatteso \(errore di esecuzione\) è segnalato dalla JVM sotto forma di una **Exception** \(eccezione\) che, come impareremo nelle prossime lezioni, sarà sempre il modo in cui la JVM ci comunica eventuali errori di run-time \(durante l’esecuzione di un programma\).

L’eccezione in questo caso è semplice da decifrare semplicemente leggendo la prima riga.

La scritta “wrong name: my/first/project/Primo” ci fa infatti capire che l’errore **NoClassDefFoundError** \(sostanzialmente “Non ho trovato la definizione della classe”\)

Creiamo quindi la directory, spostiamoci entrambi i file \(il file `.java` non sarebbe necessario spostarlo ma la sua collocazione naturale è in quella directory\) e riproviamo:

mkdir -p my/first/project mv Primo.\* my/first/project/

e finalmente eseguendo \(va usato il nome completo, fully qualified\):

java my.first.project.Primo

otteniamo:

ciao mondo

## Package e JAR

**I package in Java** sono il modo più naturale di raggruppare le classi in gruppi \(moduli, unità, gruppi, categorie\) in modo che il nostro codice sia più leggibile e possiamo pensare che ogni componente di un package name rappresenti una directory sul filesystem.

Come per i nomi le classi, Java lascia molta libertà allo sviluppatore nella scelta delle componenti di un package name. Tuttavia anche in questo caso una convenzione comunemente utilizzata è quella di utilizzare solo caratteri minuscoli ed eventualmente numeri.

Poiché il compilatore genererà sempre almeno un file `.class` da ogni file `.java` \(da un singolo file `.java` possono essere generati anche più file `.class`\), i programmi compilati in java potrebbero diventare rapidamente scomodi da gestire in quanto composti da intere directory di file.

Per questo motivo insieme al compilatore ed alla JVM viene fornito anche un altri eseguibile il cui nome è **jar** \(_java archiver_\) il cui scopo è esattamente quello di prendere una intera directory di class files e trasformarla in un unico file \(detto java archive\) più facile da maneggiare e gestire. Ad esempio:

jar cf primoprogramma.jar my

creerà il file `primoprogramma.jar` che contiene tutto il nostro albero di directory. In buona sostanza un archivio jar è la versione compressa della directory ed il formato di compression è esattamente lo zip; per “spacchettare” un jar si può addirittura usare qualsiasi programma in grado di decomprimere gli zip; anche per crearlo potreste farlo ma in tal caso dovreste creare a mano il file MANIFEST.MF che trovate nella directory META-INF dell”archivio e che jar ha provveduto automaticamente a creare.

Per controllare il contenuto del file .jar possiamo eseguire:

jar tvf primoprogramma.jar

## Classpath e jar

Naturalmente quando vorremo utilizzare le classi compilate nel jar non avremo bisogno di decompimerlo ma potremo chiedere direttamente alla JVM di utilizzare come ‘classpath’ il file jar:

java -cp primoprogramma.jar my.first.project.Primo

il **classpath** è sostanzialmente una sorta di filesystem virtuale dentro nel quale la JVM cerca le classi. Se il nostro programma fosse composto di più classi archiviate in più files jar avremmo potuto passarli tutti alla JVM concatenandoli:

java -cp primoprogramma.jar:secondo.jar my.first.project.Primo

**NOTA:** _su Windows i file_ `.jar` _devono essere separati da punto e virgola \(‘_`;`_‘\) e non da due punti \(‘_`:`_‘\) che invece funziona su Unix e OSX._

## Il main

Come ultima nota circa il nostro programmino di 6 righe \(parentesi comprese\) va osservato che il contenuto della classe è formato da un metodo chiamato **main** e dichiarato `public` e `static` \(dei qualificatori parliamo meglio nella lezione sulla programmazione orientata agli oggetti\).

Il fatto che il metodo si chiami `main` non è assolutamente un caso: main è precisamente il nome che deve avere il metodo che vogliamo far eseguire per primo alla JVM quando viene lanciata. Non solo: main dovrà avere anche la stessa firma \(signature, ovvero gli argomenti ed il valore di ritorno\) che abbiamo utilizzato in Primo.

Il fatto che il metodo statico `main` ritorni `int` significa che sarà possibile restituire un intero al sistema operativo come risultato dell’esecuzione di un programma \(questo valore di ritorno è solitamente considerato un modo per segnalare un eventuale errore se è diverso da zero\) mentre l’argomento **args** di tipo String\[\] \(array di stringhe\) sta a significare che quel metodo potrà ricevere \(dalla JVM e dal sistema operativo che la esegue\) un numero arbitrario di argomenti di tipo stringa; se modificate il programma come segue potrete sperimentare questa caratteristica:

package my.first.project;

public class Primo {

```text
public static void main(String\[\] args) {

    System.err.println("ciao " + args\[0\] + " !" );
}
```

}

Compiliamo l’esempio ed eseguiamolo il comando:

java my.first.project.Primo " Java Developer"

In questa ultima versione potete anche osservare come sia semplice concatenare stringhe in java utilizzando l’operatore ‘`+`‘, che tra numeri effettua la somma mentre tra le stringhe è \(overloaded\) usato con il significato di concatenare.

Tanto per non stare con le mani in mano prima di iniziare la prossima lezione potete provare ad osservare cosa succede se:

* eseguite il programma specificando gli argomenti senza le virgolette
* eseguite completamente senza argomenti

Cercando di spiegarvi anche il motivo dei risultati che ottenete.

