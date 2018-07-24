Così com’è la classe Callable non ha molta potenza, in quanto da sola non fa nessun tipo di esecuzione concorrente. Attraverso l’uso delle classi **Future** e **FutureTask**, però, possiamo pensare di inserire l’esecuzione in un oggetto che viene eseguito (parallelamente) e restituisce il risultato quando ci serve (attendendo la fine del task se questo non è completato).

Già in uno dei capitoli precedenti abbiamo parlato della classe **ExecutorService**, da utilizzare per il controllo del flusso dei thread. Utilizzando il metodo `submit()` della classe, passando un oggetto di tipo `Callable<T>`, avremo un oggetto `Future<T>` che potremo utilizzare per l’esecuzione concorrente di un’attività e recuperarne il risultato interrogando il metodo `get` (che restituisce un istanza di T) sul Future.

Detto così potrebbe suonare complicato. In realtà, modificando il metodo main visto nella precedente lezione, vedremo come poter effettuare la stessa operazione di ricerca ma in maniera parallela:

public static void main(String args\[\]) throws InterruptedException{  
  //Definiamo un executor  
  ExecutorService executor = Executors.newSingleThreadExecutor();  
    
  //Creiamo un task asincrono  
  CallableTask task = new CallableTask(“games”);  
  //e lo wrappiamo in un oggetto future  
  Future<Collection<String>> result = executor.submit(task);  
    
  System.out.println(“Search result (dovrebbero passare 5 secondi prima di vedere il risultato…):”);  
  //…nel frattempo possiamo fare altri compiti, come preparare un layout di presentazione  
  System.out.println(“#————- games found —- ++”);  
  //prendiamo il risultato, e se non è stato eseguito attendiamo  
  try {  
    showResult(result.get());  
  } catch (ExecutionException e) {  
    System.out.println(“Eccezione nell’esecuzione della ricerca”);  
  }  
  System.out.println(“… fine della computazione”);  
}

Per prima cosa abbiamo creato un ExecutorService sul quale richiamare il Callable creato. Il callable è parametrizzato come `<Collection<String>>`, quindi il tipo restituito dall’executor sarà un future con il medesimo parametro, il cui metodo `get()` restituirà quel parametro.

Come vediamo, l’uso di generics qui consente uno strettissimo controllo dei tipi già a tempo di compilazione, il che rende il processo di sviluppo molto meno incline a bug dovuti a casting.

Nel momento in cui eseguiamo il task, questo inizia la sua esecuzione parallela (in questo caso abbiamo simulato cinque secondi di esecuzione). Il main, però, procede senza bloccarsi su di esso, quindi prepariamo un ipotetico layout grafico per contenere i risultati (e magari effettuiamo altre operazioni, come un aggiornamento ad una tabella di statistiche per sapere le ricerche effettuate). Nel momento in cui chiamiamo il metodo `get()` su FutureTask, se il risultato è disponibile allora verrà mostrato, altrimenti ci sarà un attesa fino alla sua completa esecuzione.

Interessante è la possibilità di utilizzare il metodo `get(long,TimeUnit)` che ci consente di settare un timeout di attesa, oltre il quale l’attività è cancellata (utile ad esempio in contesti realtime o per attività con deadline). In generale vi consiglio di dare un’occhiata alla [API delle classi](http://java.sun.com/j2se/1.5.0/docs/api/java/util/concurrent/package-summary.html "API delle Classi - Link esterno") per meglio capire i dettagli di questi nuovi concetti di concorrenza in Java.