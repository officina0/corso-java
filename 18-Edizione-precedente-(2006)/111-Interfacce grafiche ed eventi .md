Eccoci finalmente arrivati alla programmazione di interfacce grafiche, quindi alla creazione di applet e di applicazioni a finestre.  
Cerchiamo innanzittutto di stabilire cosa è una interfaccia, e cosa significa farla grafica.  
Innanzitutto ogni programma, come già detto, può essere visto come un oggetto che calcola una funzione, quindi che prende dei dati dall’esterno e restituisce dei risultati. Si pensi ad esempio ad un semplice programma che fa la somma tra due numeri. Il programma in questione prenderà in input due numeri e restituirà come output un un numero, che rappresenta la somma dei primi due.  
Quanto detto è valido in generale per tutte le applicazioni, e non è un caso particolare dell’esempio precedente. Si pensi ad un videogioco, l’input sarà dato dal joystick, e l’output sarà la grafica sullo schermo, quindi concettualmente sia il videogioco, che il programma somma, che qualsiasi altro programma un programmatore possa inventarsi, sono delle funzioni calcolate su dei dati in ingresso che restituiscono dei risultati.  
L’interfaccia del programma verso l’utente che lo usa è il modo in cui il programma prende i dati dall’utente e gli restituisce i risultati.

Fino ad ora abbiamo visto delle interfacce testuali, ovvero nel caso della somma dei due numeri, i dati venivano presi sallo standard input, con una System.in.read() e i risultati venivano stampati sullo standard output con una System.out.print().  
Le System.in e out rappresentano una interfaccia del programma verso l’esterno, in questo caso verso l’utente.  
Altre interfacce con l’esterno che abbiamo già esaminato sono i files, essi rappresentano delle interfacce di ingresso o di uscita verso utenti o altri programmi.  
facendo un piccolo paragone, neanche tanto impossibile, tra un programma e l’uomo, possiamo dire che l’interfaccia in ingresso della mente è rappresentata dalla vista, il tatto, il gusto, l’udito e l’olfatto, mentre l’interfaccia in uscita è rappresentata dalla parola e dal movimento dei muscoli.  
Da notare che questa non è una cosa stranissima, infatti i robot, anche se non hanno gli stessi canali di ingresso degli uomini (hanno vista, una specie di tatto, qualcosa che segnala la prossimità) e di uscita (spesso non parlano, al posto dei muscoli hanno motori elettrici collegati a bracci meccanici, pinze, ecc…), hanno lo stesso funzionamento dell’uomo, ovvero compiono delle azioni (danno un output) in conseguenza a degli stimoli (segnali di input), anche se l’uomo è molto più complesso grazie alla capacità di pensiero, che ancora l’intelligenza artificiale non riesce a replicare in pieno (infatti un robot non è capace di prendere una decisione senza conoscere solo delle informazioni parziali di un problema, come è in grado di fare l’uomo, in base a supposizioni, a volte coscienza, ecc…).

Quindi l’interfaccia è composta da tutti quei canali da cui un programma prende informazioni e verso le quali sputa fuori dei risultati.  
Alcuni dati di ingresso servono a fare cambiare solo stato al programma, altri invece inducono immediatamente un output. Noi li cosideriamo tutti dati su cui vengono calcolate funzioni, infatti una funzione viene calcolata anche in base allo stato di un programma, e quindi anche lo stato, e di conseguenza l’input che lo ha causato, è un input della funzione.  
Chiarisco questo discorso con un esempio. Pensiamo ad un programma che calcola due funzioni, una di somma e una di sottrazione tra due numeri, e pensiamo ad un input che prende un numero per scegliere uno stato (1 o 2), e poi due numeri. A questi due numeri viene applicata la funzione somma o la funzione sottrazione a seconda dello stato del programma.

Ad esempio:

INPUT: <1,10,10> OUTPUT: 20  
INPUT:<2,10,10> OUTPUT: 0

Vedete nell’esempio, come anche l’input sullo stato viene in definitiva usato per calcolare l’output.

Quello che interessa a noi è l’interfaccia utente, abbiamo visto infatti come i programmi possono interfacciarsi con altri programmi o con periferiche (inviando ad esempio comandi ad una stampante), a noi questo non interessa, ma ci interessa vedere tutto quello che serve al programma per dialogare con l’utente, ovvero la cosiddetta interfaccia utente.  
In particolare ci interessa vedere l’interfaccia grafica del programma, ovvero quell’interfaccia che rende molto gradevole il programma all’utente, al posto della grigia interfaccia a caratteri che abbiamo visto.  
L’interfaccia grafica del programma è composta dai cosiddetti componenti GUI (Graphics User Interface), essi sono dei componenti che servono per l’input o per l’output, ed hanno un aspetto grafico. Sono ad esempio dei componenti GUI di un programma tutti i menù, i bottoni, le label, eccetera…di questo.  
Tutte queste GUI saranno ovviamente collegate ad una struttura che le contiene, le due strutture che vedremo, e che sono le più importanti, sono gli applet e le finestre.

Ogni GUI è quindi un pezzettino di interfaccia grafica, essi ricevono degli eventi e in base a questi  
danno dei risultati. Si pensi ad un bottone, esso può essere cliccato e rilasciato, quando un utente programma una interfaccia grafica che contiene un bottone dovrà gestire anche gli eventi associati a questo, ovvero dovrà dire cosa succede quando il bottone viene cliccato, quando viene rilasciato, quando ci si passa con il mouse sopra, eccetera.  
Ogni differente GUI avrà un tipo di evento associato, alcuni sono automaticamente gestiti dalla classe che viene estesa per inserire il GUI nella finestra (ad esempio la modifica grafica del bottone cliccato che diviene evidenziato ), altri possono essere definiti dall’utente (come il click di un bottone, se non c’è il gestore non succede niente) e altri ancora devono essere definiti dall’utente (come tutti gli eventi della tastiera quando si vuole ascoltare almeno uno di questi, ad esempio ci interessa il rilascio di un tasto, dobbiamo gestire anche la pressione, ecc…).  
Ad ogni tipo di evento, per ogni tipo di componente GUI, deve essere dichiarato un ascoltatore dell’evento, esso è un programma che attende il verificarsi dell’evento sul componente, e quando questo si verifica lo gestisce.  
La programmazione di interfacce grafiche è quindi diversa dalla usuale programmazione, infatti qui si disegnano le interfacce, e poi si gestiscono gli eventi che arrivano, mentre nelle usuali applicazioni c’era un main, che rappresentava l’intero programma.  
La gestione degli eventi in Java è cambiata dalla versione 1, a Java 2 (JDK 1.2 in sù), noi vedremo la nuova gestione degli eventi, quella di Java 2, perché la prima è stata cambiata in quanto a volte capitava che non si capiva un evento a quale componente era associato.

Nella nostra visita ai componenti GUI vedremo quindi come definirli, inizializzarli, inserirli in una finestra o in un applet, e quindi come gestirne gli eventi.  
Come già detto Java 2, ha due collezioni di packages per le interfacce grafiche, java.awt, già presente in Java 1, e javax.swing, uscita con Java 2, costruita sulle AWT, che amplia alla grande le  
possibilità per le interfacce grafiche.