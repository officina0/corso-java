Nella sezione precedente abbiamo visto come poter realizzare un primissimo progetto scritto in Java, come compilarlo e come costruire il file jar, possiamo dire tutto interamente “fatto a mano” e con gli unici e soli strumenti fornitici dal JDK.

Tuttavia se dobbiamo utilizzare “daily” questo strumento per progetti medio/grandi, visto l’enorme bagaglio di librerie che a corredo della JRE, le moltissime librerie di terze parti che esistono e che è possibile importare nel progetto e viste le dipendenze che esistono fra i diversi progetti, è abbastanza importante avere a nostra disposizione strumenti avanzati per lo sviluppo, che ci supporti e ci semplifichi la vita.

Perciò in questo articolo facciamo una breve rassegna degli ambienti di sviluppo (IDE – Integrated Development Environment) più famosi e più utilizzati nel mondo Java:

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/netbeans.png)  
NetBeans

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/eclipse.png)  
Eclipse

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/idea.png)  
IntelliJ Idea

Anche la generazione dei un file jar ed in generale la **gestione complessiva dei progetti** non è pensabile come operazione da fare manualmente senza l’ausilio di altri strumenti che tengano in considerazione tutte le dipendenze, legami e configurazioni presenti fra i diversi elementi che compongono un progetto Java.

I sistemi per l’**automazione del processo di build** ci vengono in aiuto in questo aspetto del ciclo di sviluppo delle applicazioni; in questa categoria prenderemo in considerazione:

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/ant.png)  
Ant

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/maven.png)  
Maven

IDE – Integrated Development Environment
----------------------------------------

L’IDE per uno sviluppatore è un amico fidato con cui passa la maggior parte del tempo. Beh, forse è per questo che sembra che gli sviluppatori si dividano in tifoserie agguerrite a difesa del proprio ambiente di sviluppo preferito.

Cercando in rete qualche confronto tra Eclipse, NetBeans e anche IntelliJ Idea si trovano vere e proprie battaglie di opinione. Ma forse la domanda stessa _“quale è l’IDE migliore?”_ è mal posta: non esiste un IDE migliore. Si tratta di applicazioni di grande complessità, estendibili, con centinaia (letteralmente centinaia) di plugin e moduli, tutte hanno le features indispensabili, quelle utili ed anche quelle delle quali non sentirete mai la necessità, ma ciascuna le “offre” secondo le sue modalità e punti di vista, quindi _è una questione di gusto ed abitudine_ scegliere l’una o l’altra.

Io uso Eclipse (per abitudine) e tutte le volte che nelle prossime lezioni ci sarà la necessità di utilizzare l’IDE (generazione di codice, validazione, gestione dei build e del deploy) lo faremo con Eclipse ma potete star sicuri che si possono effettuare le stesse operazioni anche con NetBeans e Idea.

Per scegliere davvero il consiglio è quello di provarli (a più riprese ed a livelli diversi di esperienza con Java, perché le cose possono cambiare) e di non “innamorarsi”: ricordiamoci che sono solo IDE e usare l’uno o l’altro è solo una questione di abitudine.

### NetBeans

Nato come progetto universitario negli anni 90 e poi acquistato da Sun, che decise nel 2000 di renderlo un progetto open source rilasciando tutti i sorgenti alla comunità, [NetBeans](https://netbeans.org "NetBeans") è da considerarsi l’IDE “ufficiale” per lo sviluppo con Java essendo ad oggi supportato direttamente da Oracle.

Per alcuni anni è stato considerato una sorta di progetto all’inseguimento del più “featured” Eclipse, ma con l’impegno diretto di Oracle è oggi un competitor di tutto rilievo e del tutto intercambiabile con gli altri IDE.

NetBeans è il primo IDE ad avere supporto completo per le nuove features di Java 8, Java Enterprise 7 e HTML5.

Per scaricarlo è sufficiente raggiungere la pagina di [download di NetBeans](https://netbeans.org/downloads/index.html "Scaricare NetBeans") e selezionare la versione desiderata. In prima istanza sarà sufficiente la “Java SE” ma ne esistono altre versioni per lo sviluppo in C/C++, HTML5/PHP, Java Enterprise, ed una che comprende tutte le altre. Per completare l’installazione basta eseguire il file scaricato.

Appena aperto, l’IDE vi mostrerà una scheda informativa con l’accesso a documentazione e tutorial; se volete iniziare a programmare chiudete la scheda informativa usando il pulsantino in alto a sinistra.

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_01_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_01.png)

Per **creare un progetto Java con Netbeans** è sufficiente selezionare il menu `New > Project`. Il wizard ci guida poi fino alla creazione della prima classe nel progetto ed inserisce per noi anche il metodo `main`.

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_02_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_02.png)

