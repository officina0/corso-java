La **programmazione concorrente** è sicuramente una delle migliori caratteristiche di questo linguaggio in quanto consente di avere una forma di programmazione multiprocesso “leggera”, nel senso che i Thread di Java condividono lo stesso spazio di memoria e il tempo per fare il content switching (passaggio di contesto dall’esecuzione di un Thread ad un altro) è minore rispetto ai processi tradizionali. Unico neo è quello di avere la logica di business accoppiata con il ciclo di vita e quindi la logica di esecuzione dello stesso Thread.

Ora vediamo come da Java 1.5 sia possibile disaccoppiare le due cose. Le nuove interfacce e classi di Java permettono di ottenere dei servizi migliori nello sviluppo di programmi concorrenti. Una di queste è l’interfaccia Executor, il cui compito è quello di definire un flusso di esecuzione di task (thread java) e di poterne controllare la stessa esecuzione.

Attraverso l’estensione di _java.util.concurrent.Executor_ ed ExecutorService (che ha un’interfaccia molto più fornita) possiamo definire il ciclo di esecuzione di un generico Thread ma, per gli obiettivi più comuni, utilizzeremo i metodi statici di _java.util.concurrent.Executors_ che ci consentono l’accesso a degli Executor standard, che sono:

*   Single Thread Executor
*   Fixed Thread Executor
*   Cached Thread Executor
*   Scheduled Thread Executor

Attraverso l’uso di quest’interfaccia potremo eseguire un set fisso di thread (ad esempio un server che accetta delle connessioni) o altri interessanti combinazioni di logica con l’uso delle classi Callable e Future.

Listato 21.1. Esempio di un server che effettua un servizio tramite socket

package it.html.threads;  
public class EchoServer {  
    
  public static void main(String\[\] args) {  
    ServerSocket server = null;  
      
    //Avviamo il server  
    try {  
      server = new ServerSocket(6666);  
    } catch (IOException e) {  
      System.out.println(“Eccezione all’avvio del server: “+e.getMessage());  
      System.exit(-1);  
    }  
      
    //Creo un pool di thread concorrenti  
    ExecutorService executor = Executors.newFixedThreadPool(5);  
      
    //Aspettiamo una nuova connessione cui deleghiamo l’esecuzione  
    //all’executor service definito in precedenza  
    while(true){  
      try{  
        System.out.println(“Waiting incoming connection…”);  
        Socket incoming = server.accept();  
        executor.execute(new EchoThread(incoming));  
      }catch(Exception e){  
        executor.shutdown();  
      }  
    }  
  }  
}

Il main definisce un ServerSocket che attende connessioni all’infinito. Su ogni connessione creiamo un thread (che definiamo in seguito) per gestire il servizio. In precedenza avevamo definito un Executor di cinque elementi, quindi stiamo definendo una coda di esecuzione di al massimo cinque elementi da eseguire concorrentemente. Anziché effettuare l’esecuzione (con il classico metodo `start()`) qui deleghiamo l’esecuzione all’executor service, ignorando come questo gestisca la logica di avvio.

Nel caso avvenga un’eccezione utilizziamo il metodo `shutdown()` che effettua la chiusura di tutti i thread che si stanno eseguendo. Segue il codice del thread:

class EchoThread implements Runnable{  
  Socket socket;  
  public EchoThread(Socket socket){  
    this.socket = socket;  
  }  
  public void run(){  
    System.out.println(“Incoming connection: “+socket.getInetAddress());  
    //Continua con la lettura/scrittura sulla socket aperta…  
  }  
}

Abbiamo lasciato il servizio blank, visto che non rientra ai fini della discussione. A questo punto vediamo anche un semplice client per effettuare l’esecuzione di un thread (voi potete modificarlo aggiungendo più chiamate concorrenti).

package it.html.threads;  
  
import java.io.IOException;  
import java.net.Socket;  
import java.net.UnknownHostException;  
  
public class EchoClient {  
  /**  
   \* @param args  
   \* @throws IOException  
   \* @throws UnknownHostException  
   */  
  public static void main(String\[\] args) throws UnknownHostException, IOException {  
    Socket socket = new Socket(“127.0.0.1”,6666);  
    System.out.println(“Connected!”);  
    //… fa qualcosa con lo stream  
  }  
}