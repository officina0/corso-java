Uno dei problemi che spesso gli sviluppatori hanno è quello relativo alla gestione della formattazione del testo. Infatti, come sappiamo, la rappresentazione può differire in base alle diverse esigenze che possiamo avere nello sviluppo di un programma.

Seppure banali e ripetitive, queste situazioni, quando si manifestano, mettono a dura prova il miglior programmatore. Altro problema è legato al fatto che, non utilizzando una modalità standard, siamo costretti ogni volta a reinventare la stessa soluzione da zero.

_java.util.Format_ è l’implementazione base che si incarica di gestire gli aspetti di conversione e rappresentazione, anche se spesso vedremo che non la utilizzeremo direttamente.

Listato 14.1. Primi esempi di formattazione del testo

import java.util.Date;  
import java.util.Formatter;  
  
public class FormatterTest {  
  
  public static void main(String\[\] args) {  
    String name = “Pasquale”;  
    Date today = new Date();  
    double cifra=12.332334;  
      
    //Creiamo un oggetto formatter  
    Formatter formatter = new Formatter ();      
    formatter.format(“Buongiorno %s, sono le %tT”, name, today);  
    System.out.println(formatter);

Possiamo creare una nuova istanza della classe e, attraverso il metodo format, effettuare la formattazione desiderata. Il metodo `format()` si aspetta una stringa e zero o _n_ Objects (in modo da poter formattare un numero indefinito di parametri).

La notazione da usare è un segno di percentuale seguito da una serie di caratteri con cui diciamo al metodo come formattare. In questo caso stiamo formattando una stringa (`%s`) e una data (`%T`). La sintassi è piuttosto vasta ed è perfettamente spiegata nella [documentazione delle API della classe](http://java.sun.com/javase/6/docs/api/java/util/Formatter.html "Documentazione delle API - link esterno"). Proseguiamo con l’esempio:

    //Usiamo in metodo format  
    System.out.format(“Buongiorno %s, sono le %tT”, name, today);  
          
    //Usiamo il metodo printf sullo stream out  
    System.out.printf(“nBuongiorno %s, sono le %tT”, name, today);  
      
    //Formattiamo una cifra  
    System.out.printf(“nCiao %s, stampo la cifra %.2f con 2 cifre decimali”, name, cifra );  
      
    //Possibilità di definire l’ordine dei parametri e di stamparli n volte  
    System.out.printf(“nCiao %2$2s, stampo la cifra %1$2.2f con 2 cifre decimali.” +  
        ” Adios %2$2s”, cifra, name );  
  }  
}

Come dicevamo precedentemente possiamo utilizzare diversi modi per scrivere la rappresentazione su un flusso di output. Infatti utilizziamo gli stessi parametri sul metodo `format()` di System.out. Questo perché all’interno verrà usata la classe Format.

Nell’esempio vediamo come sia possibile utilizzare il metodo `printf()` sulla variabile System.out. Gli esempi ci mostrano poi come poter formattare un decimale, troncando le cifre dopo la seconda e infine come sia possibile ordinare e utilizzare i parametri nella rappresentazione testuale.

Fino alla versione 1.4 di Java, a partire dall’oggetto System.in (InputStream), dovevamo costruirci la nostra rappresentazione utilizzando dei filtri (InputStreamReader, BufferedReader, …) ed effettuando gli opportuni controlli.

Java 1.5 presenta un’utilissima classe che già porta con sé tutti i metodi per la lettura e la conversione dei tipi (numerici o stringhe): **java.util.Scanner**.

Questa classe presenta una lunga serie di metodi, attraverso i quali possiamo gestire un input testuale in maniera ottimale senza preoccuparci delle tipiche operazioni di conversione (ad esempio facendo controlli sui tipi). Inoltre, utilizzando le Regular Expression, è possibile già definire il comportamento, quindi ad esempio stabilire delimitatori di testo in modo da recuperare solo quello di cui abbiamo bisogno.

L’esempio sotto è un programmino che chiede all’utente di digitare il nome del file da visualizzare e dopo legge il file mostrandolo all’utente.

Listato 14.2. Esempio di lettura di un parametro e di un file

import java.io.File;  
import java.io.FileNotFoundException;  
import java.util.Scanner;  
  
public class TestScanner {  
  public static void main(String\[\] args) throws FileNotFoundException {  
    System.out.println(“Insert file name>>”);  
    Scanner scan=new Scanner(System.in);  
    String fileName=scan.next();  
    System.out.println(“Reading “+fileName);  
    printToScreen(fileName);  
  }

Nel main effettuiamo la prima richiesta creando un oggetto Scanner costruito con l’InputStream di default (la console). Ovviamente qui sarà possabile montare tutti i filtri immaginabili (per bufferizzare, cifrare, …). Segue la richiesta con il metodo `next()` che legge un insieme di caratteri (inseriti dall’utente) e ne restituisce una stringa, dopodiché chiamiamo la routine `printToScreen(...)` passando proprio tale valore.

Prima di proseguire diciamo che è molto interessante l’insieme di metodi esposto dalla classe Scanner. è quello più generico, ma esistono i metodi `nextInt()`, `nextDouble()` che si occupano di convertire il valore passato dall’utente in tipo numerico. Questi metodi vanno di pari passo con il rispettivo metodo di controllo `hasNext()` che restituisce un boolean dicendoci se esiste o meno il prossimo token (quindi avremo anche `hasNextInt()`, `hasNextDouble()`, ecc).

Bisogna utilizzare sempre i metodi `nextXXX()` in coppia con `hasNextXXX()` perché la conversione può facilmente causare eccezioni a runtime in base al valore che la classe cerca di convertire.

Seguiamo e vediamo come si presenta il metodo utilizzato per leggere il file (e stamparne il contenuto):

