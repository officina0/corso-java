Il 23 Maggio 1995, John Gage, l’allora direttore dello “Science Office” di _Sun Microsystems_ insieme a Marc Andreessen, cofondatore e vicepresidente di Netscape Corporation, annunciarono alla SunWorld™ che “_Java wasn’t a myth_” e che a breve Netscape Navigator avrebbe integrato questa nuova tecnologia nel suo web browser. Era nata una nuova pietra miliare della storia dell’informatica: **Java**.

Immaginate 13 tra i maggiori esperti di informatica degli anni ’80 raccolti in un ufficio a Menlo Park, Californa, per quello che all’epoca era chiamato lo “_Stealth Project_” (poi rinominato “_Green Project_“, roba da spy story se non addirittura da racconto fumettistico) a sviluppare il linguaggio di programmazione degli anni a venire.

Eppure era successo che tra gli anni ’70 e la fine degli anni ’80 l’hardware aveva subito una delle sue più grandi rivoluzioni: prezzi caduti a picco e performance aumentate straordinariamente; Sun Microsystem aveva individuato nella realizzazione di un nuovo linguaggio di programmazione uno dei componenti essenziali per catturare “the next-wave” nell’evoluzione dell’informatica.

In quegli anni il **C** (Dennis Ritchie,1972) era nel pieno della sua diffusione ma la sua struttura procedurale ed il suo approccio “low level” (puntatori, allocazione statica della memoria, etc.) lo rendevano impegnativo e di difficile adozione per progetti di sviluppo sempre più complessi e con team di sviluppatori sempre più grandi.

Alla fine degli anni ’70 anche un nuovo player era entrato in campo: il **C++** (Bjarne Stroustrup, 1979) introduceva i principi della Programmazione Orientata agli Oggetti e alla fine degli anni ottanta era di sicuro il punto di riferimento per i progetti di sviluppo più grandi e ambiziosi.

Il piano del Green Team era dunque quello di realizzare un linguaggio che, come il C++, potesse trarre vantaggio dal nuovo paradigma di programmazione ad oggetti ma che non fosse low-level (il C++ era pur sempre una evoluzione del C) e si occupasse automaticamente della gran parte dei dettagli di gestione della memoria e potesse essere facilmente ‘portatò su processori e dispositivi diversi.

Dopo 2 anni di lavoro del Green Team nacque un linguaggio chiamato _Oak_ che rischiava di finire cancellato se Sun ed il Green Team stesso non avessero avuto la felice idea di catturare con il loro linguaggio la vera “next-wave” degli anni 90: Internet.

Nel giro di pochi mesi riuscirono a sviluppare un web browser interamente in Java (Sun’s _HotJava_, 1994) con il quale poterono mostrare l’idea che avrebbe legato il nuovo linguaggio alla evoluzione di Internet: le **applets**, piccole applicazioni che attraverso un browser potevano essere distribuite ed eseguite come mai prima. Intessendo infine rapporti con Netscape Corporation ed arrivando ad avere il supporto per le applet nel Navigator nel 1995 Sun garantì una rapida diffusione a Java.

