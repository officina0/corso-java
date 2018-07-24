In questo capitolo tratteremo aspetti di carattere più avanzato utili per comprendere quando è opportuno utilizzare uno Stream parallelo invece di uno sequenziale e viceversa. Abbiamo compreso come con Java 8 gli Stream ci abbiano consentito di utilizzare il parallelismo per processare collezioni ed array sfruttando il multicore delle CPU, ora vediamo di analizzare graficamente quello che accade con l’istruzione `integers.parallelStream().sorted()` degli esempi precedenti aggiungendo la funzione terminale `sum()`:

Figura 1. Parallel Streams.

![Parallel Streams](http://www.html.it/wp-content/uploads/2017/11/parallelStreams.png)

L’`ArrayList integers` è il punto di partenza per ottenere più Stream in parallelo, la funzione `sorted()` agisce utilizzando le CPU core a disposizione sugli Stream creati per ordinare gli interi. Al termine dell’operazione di ordinamento, eseguita con task paralleli sulle diverse CPU, si ottiene nuovamente un unico Stream sul quale si applica l’operazione terminale `sum()`.

Lo Stream parallelo utilizza il **framework Fork/Join**, che vedremo nel capitolo successivo, per eseguire la split dello Stream iniziale in più Stream (_Fork_) e la riunificazione successiva in un unico Stream (`Join`).

Nel capitolo precedente abbiamo visto come con lo Stream parallelo si possa affrontare un task in modo più performante rispetto ad uno Stream sequenziale, in realtà le situazioni che si possono avere non sono sempre cosi semplici ed immediate. Dobbiamo considerare il fatto che non tutti gli Stream sono facilmente divisibili (_splitting_). Pensiamo ad esempio all’`ArrayList` utilizzata per il codice di esempio del precedente capitolo, la sua rappresentazione interna è basata su strutture dati di tipo array, su tali strutture calcolare l’indice della posizione centrale è abbastanza semplice e quindi la struttura in se è facilmente suddivisibile.

Se pensiamo invece ad una `LinkedList`, la sua struttura impone la scansione degli elementi per poter individuare il punto di suddivisione, una `LinkedList` quindi non offre buone performance sugli Stream paralleli. Il primo criterio che dobbiamo quindi tener presente, se intendiamo utilizzare al meglio il parallelismo, è che la struttura che utilizziamo deve essere facilmente suddivisibile.

Esistono situazioni come nei giochi di ruolo, si pensi al semplice gioco del Tris, in cui si devono valutare diverse configurazioni possibili per ciascuna mossa consentita in un determinato stato ed il processo della singola configurazione ha un costo elevato. Questa è una situazione affrontabile efficacemente con uno Stream Parallelo.

Il secondo criterio che quindi è possibile tenere in considerazione per l’utilizzo efficace di uno Stream Parallelo è che il numero di Task moltiplicato per il costo medio di ciascun task sia alto.

Un altro aspetto che si può intuire facilmente, ma che è importante sottolineare, è relativo al numero di Core della CPU. Non ha infatti molto significato utilizzare uno Stream parallelo su una CPU ad un solo Core, le performance potrebbero essere addirittura peggiori di uno Stream sequenziale.

Il terzo criterio è quindi un numero di Core superiore ad uno.

Concludiamo il capitolo con un ultimo criterio che non riguarda le performance di uno Stream Parallelo, ma la sua efficacia. La funzione che viene applicata sugli elementi dei vari Stream risultato dello split deve essere:

*   **Indipendente**: la computazione su un elemento non deve fare affidamento sulla computazione degli altri elementi.
*   **Nessuna interferenza**: la funzione non deve modificare lo Stream sottostante.
*   Deve essere **stateless**, ovvero non mantenere alcuno stato.