  private static void printToScreen(String fileName) throws FileNotFoundException {  
    Scanner scan=new Scanner(new File(fileName));  
  
    while(scan.hasNext()){  
      String tmp=scan.next();  
      System.out.println(tmp);  
    }  
  }  
}

All’interno del metodo creiamo un nuovo Scanner passando al costruttore un file. Effettuiamo poi un’iterazione e stampiamo il contenuto di ogni token. Abbiamo visto che uno Scanner presenta una facile interfaccia per interrogare uno stream di input. Vediamo ora un interessante metodo per effettuare la lettura di una pagina Web in maniera identica alla lettura di un file:

Listato 14.3. Lettura di una pagina Web

private static void printURL() throws Exception {  
  URL url = new URL (“http://www.html.it/”);  
    
  Scanner scan=new Scanner(url.openConnection().getInputStream());  
  scan.useDelimiter(“\\Z”);  
  
  while(scan.hasNext()){  
    String tmp = scan.next();  
    System.out.println(tmp);  
  }  
}

In questo caso dobbiamo definire il delimitatore di fine documento (Z), oppure avremmo dovuto bufferizzare, poiché, altrimenti, l’implementazione recupera solo i primi 1024 byte nella lettura dello stream di rete. Per il resto tutto rimane identico (e abbastanza semplice).

Con la versione 1.6 di Java arriva un oggetto che finalmente virtualizza in tutto e per tutto una console testuale, la classe _java.io.Console_, appunto. Di fatto è una classe che può essere di utiità solo in applicazioni standalone e solo in virtual machine che hanno una console testuale. Il tipico modo di recuperare quest’oggetto è il metodo `System.console()` ed è implementato solo in versioni della virtual machine che presentano una console di testo (e solo dalla shell di comando).

La classe è davvero interessante in quanto non dobbiamo preoccuparci di complicati gestori, di eccezioni, stream e quant’altro… molto utile per chi inizia a programmare. Inoltre c’è un metodo per chiedere la password che evita di scrivere l’echo di quello che stiamo digitando. Vediamo l’API:

public final class Console implements Flushable {  
  public PrintWriter writer() ;  
  public Reader reader();  
    
  public Console format(String fmt, Object…args);  
  public Console printf(String format, Object…args);  
    
  public String readLine(String fmt, Object…args);  
  public String readLine();  
    
  public char\[\] readPassword(String fmt, Object…args);  
  public char\[\] readPassword();  
    
  public void flush();  
}

Il metodo più importante è senza dubbio `readLine()` la cui chiamata è bloccante in attesa dell’input dell’utente. Il risultato sarà una stringa. I metodi `printf()` e `format()` effettuano una stampa sull’output della console, mentre `readPassword()` restituisce un array che rappresenta i caratteri di input dell’utente. Questo metodo, come dicevamo, è stato codificato per evitare di mostrare il testo digitato (come in qualsiasi applicazione che chiede una password).

Vediamo un esempio concreto dove utilizziamo solo l’interfaccia Console per tutte le operazioni di input/output attraverso i metodi appena visti.

Listato 14.4. Esempio di utilizzo dell’interfaccia Console

package it.html.j16;  
import java.io.Console;  
  
public class ConsoleTest {  
  public static void main(String\[\] args) {  
    //Recuperiamo l’istanza di Console  
    //associata a questa macchina virtuale  
    Console console = System.console();  
  
    //Utilizziamola per scrivere in output  
    console.printf(“Benvenuto alla console Java 1.6”);  
  
    if (args.length>0){  
      //Ora chiediamo un valore in input (notare il formato %s)  
      String line = console.readLine(“nPrego, %s, inserisci un testo >>”, args\[0\]);  
  
      //facciamo l’eco uppercase  
      console.printf(“Echo: “+line.toUpperCase());  
  
      char pwd\[\] = console.readPassword(“nPrego, %s, inserisci il PIN >>”, args\[0\]);  
  
      if (pinEsatto(pwd))  
        console.printf(“Bene! PIN CORRETTO!”);  
      else  
        //lo mostriamo giusto per vedere la rappresentazione  
        //altrimenti non avrebbe senso  
        console.printf(“PIN ERRATO!”);  
    }else{  
      //Mostro la sintassi  
      console.printf(“nPrego inserire un argomento: java it.html.j16.ConsoleTest yourName”);  
    }  
  }  
  
  private static boolean pinEsatto(char\[\] pwd) {  
    char pinEsatto\[\] = {‘H’,’T’,’M’,’L’,’.’,’I’,’T’};  
    //Usiamo l’utility della classe Arrays  
    return java.util.Arrays.equals(pwd, pinEsatto);  
  }  
}

All’utente chiediamo due operazioni di scrittura, una richiesta di una stringa normale, giusto per effettuare una banale operazione di echo, ed una password. Sulla password effettuiamo un controllo e nel caso di pin positivo lo notifichiamo positivamente. Fate attenzione perché il codice qui non funzionerà se non eseguito da una shell di comando. Eseguire questa classe dal pannello di Eclipse, per esempio, darà un valore null alla chiamata `System.console()`.