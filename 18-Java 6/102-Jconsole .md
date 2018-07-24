In questo capitolo affrontiamo una nuova libreria introdotta con la versione 5 di Java: il [package java.lang.management](http://java.sun.com/j2se/1.5.0/docs/api/java/lang/management/package-summary.html "Documentazione Sun - link esterno"). Rispetto ai diversi set di API introdotti finora, questo non ha lo scopo di migliorare gli aspetti di sviluppo, inteso come produzione del codice, ma quelli di gestione dell’applicazione, in particolare della gestione della memoria e delle criticità che l’esecuzione di un programma può presentare (ad esempio accessi concorrenti o altri aspetti peculiari del multithreading).

Effettivamente, una delle carenze del linguaggio era quella di non poter valutare le risorse su cui esso viene eseguito (anche perché non esegue codice nativo) colmata comunque con l’introduzione di questo package e di uno strumento che approfondiremo: **JConsole**.

Prima di vedere cos’è JConsole, vediamo le principali classi che vengono utilizzate dal tool e che possono essere utilizzate da interfaccia programmatica (API) per le esigenze dei programmi.

*   ClassLoadingMXBean Class loading system;
*   CompilationMXBean Compilation system;
*   MemoryMXBean Memory system;
*   ThreadMXBean Threads system;
*   RuntimeMXBean Runtime system;
*   OperatingSystemMXBean Operating system;
*   GarbageCollectorMXBean Garbage collector;
*   MemoryManagerMXBean Memory manager;
*   MemoryPoolMXBean Memory pool.

Ognuno di essi (come potete vedere dalla convenzione) è un MXBean che è creato secondo lo standard JMX ([Java Managment eXtensions](http://java.sun.com/j2se/1.5.0/docs/guide/jmx/spec.html "Java Managment eXtensions - link esterno - documentazione Sun")). Non vedremo nel dettaglio il funzionamento e l’uso delle suddette classi. Diciamo solo che attraverso esse è possibile interrogare le diverse risorse utilizzate da una JVM per poter capire come le sta utilizzando, quindi torna utile per valutare a runtime la bontà di un nostro algoritmo o di come le nostre classi si stanno relazionando.

In realtà non utilizzeremo quasi mai direttamente queste classi. Se vogliamo monitorare l’andamento di un nostro programma Java lo faremo utilizzando il già citato JConsole, uno strumento diagnostico introdotto da Java 5 che si basa sulla tecnologia JMX; si tratta di uno strumento grafico all’apparenza abbastanza complesso, ma che in realtà ci può dare delle semplici indicazioni su alcuni potenziali problemi che stanno avvenendo sul programma Java in esecuzione.

Il compito di JConsole, secondo la definizione Sun è quello di:

*   Scoprire cali di memoria;
*   Abilitare o disabilitare il Garbage Collector;
*   Scoprire deadlocks;
*   Controllare e manipolare dinamicamente il log delle applicazioni;
*   Accedere alle risorse del sistema operativo;
*   Gestire i Managed Beans (MXBeans) visti sopra.

Per accedere al tool dovrete eseguire il programma _bin/jconsole.exe_ che si trova nella directory degli eseguibili della JDK (1.5 o superiore). Una volta acceduti, un pannello di controllo vi chiedere le azioni da eseguire. Potete scegliere tra “amministrazione locale”, “amministrazione remota” e “amministrazione avanzata”.

La prima gestirà la (o le) Java Virtual Machine in esecuzione in locale, la seconda vi permetterà di accedere ad una macchina remota (un server su cui è presente un servizio, ad esempio), con la terza, sempre remotamente, vi potrete connettere anche a una JVM su cui gira JDK 1.4.

Una volta selezionata la modalità avrete un pannello con le diverse risorse disponibili. Le aree principali sono:

1.  Sommario
2.  Memoria
3.  Threads
4.  Classi
5.  Virtual Machine
6.  Managed Beans

Sta di fatto che in alcune di esse avrete la possibilità di studiare l’andamento grafico (memoria), la concorrenza di risorse (threads) o il numero di classi che, in una determinata istanza della JVM, si stanno eseguendo al momento.

Vi sono alcune caratteristiche basiche che consentono un immediato uso del tool, ma sicuramente, per una amministrazione più professionale, dovrete imparare bene ognuna delle possibili situazioni critiche e come poterle risolvere con JConsole: a tal proposito vi consiglio la lettura della [documentazione Sun](http://java.sun.com/developer/technicalArticles/J2SE/jconsole.html "JConsole - documentazione Sun - link esterno").