![Duke](http://www.html.it/wp-content/uploads/2006/03/java01_01.png)

Java e la OOP resa “facile”
---------------------------

Java nasce quindi sostanzialmente per creare un linguaggio Object Oriented che risolvesse principalmente due problemi concreti:

1.  garantire maggiore semplicità rispetto al C++, per la scrittura e la gestione del codice;
2.  permettere la realizzazione di programmi non legati ad una architettura precisa.

Il primo degli obbiettivi fu affrontato liberando il programmatore dall’onere della gestione della memoria (e togliendo dalle sue mani la gestione dei puntatori) creando il primo linguaggio destinato alla grade diffusione basato su un sistema di gestione della memoria chiamato **gargbage collection** in cui ‘automaticamentè la memoria viene assegnata e rilasciata a seconda delle esigenze del programma.

Il secondo punto fu invece affrontato applicando il concetto di Macchina Virtuale (**JVM**) e facendo sostanzialmente in modo che i programmi non fossero compilati in codice macchina (nativo) ma in una sorta di codice “intermedio” (chiamato **bytecode**) che non è destinato ad essere eseguito direttamente dall’hardware ma che deve essere, a sua volta, interpretato da un secondo programma, la macchina virtuale appunto.

Questo significa che lo stesso codice può essere eseguito su più piattaforme semplicemente trasferendo il bytecode (non il sorgente) purché sia disponibile una JVM (concetto chiamato WORA, Scrivi una volta ed esegui ovunque).

Java, C, C++ e JavaScript
-------------------------

Java eredita dal C buona parte della sua sintassi (scelta fatta per facilitarne l’adozione da parte degli sviluppatori C e C++) mentre dal C++ eredita l’approccio object-oriented e basato sulle classi.

Parlando di similitudini Java e JavaScript non ne hanno quasi nessuna: ci si limita quasi esclusivamente al nome (per motivi di marketing) e alla una sintassi molto vicina al C.

Le versioni di Java
-------------------

La prima implementazione pubblica del 1995 (Java 1.0) mantenne da subito la promessa dello sviluppo WORA, ed essendo questo linguaggio abbastanza sicuro per gli standard dell’epoca (grazie alla sandbox naturale offerta dalla JVM), la maggior parte i web browser seguirono Netscape Navigator e la incorporarono per permettere l’utilizzo delle “applet”.

Insieme alla prima JVM fu rilasciato anche il **JDK 1.0** (Java Development Kit) che includeva sia l’ambiente di esecuzione (JVM + librerie, chiamato _JRE_, Java Runtime Environnement) che il compilatore (java to bytecode, _javac_) e le tool di sviluppo.

### Java 2

La versione **Java 1.2** fu rilasciato nel 1998 come un “quick fix” delle versioni precedenti ma fu considerata l’inizio per un nuovo Java, da cui il nome Java2 che ha accompagnato fino ai gionri nostri ogni nuova release di Java: la versione Java 1.3 è chiamata anche Java3 etc.

I maggiori cambiamenti furono:

*   riscrittura del sistema di event handling (aggiunta dell’Event Listner);
*   introduzione del JIT (Just In Time compiler)

Con la versione “Java 2” furono introdotte anche 2 nuove distrubuzioni:

*   **J2EE**, destinata allo sviluppo Enterprise (Java 2 Enterprise Edition)
*   **J2ME**, destinata allo sviluppo si dispositivi mobile ed embedded (Java 2 Micro Edition)

e per enfatizzarne la nascita la distribuzione ‘classicà fu rinominata **J2SE** (Java 2 Standard Edition).

### Java 3

Nel Maggio del 2000 è stata rilasciata la terza release: **Java 3** e i cambiamenti più rilevanti sono stati:

*   inclusione di HotSpot JVM (macchina virtuale ottimizzante ed adattativa);
*   JavaSound;
*   Java Naming & Directory Interface (JNDI) nelle core libraries (in precedenza utilizzabile com estenzione);
*   JDPA (Java Platform Debugger Architecture).

### Java 4

Del 2002 è la **Java 1.4**, che ha aumentato le caratteristiche del linguaggio e le API disponibili, con l’aggiunta di:

*   Assertion;
*   Regular Expression;
*   XML processing;
*   Non-blocking I/O;
*   Logging.

### Java 5

Del 2004 è invece la versione 1.5, che include (oltre a numerose ottimizzazione e bugfix sulla JVM):

*   Generics (che forniscono la tipazione a compile-time);
*   Autoboxing/unboxing;
*   Static imports;
*   Annotation/Metadata.

### Java 6

Nel Dicembre 2006 è stata rilasciata la 1.6, che aggiunge:

*   Web services;
*   Scripting (che facilita l’embedding in java di scripting engines, tra cui uno per il Javascript basato su Mozilla Rhino);
*   Desktop APIs e Desktop deployment;
*   Security (Java SE 6 semplifica la gestione della sicurezza con l’aggiunta di nuovi modi per accedere ai servizi nativi, come PKI, Public Key Infrastructure, servizi crittografici per l’autenticazione e comunicazone sicura su MSWindows, Java Generics Security Services -Java GSS-, servizi Kerberos per l’autenticazione, accesso a server LDAP per l’autenticazione degli utenti).

### Java 7

La versione la 1.7, rilasciata nel Luglio 2011 aggiunge al linguaggio il supporto della JVM per i linguaggi dinamici, nuova libreria file I/O e nuove APIs per la grafica.

### Java 8

La versione 1.8 di Java è quella attuale e presenta, accanto ad uttimizzazioni sulla JVM, crittografia, un nuovo Interprete JavaScript (Nashorn, ECMAScript-262 compliant) anche la prima incarnazione del Project Lambda ossia Lamba Expressions native in Java (una prima tranche di programmazione funzionale) assieme agli Stream (set infiniti) relative bulk operations.

Curiosità
---------

Se volete sapere i motivi che hanno portato alla scelta del nome Java potete leggere [questo articolo](http://www.javaworld.com/article/2077265/core-java/so-why-did-they-decide-to-call-it-java-.html).