# 078 Sequential Vs Parallel Programming

Abbiamo accennato al fatto che gli Stream possono essere sequenziali o paralleli, ed abbiamo visto esempi di Stream sequenziali. Le operazioni su Stream sequenziali sono realizzate attraverso un singolo Thread, mentre gli Stream paralleli eseguono operazioni attraverso un insieme di Thread concorrenti.

Iniziamo con il vedere un semplice esempio didattico che confronta le prestazioni di uno Stream sequenziale con uno parallelo attraverso l’operazione di ordinamento di una lista di interi realizzata utilizzando un `ArrayList`:

ArrayList integers = new ArrayList\(\); for\(int i=MAX; i&gt;=1; i--\) { integers.add\(i\); }

Proseguiamo definendo il codice per uno Stream sequenziale di ordinamento degli interi presenti nella lista:

long start = System.nanoTime\(\);

integers.stream\(\).sorted\(\);

long end = System.nanoTime\(\);

System.out.println\("Sequential sort time:"+\(end-start\)\);

Siamo interessati a valutare il tempo impiegato dagli Stream per eseguire l’operazione di ordinamento, quindi calcoliamo gli istanti di tempo che precedono e seguono l’operazione applicata allo Stream. Uno Stream parallelo si realizza utilizzando il metodo `parallelStream()` al posto del metodo `stream()`:

start = System.nanoTime\(\);

integers.parallelStream\(\).sorted\(\);

end = System.nanoTime\(\);

System.out.println\("Parallel sort time:"+\(end-start\)\);

Se eseguiamo il codice nel _main_ di una classe Java notiamo la stampa di tempi simili ai seguenti:

Sequential sort time:6377809 Parallel sort time:97463

Questo risultato mostra che esistono situazioni in cui possiamo realizzare un’operazione con un costo di tempo notevolmente inferiore utilizzando uno Stream parallelo, anche se gli Stream paralleli non offrono sempre migliori performance.

Come ulteriore esempio di confronto Stream sequenziale-parallelo, consideriamo un caso nel quale abbiamo la necessità di inviare in modo massivo delle email e supponiamo che il quantitativo di email sia dell’ordine di diverse migliaia. Creiamo all’interno della classe dimostrativa una classe privata che simuli in modo molto semplice un MailSender, desideriamo soltanto che il suo metodo `sendMail()` perda del tempo simulando l’invio di una mail:

private static class MailSender { private static void sendMail\(String address\) { int i=10000; while\(i&gt;=1\) i--; } }

Successivamente creaimo un’`ArrayList` di stringhe che contenga ipotetici indirizzi mail:

ArrayList mails = new ArrayList\(\); for\(int i=MAX; i&gt;=1; i--\) { mails.add\("Mail"+i\); }

Definiamo quindi uno Stream sequenziale per l’invio delle mail:

long start = System.nanoTime\(\);

mails.stream\(\).forEach\(MailSender::sendMail\);

long end = System.nanoTime\(\);

System.out.println\("Sequential sort time:"+\(end-start\)\);

ed uno equivalente che sfrutta il parallelismo:

start = System.nanoTime\(\);

mails.parallelStream\(\).forEach\(MailSender::sendMail\);

end = System.nanoTime\(\);

System.out.println\("Parallel sort time:"+\(end-start\)\);

Se eseguiamo l’applicazione demo otteniamo sulla console di output la stampa di valori simili ai seguenti:

Sequential sort time:152922649 Parallel sort time:75702713

Ancora una volta notiamo un comportamento più performante affrontando il problema attraverso l’uso di uno Stream parallelo.

Concludiamo il capitolo con una rapida digressione verso le Mappe che, come detto in precedenza, non supportano gli Stream. Le mappe sono state dotate di nuovi ed utili metodi per il supporto alle operazioni più comuni. Ad esempio possiamo inserire un elemento in una mappa se questo non è presente attraverso una singola istruzione evitando test di verifica preliminare:

Map map = new HashMap&lt;&gt;\(\); map.putIfAbsent\(1, "NewValue"\);

Eseguire funzioni sugli elementi di una mappa:

map.computeIfPresent\(1, \(num, val\) -&gt; val + num\);

Che aggiorna il valore stringa a `NewValue1` per l’elemento di chiave 1. Oppure iterare su una Mappa attraverso una singola istruzione:

map.forEach\(\(id, val\) -&gt; System.out.println\(val\)\);