Come si vede in figura l’IDE ci aiuta nella scrittura del codice segnalandoci gli errori (il marker rosso alll’inizio della riga ancora non completa), assistendoci nella scelta dei nomi dei metodi (la dropdown list contiene tutti i metodi dell’oggetto `System.out` che matchano con la parte scritta) e dandoci immediatamente anche la documentazione (quella generata con _javadoc_).

Infine per compilare ed eseguire il progetto non serve tenere a mente dove si trova il suo main, la struttura delle classi e il classpath, NetBeans lo fa per noi e basta utilizzare il pulsante `Play` (quello con la freccia verde in alto) per eseguire e vedere l’output senza lasciare l’ambiente di sviluppo.

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_03_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_03.png)

### Eclipse

[Eclipse](http://www.eclipse.org "Eclipse") è certamente l’ambiente di sviluppo più noto e usato in ambito Java (ma anche per molti altri linguaggi). Nasce agli inizi del 2000 per un accordo tra molte grandi società (Borland, IBM, QNX Software Systems, Red Hat, SuSE e molti altri) che concordarono nella creazione di una fondazione (la _Eclipse Foundation_ appunto) per promuovere lo sviluppo e la crescita di un IDE originariamente sviluppata da IBM (ed il cui investimento in termini di tecnologia e sviluppo era a quei tempi stimato in qualcosa come 40 milioni di dollari).

Oggi la [Eclipse Foundation](https://www.eclipse.org/org/foundation/ "Eclipse Foundation") oltre all’IDE vero e proprio rappresenta una delle organizzazioni più floride di [progetti open source](http://projects.eclipse.org/ "Eclipse Projects").

Installare Eclipse è semplice: basta [scaricare l’archivio](http://www.eclipse.org/downloads/ "Scarica Eclipse") ed eseguirlo. Come per NetBeans non c’è molto altro da fare e l’IDE è pronto. Una volta lanciato, ci viene chiesto di selezionare uno workspace, una directory di riferimento che Eclipse utilizza per organizzare i progetti, ciascuno con una sua directory specifica.

La pagina di benvenuto di Eclipse è simile a quella di NetBeans e mostra l’immancabile scheda introduttiva con i link alla documentazione ed i tutorial.

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_04_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_04.png)

La **creazione di un progetto** è guidata da un semplice wizard (lanciato scegliendo, `new > java project`) .

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_05_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_05.png)

Anche in questo caso è possibile con un solo pulsante `Play` (la freccia verde nella barra in alto) eseguire un programma Java e visualizzarne l’output senza lasciare l’IDE.

[  
![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_06_s.png)  
](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_06.png)

Eclipse è diventato di uso comune anche perché google offre i plugin per sviluppare applicazioni Android, e Google App Engine e GWT.

### IntelliJ IDEA

Probabilmente il più giovane tra gli IDE che stiamo esaminando ma di certo il più aggressivo: la sua [pagina Web](http://www.jetbrains.com/idea/ "IntelliJ IDEA") riporta nel title _“The Best Java and Polyglot IDE”_.

A differenza delle altre soluzioni proposte, Idea è un prodotto commerciale di JetBrains che ne offre una **versione community** liberamente scaricabile ed una versione commerciale (“ultimate”).

Oltre a tutte le features che caratterizzano anche le altre IDE, Idea offre un egregio supporto per la gestione di progetti basati su maven ed ha i suoi maggiori fan tra gli sviluppatori “enterprise”, dove la flessibilità e la potenza degli strumenti di sviluppo viene messa a dura prova.

Build System
------------

Anche per quanto riguarda i build system (_maven_ in realtà non è semplicemente un build system, ma un project management & comprehension tool) citiamo qui quelli più diffusi e ne vediamo le caratteristiche e le funzionalità di base, lo scopo di questa guida non è quello di approfondire in dettaglio questi argomenti, ma è utile conoscere l’esistenza dei tool più importanti per la gestione dei progetti Java.

### Apache Ant

[Apache Ant](https://ant.apache.org/ "Apache Ant") (più comunemente _Ant_) è un tool di sviluppo per l’automazione del processo di building, o semplicemente build system. È un progetto Apache, rilasciato Open Source sotto licenza Apache Software License.

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_07.png)

Per fare un paragone, è simile a _Make_ (famoso tool per la complilazione di progetti C/C++). Anche se implementato completamente in Java ed appositamente progettato per la piattaforma Java, Ant può gestire qualunque tipo di progetto (anche C++).

La principale caratteristica di Ant è l’utilizzo di file XML per descrivere il processo di build, tipicamente (per default) si utilizza un file chiamato `build.xml`.

Il build file contiene informazioni su come effettuare il build del progetto e per ogni progetto possono essere presenti più **target** (azioni come creare directory, compilare i sorgenti, eseguire test, …) e ciascun target può avere dipendenze con altri target.

Ecco un esempio di file `build.xml` per un semplice progetto Java “Hello World”, Vengono definiti 4 target (`clean`, `clobber`, `compile` e `jar`), ciascuno dei quali ha associata una descrizione.

```
<?xml version="1.0"?>
<project name="Hello" default="compile"> 
	<target name="clean" description="remove intermediate files">
		<delete dir="classes"/>
	</target> 
	<target name="clobber" depends="clean" description="remove all artifact files">
		<delete file="hello.jar"/>
	</target> 
	<target name="compile" description="compile the Java source code to class files">
		<mkdir dir="classes"/>
		<javac srcdir="." destdir="classes"/>
	</target> 
	<target name="jar" depends="compile" description="create a Jar file for the application">
		<jar destfile="hello.jar">
			<fileset dir="classes" includes="**/*.class"/>
			<manifest>
				<attribute name="Main-Class" value="HelloProgram"/>
			</manifest>
		</jar>
	</target>
</project>
```

Il target `jar` come dipendenza `compile`, questo significa che prima che Ant inizi ad eseguire il target `jar` dovrà aver prima eseguito correttamente il target `compile`.

All’interno di ogni target sono specificate le operazioni che Ant deve eseguire; queste sono spesso realizzate con task che Ant ha già definiti (built-in).

#### Installare Ant

Se stiamo lavorando con uno degli IDE sopra descritti, l’installazione di Ant è quasi gratuita e gli ambienti di sviluppo hanno già Ant incluso nella maggior parte delle loro distribuzioni. Per info e dettagli:

*   [Ant su netBeans](https://netbeans.org/features/java/build-tools.html "Ant su netBeans")
*   [Ant su Eclipse](http://ant.apache.org/ivy/ivyde/history/trunk/ant.html "Ant su Eclipse")
*   [Ant su IntelliJ Idea](https://www.jetbrains.com/idea/webhelp/ant.html "Ant su IntelliJ Idea")

In alternativa il primo passo è quello di [scaricare Ant](https://ant.apache.org/bindownload.cgi "Scaricare  Ant") dal sito della Apache Foundation. Preleviamo l’ultima versione del bynaryfile. A seconda del sistema operativo che abbiamo scarichiamo un archivio tar oppure zip (nel caso di Windows).

Una volta scaricato e decompresso il file scegliamo il path “definitivo” per Ant sulla nostra macchina. Infine andranno impostate le variabili d’ambiente `JAVA_HOME` e `ANT_HOME` e andrà aggiunto `${ANT_HOME}/bin` (Unix) o `%ANT_HOME%/bin` (Windows) nella `PATH`.

Per ulteriori info e dettagli su come installare e configurare Ant (anche in base al sistema operativo) fate riferimento alla [guida ufficiale](https://ant.apache.org/manual/install.html "Guida ufficiale Apache Ant").

### Apache Maven

[Apache Maven](http://maven.apache.org/ "Apache Maven") (o solo Maven) è un tool per l’automazione della fase di building di un progetto usato principalmente e primariamente per progetti Java.

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/08/java04_08.png)

Maven mira principalmente a risolvere due aspetti:

1.  descrivere come il programma/progetto deve essere costruito;
2.  descrivere le sue dipendenze.

Come per Ant, la descrizione del processo di build, le sue dipendenze da moduli e componenti esterne, l’ordine delle operazioni, le directory e i plugin necessari, è fatta attraverso un file XML.

Maven scarica automaticamente tutte le librerie Java ed i plugin necessari da uno o più repository (come _Maven2 Central Repository_) e li salva in una cache locale.

I progetti Maven vengono configurati utilizzando un **Project Object Model**, che è salvato in un file chiamato `pom.xml`. Di seguito un esempio minimalista:

```
<project>
	<!-- model version is always 4.0.0 for Maven 2.x POMs -->
	<modelVersion>4.0.0</modelVersion>
	<!-- project coordinates, i.e. a group of values which uniquely identify this project -->
	<groupId>com.mycompany.app</groupId>
	<artifactId>my-app</artifactId>
	<version>1.0</version>
	<!-- library dependencies -->
	<dependencies>
		<dependency>
			<!-- coordinates of the required library -->
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<!-- this dependency is only used for running and compiling tests -->
			<scope>test</scope>
		</dependency>
	</dependencies>
</project>
```

Questo POM definisce un identificatore univoco per il progetto (coordinates) e le sue dipendenze nel framework JUnit.

In ogni caso questo è abbastanza per costruire il progetto ed eseguire gli unit test associati (Maven fornisce valori di default per la configurazione del progetto).