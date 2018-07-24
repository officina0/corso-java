In questo capitolo affrontiamo il problema dell’**accesso concorrente a risorse condivise** attraverso le nuove **API Java 8**. Riprendiamo l’esempio del Produttore-Consumatore ed implementiamolo attraverso `ExecutorService` e la classe `ReentrantLock` che permette un lock dal funzionamento simile a quanto visto con la parola chiave `synchronized`. Iniziamo col modificare la classe `CubbyHole` che rappresenta la nostra risorsa condivisa in modo tale che estenda `ReentrantLock`:

public class CubbyHole extends ReentrantLock { ... }

Procediamo realizzando un _Consumer_ attraverso implementazione della classe `Runnable`:

public class ProducerRunnable implements Runnable {
	
	private final CubbyHole cubbyHole;

	public ProducerRunnable(CubbyHole cubbyHole){
		   this.cubbyHole = cubbyHole;
	}

	@Override
	public void run() {
		while (!Thread.currentThread().isInterrupted()) {
		  cubbyHole.lock();	
		  try {
			  if (cubbyHole.isEmpty()) {
						cubbyHole.setEmpty(false);
						System.out.println("Producer: Inserisco qualcosa nel contenitore");
			  }
		  } finally {
			cubbyHole.unlock();
		  }
		}
		System.out.println("Producer: interruzione");
	}
}

Il metodo `run()` cambia la sua implementazione rispetto a quanto visto in precedenza, in esso notiamo infatti il differente tentativo di acquisizione del `lock()` sull’oggetto `ReentrantLock` condiviso con il successivo _Consumer_.

Se l’oggetto non è bloccato, perchè il lock è correntemente acquisito da un altro Thread, l’acquisizione del lock procede, viene quindi eseguito il codice seguente e rilasciato infine il lock nel blocco `finally`.

Nel caso in cui il lock sia in possesso di un Thread concorrente, il Thread corrente si blocca in attesa dell’acquisizione e non è eleggibile per l’esecuzione fino a quando il lock non viene rilasciato. Continuiamo con l’implementazione del _Consumer_ attraverso la stessa struttura di codice:

public class ConsumerRunnable implements Runnable{
	
	private final CubbyHole cubbyHole;

	public ConsumerRunnable(CubbyHole cubbyHole){
	   this.cubbyHole = cubbyHole;
	  }

	@Override
	public void run() {
		while (!Thread.currentThread().isInterrupted()) {
			cubbyHole.lock();
			try {
				if (!cubbyHole.isEmpty()) {
						cubbyHole.setEmpty(true);
						System.out.println("Consumer: Prelevo qualcosa dal contenitore");
				}
			} finally {
				cubbyHole.unlock();	
			}
	    }
		System.out.println("Consumer: interruzione");
	}
}

Realizziamo infine una classe demo `ReentrantLockDemo`, che definisca un _Executor_ con due Thread: un _Producer_ e un _Consumer_. Il codice all’interno del metodo `main` della classe demo è il seguente:

   ...
   CubbyHole cubbyHole = new CubbyHole();
		
   ProducerRunnable producer = new ProducerRunnable(cubbyHole);
   ConsumerRunnable consumer = new ConsumerRunnable(cubbyHole);
		
   ExecutorService executor = Executors.newFixedThreadPool(2);
   executor.submit(producer);
   executor.submit(consumer);

   try {
     Thread.sleep(10);
   } catch (InterruptedException ex) {
	 ex.printStackTrace();
   }
   
   executor.shutdownNow();
   System.out.println("Shutdown completato");
   ...

Evidenziamo la registrazione degli oggetti _producer_ e _consumer_, la loro esecuzione per 10 ms, e lo shutdown dell’_Executor_ che arresta i Thread coinvolti. L’esecuzione della classe demo mostrerà in console lo stesso risultato visto nel precedente capitolo.

Esistono altre classi utilizzabili per l’implementazione di un lock, particolarmente interessante è l’interfaccia `ReadWriteLock` che specifica un lock per la lettura ed uno per la scrittura, l’idea alla base di esso è che l’accesso multiplo in lettura ad una risorsa condivisa non necessita di essere Thread safe, cosi una molteplicità di Thread può acquisire contemporaneamente un lock di lettura se non è attivo un lock di scrittura sulla risorsa.

L’uso del lock di tipo ReadWriteLock è una soluzione particolarmente efficiente quando le scritture sono poco frequenti e le letture, al contrario, avvengo con molta frequenza. Vediamo un esempio di creazione di un oggetto di questo tipo:

ReadWriteLock lock = new ReentrantReadWriteLock();

Acquisizione del lock di tipo scrittura:

lock.writeLock().lock();

Il suo rilascio:

lock.writeLock().unlock();

Ed analogamente al lock di tipo lettura:

lock.readLock().lock();
lock.readLock().unlock();