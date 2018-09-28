Di RMI (Remote Method Invocation) abbiamo discusso nella [guida J2EE](http://java.html.it/guide/lezione/3486/remote-method-invocation/ "RMI guida a J2EE") e ne abbiamo parlato a livello teorico, essendo la base su cui la tecnologia distribuita di Java (e quindi anche gli EJB) si basa. Il modello teorico non cambia di una virgola con le ultime release di Java, quello che cambia è l’implementazione che si avvale di uno strumento, il Dynamic Proxy, che evita allo sviluppatore la noia di dover creare a tempo di compilazione gli stub e gli skeleton.

Questa possibilità ci è data dall’introduzione della classe _java.lang.reflect.Proxy_ a partire dalla versione Java 1.3. L’implementazione di RMI è però cambiata solo nella release 1.5 dove, appunto, utilizzando la classe Proxy viene creata dinamicamente l’infrastruttura di stub e skeleton che prima dovevamo definire “a mano” utilizzando il compilatore rmic. Lo sviluppatore di applicazioni RMI, sicuramente, sa quanto vale questa modifica, in quanto lo solleva dal compito di mantenere queste interfacce e modificarle ad ogni cambio, ricompilandole e rieseguendo il tutto. Se poi pensate ad un ambiente distribuito dove una macchina server serve centinaia di client, capite quanto realmente sia una miglioria.

La classe Proxy su cui la generazione dinamica RMI si basa è una classe piuttosto semplice e il concetto alla base è quello di associare un oggetto di tipo _java.lang.reflect.InvocationHandler_ (un’interfaccia) che si occuperà di effettuare la logica che vogliamo controllare in maniera dinamica. In questo caso tutto è gestito a runtime, grazie alla reflection che si occuperà di creare uno stub lato client da connettersi allo skeleton lato server, in maniera del tutto trasparente (effettuando sempre lo scambio di messaggi su protocollo TCP/IP).

Noi alla fine non ci accorgeremo di nulla, il nostro compito sarà sempre quello di definire la classe “server” e di installarla su una macchina e poi utilizzare quell’istanza richiamandola attraverso un registro RMI. Nell’esempio lo vediamo benissimo:

package it.html.java5.rmi.generic;

import java.rmi.Remote;  
import java.rmi.RemoteException;  
public interface RemoteEchoExecutor extends Remote {  
  //definiamo il comportamento dell’interfaccia attraverso i metodi  
  public String echo(String textToEcho)throws RemoteException;  
}

Innanzittutto definiamo l’interfaccia del servizio da realizzare, preoccupandoci solo di definirla Remote. Con l’interfaccia definiamo i metodi da realizzare.

Segue l’implementazione del server, che non deve fare null’altro che definire l’implementazione del servizio.

import it.html.java5.rmi.generic.RemoteEchoExecutor;  
import java.rmi.RemoteException;  
import java.rmi.server.UnicastRemoteObject;

public class EchoExecutorServer extends UnicastRemoteObject implements RemoteEchoExecutor {  
  public EchoExecutorServer() throws RemoteException {  
    super();  
  }

  //Implementiamo il metodo dell’interfaccia: l’esecuzione avviene sul server  
  public String echo(String textToEcho) throws RemoteException {  
    System.out.println(“Someone ask to echo: “+textToEcho);  
    return textToEcho.toUpperCase();  
  }  
}

Sarà la classe **UnicastRemoteObject** a preoccuparsi di definire il proxy per effettuare lo scambio di informazioni tra client e server, secondo la logica di proxy dinamico detta in precedenza.

L’istanza della classe dovrà essere lanciata ed eseguita su un server, per fare ciò abbiamo bisogno di un registro per pubblicare il servizio che il client interrogherà (in questo modo superiamo il problema dell’accoppiamento tra client e server).

package it.html.java5.rmi.registry;  
import it.html.java5.rmi.server.EchoExecutorServer;  
import java.rmi.Naming;  
import java.rmi.registry.LocateRegistry;

public class RegistryStart {  
  public static void main(String[] args) throws Exception {  
    int registryPortNumber = 1099;  
    // Start RMI registry  
    LocateRegistry.createRegistry(registryPortNumber);  
    // Effettuiamo il bind  
    Naming.rebind(“ECHOSERVER”, new EchoExecutorServer());  
    System.out.println(“Server running…”);  
  }  
}

Una volta avviato il main, qualsiasi client potrà utilizzare la classe remotamente come nell’esempio che segue:

package it.html.java5.rmi.client;  
import it.html.java5.rmi.generic.RemoteEchoExecutor;  
import java.rmi.Naming;

public class EchoClientit {  
  public static void main(String[] args) throws Exception {  
    String host = “localhost”;  
    int portNumber = 1099;  
    String lookupName = “//” + host + “:” + portNumber + “/” + “ECHOSERVER”;  
    RemoteEchoExecutor executor = (RemoteEchoExecutor)Naming.lookup(lookupName);

    String result = executor.echo(“Hello html.it user!”);  
    System.out.println(“Task executed for ” + result);  
  }  
}

Unica cosa a cui dobbiamo prestare attenzione è il fatto che, sia il client che il server, condividono l’interfaccia RemoteEchoExecutor, quindi entrambe le JVM devono averla sotto il proprio classpath.

Questa che abbiamo descritto è la principale miglioria che potete sfruttare a partire da Java 1.5. Una piccola modifica è stata introdotta in Java 1.6, rendendo generica la classe `MarshalledObject<T>`: questa classe è quella che si occupa della serializzazione/deserializzazione dei parametri.