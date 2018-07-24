Le annotazioni di default sono di sicuro interesse, in quanto permettono di migliorare alcuni aspetti nelle fasi di compilazione, facendo sì che lo sviluppatore possa fare a meno di preoccuparsi di alcuni potenziali errori di codifica.

JDK 5 permette allo sviluppatore di definire delle proprie annotazioni totalmente customizzabili. Si tratta semplicemente di definire il nome dell’annotazione e di alcune proprietà che la contraddistinguono.

La cosa che rende le annotations uno strumento davvero importante, tanto da alterare il tipico paradigma di programmazione, è la possibilità di effettuare introspezione del codice.

Con la [reflection](http://java.html.it/articoli/leggi/2414/reflection-in-java/ "Reflections - Articolo") è possibile valutare a runtime quali annotations sono presenti (e quali valori hanno in esse) e quindi effettuare determinate operazioni. Possiamo pensare ad un [framework che gestica la persistenza con un database](http://java.html.it/articoli/leggi/2397/framework-per-la-gestione-di-un-database/ "framework che gestica la persistenza con un database - articolo"), dove all’interno del codice sono presenti delle annotations che indicano come mappare attributi di classe su colonne di database. Oppure utilizzare uno strumento personalizzato per team di sviluppo per commentare opportunamente il codice, chi ne modifica i metodi, chi li crea e quando e così via, per mantenere traccia del lavoro svolto.

Per un esempio pratico sulla creazione delle Java Annotations, rimandiamo all’[articolo dedicato](http://java.html.it/articoli/leggi/3002/creazione-di-java-annotations/ "Articolo sulla creazione di Java Annotations"), che sicuramente tratta meglio della guida l’argomento.

Dove usare le annotazioni
-------------------------

È chiaro come l’utilizzo opportuno di annotation sia capace di dare tanta espressività in più al codice prodotto. Il reale vantaggio che si ha è quando tale strumento viene affiancato da altri strumenti che fanno introspezione del codice e, in base ad esso, creano delle configurazioni per framework.

È proprio in queste situazioni, infatti, che le annotations hanno un notevole vantaggio. Prima citavamo il framework per la persistenza automatizzata, è il caso di [Hibernate](http://java.html.it/articoli/leggi/2421/introduzione-ad-hibernate/ "Introduzione ad Hibernate"), che permette di mappare classi e tabelle attraverso descrittori XML. Ora, attraverso l’uso di annotation, non sarà più necessario creare dei descrittori di configurazione, bensì, adottare delle annotazioni che suggeriscano l’associazione direttamente all’interno del codice.

Mantenere descrittori di configurazione opportunamente allineati può essere un problema, in quanto si tratta di file esterni (generalmente XML), difficilmente leggibili dall’occhio umano.

È il caso anche degli [EJB 3.0](http://java.html.it/articoli/leggi/2464/ejb-30/ "Articolo sugli EJB 3.0") che, dalle annotazioni, traggono un notevole vantaggio, rendendo più veloce e meno incline ad errori la produzioni di logica applicativa enterprise.