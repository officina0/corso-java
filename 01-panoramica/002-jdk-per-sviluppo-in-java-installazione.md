# 002 JDK Per Sviluppo In Java Installazione

## Cos’è il JDK

Preparare il proprio computer per sviluppare con Java è davvero un gioco da ragazzi in quanto scaricando un unico file abbiamo subito tutti i programmi che ci servono per iniziare.

Gli strumenti principali che il **Java Development Kit \(JDK\)** ci mette a disposizione sono:

`java` composto dalla macchina virtuale propriamente detta \(JVM\), le librerie standard \(che compongono la cosidetta Java core API\) ed il comando che serve per far partire la JVM ed eseguire i programmi.

Strumento

Descrizione

**javac**

È il **compilatore**, il cui compito è quello di trasformare il codice sorgente Java nel bytecode che sarà poi eseguito dalla macchine virtuale java \(JVM\)

**JRE \(Java Runtime Environment\)**

L’ambiente di runtime

Oltre a questi contiene molti tool per lo sviluppo, di seguito introduciamo solamente i più comunemente usati e ci riserviamo di parlare degli altri via via che serviranno:

Strumento

Descrizione

**jar**

Java Archiver, che utilizzeremo a partire dalla prossima sezione e serve a realizzare archivi di classi.

**javadoc**

Utility che serve per generare la documentazione \(in HTML\) di codice java a partire da commenti inseriti nei sorgenti stessi. La documentazione generata da javadoc è quella che quasi ogni progetto Java mette a disposizione e ne è un esempio quella [ufficiale della java api](http://docs.oracle.com/javase/8/docs/api/).

**javap**

Java Class File Disassembler, una tool per invertire il processo di compilazione, cioè uno strumento che dato un file che contiene la versione compilata \(il bytecode\) di una classe java recupera i nomi ed i tipi dei field ed i metodi della stessa.

**javah**

Tool utilizzata per permettere l’utilizzo di codice scritto in C \(detto nativo\) da java.

**appletviewer**

Viewer per applet che consente di eseguirle senza l’ausilio di uno web browser.

**jdb**

Java debugger.

**Oracle**, che dal 2010 \(anno in cui ha acquistato Sun\) è proprietaria del marchio Java, supporta il Java Development Kit su molteplici architetture e sistemi operativi: tutte le versioni di Windows da Vista a 8, le versioni di Windows Server a partire dalla 2008; Mac OS X Mountain Lion e Mavericks; Linux Oracle, RedHat, Suse, Ubuntu oltre a Solaris e Ubuntu su processori ARM.

Se utilizzate uno di questi sistemi operativi vi basterà aprire con il vostro browser preferito l’URL:

[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

e premere il pulsante “DOWNLOAD” per scaricare la release corrente \( Java Platform \(JDK\) 8, al momento in cui viene scritta la guida\).

![](http://www.html.it/wp-content/uploads/2006/03/java03_01.png)

Sarete automaticamente indirizzati alla pagina dei download e dovrete a questo punto accettare i termini della licenza prima di poter procedere al download.

**Nota:** _Se state usando Linux o BSD e i termini della licenza Oracle non vi convincono potete provare ad usare_ [_OpenJDK_](http://openjdk.java.net)

Su Windows si dovrà effettuare il download di uno dei 2 file eseguibili `jdk-8-windows-i586.exe` e `jdk-8-windows-x64.exe` \(entrambi di oltre 150 MB\) a seconda che si usi un sistema a 32 o a 64 bit mentre su Linux si puo’ scegliere tra gli archivi tar compressi installabili su tutte le distribuzioni e gli rpm che invece sono indicati per Fedora, SUSE etc.

La procedura di installazione è totalmente automatica e sarà sufficiente accettare i default del sistema di installazione. Ad esempio l’installazione su Windows prevede pochi semplici passi: la selezione delle componenti da installare e la scelta della path del JRE.

![](http://www.html.it/wp-content/uploads/2006/03/java03_02.png)

Dopo qualche secondo di attesa avrete installato tutti i tool necessari per sviluppare in Java.

Il wizard, una volta completata l’istallazione ci suggerisce di visitare il sito della piattaforma [Java, standard edition](http://docs.oracle.com/javase/8) dove potrete trovare molti documenti e tutorial. Meglio tenere questo indirizzo tra i preferiti per avere un riferimento in futuro.

## Impostare le variabili d’ambiente \(Windows / Linux \)

Il compilatore \(`javac`\) per default non è nella path e quindi per ‘provarlo’ dovrete andare a cercarlo nella direcroty di installazione, ad esempio:

C:\Program Files\Java\jdk1.8.0\bin\

Po poterlo utilizzare senza doverci preoccupare di includere tutto il percorso possiamo includerlo nelle variabili di ambiente del sistema \(PATH\). Vediamo come fare nei casi più comuni.

### Impostare PATH su Windows

Per le ultime versioni di Windows i passaggi sono piuttosto simili. Si parte dal pannello di controllo \(quello desktop nel caso di Windows 8\) e poi cerchiamo l’icona _Sistema_. Nella maschera che appare clicchiamo su _Impostazioni di sistema avanzate_ \(sulla sinistra\). Infine nella nuova maschera troviamo il pulsante _“Variabili d’ambiente”_. Che ci permetterà di accedere al pannello di modifica delle variabili di sistema.

![](http://www.html.it/wp-content/uploads/2006/03/java03_04.png)

Qui non rimane che cercare la variabile _Path_ e aggiungere il percorso relativo allaa nostra installazione \(copiamolo tutto fino a `[...]bin\`\).

### Impostare PATH su Ubuntu

Su Ubuntu il procedimento è ancora più semplice \(e vale anche per altre distribuzioni che utilizzano bash o derivati\). È sufficiente modificare il file `.bashrc` e aggiungere la riga:

export PATH=$PATH:

ad esempio:

export PATH=$PATH:/opt/jdk1.8.0/bin

## Verificare l’installazione

Per verificare che l’istallazione abbia avuto successo potete provare ad eseguire la macchina virtuale, ad esempio aprendo l’interprete dei comandi \(o il terminale\) e provando a lanciare il comando:

java -version

Il risultato dovrebbe essere simile a quello in figura:

![](http://www.html.it/wp-content/uploads/2006/03/java03_03.png)

Link utili

* [Impostare la PATH](https://www.java.com/it/download/help/path.xml), descrizione dal sito Oracle.
* [Elenco completo dei tool del JDK](http://docs.oracle.com/javase/8/docs/technotes/tools/index.html), release 1.8 del JDK \(SE, standard edition\).

