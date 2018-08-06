Osservando il package _java.util.concurrent_ potete notare le tante nuove interfacce e classi che vi sono contenute. Finora abbiamo discusso di alcune di loro, come gli Executor e i Future, cioè di come sviluppare gli aspetti di logica di ogni singolo thread. Accanto a queste novità gli ingegneri della Sun hanno curato anche l’aspetto della gestione della concorrenza, attraverso delle novità per i meccanismi di accesso in mutua esclusione, laddove necessario. Qui, ci occuperemo di una nuova classe che introduce uno strumento molto importante nella programmazione concorrente, il semaforo.

Un semaforo è una delle strutture di controllo di concorrenza più semplici ed importanti, in quanto permette di dare, o meno, l’accesso a una risorsa condivisa, garantendo mutua esclusione o accesso condiviso a un numero limitato di clienti. Di fatto, un semaforo (classe _java.util.concurrent.Semaphore_) si comporta come un semaforo stradale, dando l’accesso ad alcuni, e negando l’accesso ad altri.

Finora l’implementazione doveva essere fatta da ognuno che ne avesse l’esigenza e pur trattandosi di una classe banale (tipico esercizio di programmazione concorrente) non ha senso avere decine di diverse implementazioni. La classe si presenta con i metodi `acquire()` e `release()`, dove il richiedente si pone in attesa sul metodo `acquire()` fino a che una risorsa non viene rilasciata da un altro cliente (con il metodo `release()`). Ci sono altri interessanti metodi, come il `tryAcquire()` dove il richiedente si pone in attesa per un limite di tempo e poi esce (senza acquisire la risorsa).

Il tipico uso che si fa di questa classe è quello del pool, dove vi sono una serie di oggetti condivisi (connessioni, transazioni, ecc) e un gestore che si occupa di assegnarli in base alle richieste e alla disponibilità del pool.

Preseguiamo con un esempio concreto per mostrare l’uso della classe. Come contesto scegliamo una simpatica simulazione dove la risorsa condivisa è una festa (con un numero di posti liberi limitato) e un numero di invitati maggiore. Per rendere la cosa più veritiera abbiamo alcuni invitati che per pigrizia attendono l’ingresso ma non oltre un certo tempo, dopodiché lasciano perdere.

Listato 24.1. Definisce la virtualizzazione di una risorsa condivisa, in questo caso un party

package it.html.threads.semaphore;  
  
import java.util.concurrent.Semaphore;  
import java.util.concurrent.TimeUnit;  
  
public class Party {  
  
  //Dimensione massima della sala: quante persone al massimo riusciamo a servire  
  int dimensione;  
    
  //Il semaforo che controllerà gli accessi  
  Semaphore semaphore;  
    
  public Party(int dim){  
    this.dimensione = dim;  
    //creiamo il semaforo con il numero massimo di accessi  
    this.semaphore = new Semaphore(dimensione);  
  }  
    
  public boolean goIn(){  
    try{  
      semaphore.acquire();  
      if (semaphore.availablePermits()==0)  
        System.out.println(“Please wait… nessun posto disponibile!”);  
      return true;  
    }catch(InterruptedException e){  
      e.printStackTrace();  
    }  
    return false;  
  }  
    
  public boolean goIn(long time){  
    try{  
      boolean in = semaphore.tryAcquire(time, TimeUnit.SECONDS);  
      if (semaphore.availablePermits()==0)  
        System.out.println(“Please wait… nessun posto disponibile!”);  
      return in;  
    }catch(InterruptedException e){  
      e.printStackTrace();  
    }  
    return false;  
  }    
    
  public void goOut(){  
    semaphore.release();  
    System.out.println(“Uno meno! Posti liberi… “+semaphore.availablePermits());  
  }  
}

La classe Party simula la festa, quindi ha un numero di posti disponibile e un semaforo con cui cureremo gli aspetti di accesso concorrente. Attraverso i metodi `goIn()` i richiedenti avranno accesso all’ingresso, con il metodo `goOut()` usciranno, liberando il posto.

Le classi che accedono alla festa sono le classi Invited e LazyInvited che eredita dalla prima:

Listato 24.2. Simulazione del “traffico” alla festa, con ingressi e uscite

package it.html.threads.semaphore;  
  
import java.util.concurrent.TimeUnit;  
  
public class Invited extends Thread {  
  String name;  
  Party party;  
    
  public Invited(String name, Party party){  
    this.name = name;  
    this.party = party;  
  }  
    
