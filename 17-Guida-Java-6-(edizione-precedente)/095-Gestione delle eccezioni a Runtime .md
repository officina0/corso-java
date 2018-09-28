Quando abbiamo programmato in ambiente multithread, uno degli aspetti più complessi da gestire è sempre stato quello di come comportarsi in eventuali casi eccezionali, senza risultare fatali al thread in esecuzione. Cosa avviene se un thread, nel metodo run, solleva una RuntimeException? Semplicemente il thread si interrompe passando la gestione dell’eccezione al ThreadGroup a cui appartiene. Per poter gestire quest’eccezione dobbiamo creare una estensione di ThreadGroup e gestirne tutto il codice relativo.

Tra le diverse modifiche apportate al modello di programmazione concorrente di Java c’è l’introduzione di un gestore di eccezioni da utilizzare nel caso in cui non volessimo ricorrere alla programmazione di un ThreadGroup a cui delegarne il compito, quindi definendo un comportamento di default per le eccezioni di una classe di Thread.

Java 1.5 introduce il metodo statico _Thread.setDefaultUncaughtExceptionHandler()_ che si aspetta come parametro un _Thread.UncaughtExceptionHandler_, un’interfaccia inner che dobbiamo implementare. Quando l’eccezione viene sollevata verrà richiamato il metodo _Thread.UncaughtExceptionHandler.uncaughtException()_, il metodo che dovremo implementare con il relativo codice di gestione.

Attraverso questo nuovo concetto riusciamo a gestire, a livello di singola classe, quello che prima avremmo dovuto gestire a livello di gruppo, con un evidente semplificazione anche a livello concettuale. Implementiamo un semplice Thread che effettua un compito banale e che nel caso di una particolare situazione sollevi un’eccezione a runtime nel metodo `run()`.

Listato 19.1. Effettua un semplice compito per poter sollevare un’eccezione e vedere come viene gestita

package it.html.threads;

public class ExceptionalThread extends Thread {

  private int iterations;  
    
  public ExceptionalThread(String threadName,int iterations){  
    setName(threadName);  
    this.iterations = iterations;  
    setDefaultUncaughtExceptionHandler(new ThreadException());  
  }  
    
  public void run(){  
    System.out.println(“Running #”+getName());  
    if(iterations<0)  
      throw new IllegalArgumentException(“Numero di iterazioni negative!”);  
      
    //altrimenti effettuiamo l’iterazione  
    for (int i=0;i<iterations;i++){  
      System.out.println(“Iterazione “+i+”) “+getName());  
    }  
  }  
}

La prima parte risulta piuttosto normale, unica cosa nuova è l’inizializzazione di un nuovo exception handler (`ThreadException`, di cui vedremo l’implementazione di seguito) ed il setting del medesimo attraverso il metodo statico _Thread.setDefaultUncaughtExceptionHandler()_.

Vediamo in concreto l’implementazione della classe (qui nella stessa unità di compilazione):

Listato 19.2. Definisce il comportamento da eseguire nel caso venga sollevata nel metodo run() di un Thread

class ThreadException implements Thread.UncaughtExceptionHandler{  
  @Override  
  public void uncaughtException(Thread arg0, Throwable arg1) {  
    System.out.println(arg0+” ha sollevato la seguente eccezione: “+arg1.getMessage());  
    //qui potremmo gestire un flusso eccezionale, cosa che prima non poteva essere fatta  
  }  
}

La classe deve implementare il metodo `uncaughtException()` e definire la logica eccezionale: sia essa la semplice stampa di un messaggio di log o una qualsiasi logica più o meno complessa. Ponete attenzione in quanto non è sufficiente creare la classe handler, ma bisogna anche settare (con il metodo setter statico) quale handler utilizzare. Provate a fare delle prove commentando il metodo `setDefaultHandler UncaughtException(...)` per controllare la differenza nell’esecuzione.

Listato 19.3. Classe di test per sollevare l’eccezione nel thread

package it.html.threads;

public class MainExceptionalThread {  
  public static void main(String[] args) throws InterruptedException {  
    //Definiamo un pò di thread, alcuni con iterazioni negative  
    ExceptionalThread et1 = new ExceptionalThread(“Thread 1”,10);  
    ExceptionalThread et2 = new ExceptionalThread(“Thread 2”,0);  
    ExceptionalThread et3 = new ExceptionalThread(“Thread 3”,-10);  
    ExceptionalThread et4 = new ExceptionalThread(“Thread 4”,3);  
    ExceptionalThread et5 = new ExceptionalThread(“Thread 5”,-1);  
      
    //Lanciamo i thread, et3 e et5 devono sollevare eccezione  
    et1.start();  
    et2.start();  
    et3.start();  
    et4.start();  
    et5.start();  
      
    //Aspettiamo che terminino  
    et1.join();  
    et2.join();  
    et3.join();  
    et4.join();  
    et5.join();  
      
    System.out.println(“Sessione terminata!”);  
  }  
}

Lanciamo una serie di thread, alcuni dei quali con parametri tali per sollevare l’eccezione. Il risultato, nel caso eccezionale, sarà l’esecuzione dell’handler e la relativa logica.