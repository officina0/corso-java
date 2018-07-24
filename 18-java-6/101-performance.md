# 101 Performance

Java, essendo un linguaggio di programmazione “virtuale”, è sempre stato etichettato come un linguaggio a scarse performance. Anche da questo punto di vista, negli anni, il linguaggio si è trasformato portando migliorie da una versione all’altra, addirittura tra il 50 e il 75 percento. Tutto ciò è garantito proprio dall’ambiente di compilazione e di esecuzione del bytecode, con delle attività che elevano a uno stato di ottimo le performance di un software sviluppato con Java.

Riporto qui una slide presentata al Java Conference di Zurigo del Giugno 2007:

Figura 26.1. Miglioramenti velocità Java

![Velocit&#xE0; Java](http://www.html.it/guide/img/guida_java_6/java.jpg)

Come vediamo, la crescita nelle performance è lineare e sempre ben marcata \(questa la slide della JRE server, ma quella desktop si discosta di poco\). Fatta 100 la versione 1.3, la 1.6 arriva quasi a 350 di rendimento.

Ovviamente mai potrà attestarsi a livelli di un linguaggio a bassissimo livello ma, secondo alcune ricerche, ci sono delle similitudini a livello di performance che permettono di fare ottime esecuzioni \(ovviamente non sceglieremo mai Java per sviluppare un software per applicazioni critiche real-time\). Vediamo alcune delle attività nascoste del linguaggio Java che gli consentono di avere prestazioni eccellenti:

* Compilazione Just In Time \(JIT\);
* Hot Spot;
* Garbage Collection.

Discutiamo di come si siano evolute queste attività nelle ultime versioni. Ovviamente si tratta di attività nascoste che il comune programmatore non si deve preoccupare di conoscere, ma che ci sollevano dall’effettuare alcuni compiti \(come la gestione della memoria da parte del garbage collector\) che necessiterebbero un livello di conoscenza più profondo della tecnologia.

Attraverso la compilazione Just In Time si effettua un lavoro di ottimizzazione per eseguire un pezzo di codice Java compilandolo in codice nativo solo al momento della reale necessità. Java effettua questo con un linguaggio intermedio \(il famoso bytecode\), il cui compito è quello di rendere più facile il passaggio verso il codice finale “nativo” sul sistema operativo di destinazione. Nelle ultime versioni di Java sono introdotte delle migliorie che decisamente migliorano in termini percentuali elevati grazie a tecniche “adattative” che danno vita al seguente strumento di Java: l’hot spot.

**Java Hot Spot** \(che è un trademark di Java\) è uno strumento, introdotto nella versione di Java 2 \(o 1.2\), il cui compito è quello di individuare gli “hot spots” \(i punti caldi\) di esecuzione per poterli ottimizzare e rendere il processo di traduzione ed esecuzione il più rapido possibile. Per intenderci, se l’hot spot individua un metodo di una classe utilizzato spesso, lo compila e lo lascia già compilato in maniera che il prossimo utilizzo non necessita compilazione. Come faccia la JVM ad individuare un hot spot è compito degli algoritmi adattativi di intelligenza artificiale, che addirittura sono in grado di prevedere con un basso margine di errore le attività più frequenti nel ciclo di vita di una applicazione \(giusto per fare un esempio, se dopo un metodo `open()`, chiamiamo sempre un metodo `write()` è facile presupporre che alla prossima chiamata di `open()` la JVM si aspetti un `write()`\). Anche qui, le ultime versioni portano delle migliorie in questi algoritmi adattativi, prendendo a vantaggio anche l’utilizzo del Class Data Sharing.

Il **Class Data Sharing** viene introdotto in Java 5 e consente di migliorare la velocità all’avvio di una applicazione riutilizzando eventuali package di librerie già caricati in memoria da altre JVM.

Il **garbage collector** è un altro utilissimo strumento, forse il più importante per il successo di Java. Chiunque abbia sviluppato applicazioni informatiche, prima dell’utilizzo di Java, sa bene quanto complessa e “costosa” sia la gestione della memoria in un linguaggio di programmazione dove lo spazio deve essere allocato \(prima del suo utilizzo\) e liberato \(quando non ci serve più\). Con Java tutto questo non è mai servito proprio grazie al Garbage Collector che è un’altra attività automatica effettuata dalla JVM, il cui compito è verificare se un’istanza di un oggetto \(e la relativa parte di memoria nell’heap\) sia ancora necessario \(referenziato da uno degli oggetti che il programma sta utilizzando\). Possiamo pensare al garbage collector come a un processo asincrono che tiene una mappa di oggetti e referenze alle istanze utilizzate e che cancella lo spazio di memoria fisico quando uno di questi oggetti non ha referenze da parte di nessuno.

Dal punto di vista delle sostanziali modifiche, anche qui, le principali vennero fatte nella versione 1.5, con l’introduzione del concetto di “young generation” e “old generation”. In realtà il concetto è un po’ più complesso di quello che vado a spiegare ma il principio rimane valido. Quello che si è visto è che la maggior parte delle istanze di una classe sono temporanee, create, utilizzate e scartate, con un ciclo di vita abbastanza giovane. D’altra parte una piccola minoranza di istanze \(circa il 10% secondo studi\) ha un ciclo di vita maggiore, quasi quello dell’applicazione. Applicare gli stessi concetti di garbage collection è evidentemente una cosa inutile, in quanto quello che vale per oggetti dal ciclo di vita giovane, non vale per il ciclo di vita vecchio.

La Sun ha quindi introdotto una suddivisione tra “**young generation**” e “**old generation**” con garbage collector che appartengono alle due classi e si comportano di conseguenza. Non entriamo nel dettaglio di nessuna delle tecniche utilizzate in quanto richiederebbero approfondimenti piuttosto lunghi, ma penso che già il concetto ci faccia capire le principali modifiche per avere un maggiore rendimento.