  //Facciamo un ciclo infinito di ingressi, attese, uscite, attese, …  
  public void run(){  
    while(true){  
      System.out.println(“\[“+name+”: \] Sono in coda. Aspetto.”);  
      party.goIn();  
      System.out.println(“\[“+name+”: \] Yuppy! Sono dentro.”);  
      try{  
        //Si diverte un po’ alla festa  
        int timeToSleep = (int) (Math.random()*10);  
        System.out.println(“\[“+name+”: \] rimarrò “+timeToSleep+” secs.”);  
        TimeUnit.SECONDS.sleep(timeToSleep);  
      }catch(InterruptedException e){  
        e.printStackTrace();  
      }  
      //decide di uscire…  
      party.goOut();  
      System.out.println(“\[“+name+”: \] Esco a prendere aria.”);  
      try{  
        //sta un po’ fuori…  
        int timeToSleep = (int) (Math.random()*10);  
        TimeUnit.SECONDS.sleep(timeToSleep);  
      }catch(InterruptedException e){  
        e.printStackTrace();  
      }  
    }  
  }  
}

La vita di un invitato è quella di provare ad entrare nella festa (acquisendo il diritto ad accedere), divertirsi un po’ (simulato da uno sleep random), uscire (rilasciando il diritto di accesso ad altri), riposarsi un po’ (sleep random) e provare a ritornare dentro.

La coppia di metodi `goIn()` e `goOut()` ci assicura la consistenza dell’azione globale.

Listato 24.3. Gestione della vita dell’invitato

package it.html.threads.semaphore;  
import java.util.concurrent.TimeUnit;  
  
public class LazyInvited extends Invited {  
  public LazyInvited(String name, Party party) {  
    super(name, party);  
  }  
  
  //Facciamo un ciclo infinito di ingressi (con attesa limitata), attese, uscite, attese  
  public void run(){  
    while(true){  
      System.out.println(“\[“+name+”: \] Sono in coda. Aspetterò un po’ (5 sec).”);  
      boolean in = party.goIn(5);  
      if (in){  
        System.out.println(“\[“+name+”: \] Yuppy! Sono dentro.”);  
        try{  
          //Si diverte un po’ alla festa  
          int timeToSleep = (int) (Math.random()*10);  
          System.out.println(“\[“+name+”: \] rimarrò “+timeToSleep+”  
secs.”);           TimeUnit.SECONDS.sleep(timeToSleep);  
        }catch(InterruptedException e){  
          e.printStackTrace();  
        }  
        //decide di uscire…  
        party.goOut();  
        System.out.println(“\[“+name+”: \] Esco a prendere aria.”);  
      }else{  
        //timeout di attesa scaduto!  
        System.out.println(“\[“+name+”: \] Basta! mi sono rotto, esco.”);  
      }  
        
      try{  
        //sta un po’ fuori…  
        int timeToSleep = (int) (Math.random()*10);  
        TimeUnit.SECONDS.sleep(timeToSleep);  
      }catch(InterruptedException e){  
        e.printStackTrace();  
      }  
    }  
  }  
}

Un invitato “lazy” (pigro) ha lo stesso ciclo di vita di un invitato normale, con l’eccezione che dopo 5 secondi di fila si annoia ed esce dalla coda. In questo caso il metodo `goIn(millis)` effettua un `tryAcquire()` che, passato il timeout, si risolve senza l’accesso lasciando libero però l’invitato di fare altro.

Per vedere il tutto all’opera creiamo un main e seguiamo l’evoluzione della vita mondana attraverso il testo di output:

Listato 24.4. Simulazioni inviti e ingressi

package it.html.threads.semaphore;  
  
public class MainPR {  
  //Simulazione del PR che invita la gente al party  
  public static void main(String\[\] args) {  
    //Definiamo il party di 20 persone concorrenti  
    Party party = new Party(20);  
      
    //creiamo 40 invitati  
    Invited inLista\[\] = new Invited\[40\];  
      
    //metà saranno lazy, gli altri no  
    for(int i=0;i<inLista.length;i++){  
      Invited tmp;  
        
      if (i%2==0)  
        tmp = new Invited(“NotLazy#”+i, party);  
      else  
        tmp = new LazyInvited(“Lazy#”+i, party);  
          
      inLista\[i\]=tmp;  
    }  
      
    for(int i=0;i<inLista.length;i++){  
      inLista\[i\].start();  
    }  
  }  
}