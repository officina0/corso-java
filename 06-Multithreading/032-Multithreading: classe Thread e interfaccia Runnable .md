In questo capitolo vedremo le due diverse forme base per la creazione e l’esecuzione di un **Thread** ed i possibili stati in cui un Thread può trovarsi nel suo ciclo di vita.

Classe Thread
-------------

Iniziamo con l’analizzare la classe `Thread`, definire un Thread attraverso questa classe significa semplicemente realizzare una classe che estende `Thread` e che faccia override del metodo `run()` come nel seguente esempio:

public class ThreadA extends Thread{
  @Override
  public void run(){
   System.out.println("Codice del thread A");
  }
}

Per creare il Thread istanziamo un oggetto della classe ed invochiamo su di esso il metodo `start()`:

public class DemoThreadA {
   public static void main(String\[\] args) {
	 ThreadA threadA = new ThreadA();
	 threadA.start();
   }
}

L’invocazione del metodo `start()` non comporta l’immediata esecuzione del Thread, ma la richiesta di registrazione del Thread presso il **Thread Scheduler**, il quale può essere parte della JVM oppure del sistema operativo. Invocare il metodo `start()` equivale quindi a rendere il Thread eleggibile per l’esecuzione.

Il Thread Scheduler svolge il lavoro di determinare quale Thread deve essere in esecuzione e su quale delle CPU disponibili. Il Thread verrà quindi eseguito quando lo Scheduler lo selezionerà dalla coda dei Thread in stato **Ready**. La sua esecuzione avrà come risultato l’invocazione del metodo `run()`.

Interfaccia Runnable
--------------------

Da un punto di vista della programmazione object oriented, se definiamo un Thread attraverso la classe `Thread` diciamo che la nostra classe rappresenta effettivamente un Thread.

Un modo alternativo di creare un Thread, consiste nell’utilizzo dell’**interfaccia Runnable**:

public class ThreadB implements Runnable{
	public void run(){
		System.out.println("Codice del thread B");
	}
}

In questo caso per definire un Thread occorre un’istanza della classe `Thread` utilizzando il costruttore che accetta un riferimento all’interfaccia `Runnable`:

public class DemoThreadB {
	public static void main(String\[\] args) {
		Thread thread = new Thread(new ThreadB());
		thread.start();
	}
}