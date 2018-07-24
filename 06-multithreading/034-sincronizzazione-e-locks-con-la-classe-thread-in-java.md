# 034 Sincronizzazione E Locks Con La Classe Thread In Java

Nel precedente capitolo abbiamo visto come un Thread, dopo l’invocazione del metodo `start()`, non venga eseguito immediatamente ma transiti nello stato `Ready` pronto per l’esecuzione. Il Thread permane quindi in `Ready` fino alla sua selezione da parte dello Scheduler con conseguente transizione in `Running`. Un Thread nello stato `Running` ottiene il controllo della CPU e viene eseguito.

L’esecuzione di un Thread può interrompersi portandolo ad una transizione verso altri stati:

Stato

Descrizione

**Non Running states**

Sono gli stati blocked ,suspended e sleeping.

**Dead**

Il thread ha completato l’esecuzione del metodo run.

**Blocked**

Il Thread è in attesa di una risorsa occupata da un altro thread. Un esempio è il blocco di IO oppure attesa di acquisizione del lock su un oggetto.

**Suspended**

Il thread può transitare verso questo stato attraverso l’uso del metodo suspend\(\) della classe Thread. Questo metodo, cosi come resume\(\), è stato deprecato perchè può causare deadlock.

Ogni oggetto ha un lock che può essere controllato da un solo Thread. L’acquisizione del lock consente l’accesso al codice sincronizzato dell’oggetto, se il lock è libero il Thread corrente può acquisirlo, altrimenti, poichè il lock è sotto il controllo di un altro Thread, il Thread corrente transita nello stato `Seeking lock` in attesa di poter acquisire il lock sulla risorsa condivisa.

Quando il Thread in possesso del lock esce dal codice sincronizzato il lock viene rilasciato. Uno dei Thread in Seeking lock acquisisce il lock ora libero e transita nello stato `Ready`.

La sincronizzazione del codice condiviso può avvenire in due modi: il primo consiste nel sincronizzare un intero metodo attraverso il modificatore `syncronized` nella dichiarazione del metodo; in questo caso un Thread deve acquisire il lock sull’oggetto.

Il secondo prevede di sincronizzare un blocco di codice all’interno di un metodo con l’espressione:

```text
                syncronized(object){
                        ..
                }
```

In questo caso il lock viene ottenuto soltanto su un blocco di codice. I metodi alla base della sincronizzazione sono: `wait()`, `notify()` e `notifyAll()` della classe `Object` utilizzabili solo all’interno di codice sincronizzato. Un Thread in possesso del lock, può invocare il metodo `wait()` per passare allo stato `Waiting` in attesa di notifiche da parte di altri Thread per poter continuare il suo lavoro sulla risorsa condivisa.

Il Thread `Waiting` sarà risvegliato, successivamente dall’invocazione di `notify()`, `notifyAll()` o da un interrupt da parte del Thread correntemente in possesso del lock.

Quando si usa `notify()` non è possibile scegliere quale thread risvegliare dal pool di thread in stato `Waiting`, lo scheduler ne sceglie uno autonomamente. Il Thread che esce dallo stato `Waiting` passa nello stato Seeking lock per poter riacquisire il lock.

Vediamo un esempio di sincronizzazione tra due Thread che implementi il classico schema produttore-consumatore. Realizziamo una classe che si comporti da risorsa condivisa:

public class CubbyHole {

private boolean empty = true;

public boolean isEmpty\(\) { return empty; } public void setEmpty\(boolean empty\) { this.empty = empty; }

}

Essa implementa un contenitore nel quale il Thread `Producer` inserirà qualcosa e dal quale il Thread `Consumer` leggerà il contenuto. Continuiamo con il Thread Producer:

public class Producer extends Thread {

```text
private final CubbyHole cubbyHole;

public Producer(CubbyHole cubbyHole){
       this.cubbyHole = cubbyHole;
}

@Override
public void run() {
    while (!isInterrupted()) {
        synchronized (cubbyHole) {
            if (cubbyHole.isEmpty()) {
                cubbyHole.setEmpty(false);
                System.out.println("Producer: Inserisco qualcosa nel contenitore");
            }
            cubbyHole.notifyAll();
            try {
                cubbyHole.wait();
            } catch (InterruptedException ex) {
                interrupt();
            }
        }
    }
    System.out.println("Producer: interruzione");
}
```

}

Il `Producer` riceve l’oggetto contenitore, in esecuzione entra poi in un loop dal quale esce solo dopo la ricezione di un interrupt. All’interno del loop si tenta di acquisire il lock sul contenitore, si cambia il valore della variabile booleana `empty` per simulare un inserimento, si invoca `notifyAll()` per notificare il rilascio imminente del lock ai Thread in attesa e, infine, si invoca il metodo `wait()` per passare nello stato `Waiting`.

Il Producer ha la stessa struttura, la differenza risiede nell’uso dello stesso oggetto condiviso:

public class Consumer extends Thread{

```text
private final CubbyHole cubbyHole;

public Consumer(CubbyHole cubbyHole){
   this.cubbyHole = cubbyHole;
  }

@Override
public void run() {
    while (!isInterrupted()) {
        synchronized (cubbyHole) {
            if (!cubbyHole.isEmpty()) {
                cubbyHole.setEmpty(true);
                System.out.println("Consumer: Prelevo qualcosa dal contenitore");
            }
            cubbyHole.notifyAll();
            try {
                cubbyHole.wait();
            } catch (InterruptedException ex) {
                if (!cubbyHole.isEmpty()) {
                    cubbyHole.setEmpty(true);
                }
                interrupt();
            }
        }
    }
    System.out.println("Consumer: interruzione");
}
```

}

Realizziamo infine la classe `Demo` che istanzi i due oggetti Thread, li faccia dialogare per alcuni secondi e invii l’interrupt per arrestare la simulazione:

public class ThreadLockDemo { public static void main\(String\[\] args\) {

```text
    CubbyHole cubbyHole = new CubbyHole();

    Producer producer = new Producer(cubbyHole);
    Consumer consumer = new Consumer(cubbyHole);

    producer.start();
    consumer.start();

    try {
        Thread.sleep(10);
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    }

    producer.interrupt();
    consumer.interrupt();

    try {
        producer.join();
        consumer.join();
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    }
}
```

}

Eseguendo il codice dovremmo ottenere un output come il seguente:

Producer: Inserisco qualcosa nel contenitore Consumer: Prelevo qualcosa dal contenitore Producer: Inserisco qualcosa nel contenitore Consumer: Prelevo qualcosa dal contenitore Producer: Inserisco qualcosa nel contenitore Consumer: Prelevo qualcosa dal contenitore .. Producer: interruzione Consumer: interruzione

Il metodo `join()` consente di far attendere al Thread principale la terminazione di `Producer` e `Consumer`.

