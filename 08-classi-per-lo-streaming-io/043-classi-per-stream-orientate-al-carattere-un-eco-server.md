# 043 Classi Per Stream Orientate Al Carattere Un Eco Server

Realizziamo un semplice applicativo Client-Server nel quale un programma client invia linee di testo verso un programma server che si limita a restituirle come risposta, un semplice **eco-server**. Iniziamo con la classe che implementa il client. Abbiamo bisogno di una Socket che apra la connessione verso il server, successivamente otteniamo dalla socket gli stream di input ed output per leggere e scrivere nella socket e quindi inviare e ricevere dati dal server.

Questi stream sono orientati al byte, utilizziamo quindi le classi bridge byte-carattere, `InputStreamReader` e `OutputStreamWriter`, per collegarli a stream orientati al carattere:

public class SocketClient {

```text
public static void main(String\[\] args) throws IOException {

  Socket socket = new Socket("127.0.0.1", 4444);
  System.out.println("Socket Client Demo");
  InputStreamReader socketInStream = new InputStreamReader(socket.getInputStream());
  BufferedReader socketReader = new BufferedReader(socketInStream);
  OutputStreamWriter socketOutStream = new OutputStreamWriter(socket.getOutputStream());
  BufferedWriter socketWriter = new BufferedWriter(socketOutStream);

}
```

}

Proseguiamo con gli stream bridge da collegare allo stream per lo standard di input \(la tastiera\) in modo tale da poter interagire con il programma da riga di comando all’interno di una console:

public class SocketClient {

public static void main\(String\[\] args\) throws IOException {

```text
  Socket socket = new Socket("127.0.0.1", 4444);
  System.out.println("Socket Client Demo");
  InputStreamReader socketInStream = new InputStreamReader(socket.getInputStream());
  BufferedReader socketReader = new BufferedReader(socketInStream);
  OutputStreamWriter socketOutStream = new OutputStreamWriter(socket.getOutputStream());
  BufferedWriter socketWriter = new BufferedWriter(socketOutStream);

  InputStreamReader consoleStream = new InputStreamReader(System.in);
  BufferedReader consoleReader = new BufferedReader(consoleStream);
```

} }

Concludiamo quindi con il ciclo while che acquisisce una stringa in input, la invia al server e recupera la risposta stampandola in console.

Un aspetto importante da sottolineare, è l’uso del metodo `flush()` sull’oggetto `BufferedWriter`, ne abbiamo bisogno per forzare lo svuotamento del buffer ed inviare i dati al server.

Un’alternativa consiste nell’utilizzare la classe `PrintWriter` come ulteriore collegamento nella catena di stream; possiamo infatti collegare un `PrintWriter` ad un `BufferedWriter` ed utilizzare il metodo `println()` di `PrintWriter` per inviare la stringa al server senza necessità di utilizzare `flush()`.

public class SocketClient {

```text
public static void main(String\[\] args) throws IOException {

  Socket socket = new Socket("127.0.0.1", 4444);
  System.out.println("Socket Client Demo");
  InputStreamReader socketInStream = new InputStreamReader(socket.getInputStream());
  BufferedReader socketReader = new BufferedReader(socketInStream);
  OutputStreamWriter socketOutStream = new OutputStreamWriter(socket.getOutputStream());
  BufferedWriter socketWriter = new BufferedWriter(socketOutStream);

  InputStreamReader consoleStream = new InputStreamReader(System.in);
  BufferedReader consoleReader = new BufferedReader(consoleStream);

  String consoleIn;

  do{
      System.out.print("Client>");
      consoleIn=consoleReader.readLine();
      socketWriter.write(consoleIn+"\\n");
      socketWriter.flush();
      System.out.println("Server>"+socketReader.readLine());
  } while(!consoleIn.equals("exit"));

  socketInStream.close();
  socketReader.close();
  socketOutStream.close();
  socketWriter.close();
  consoleStream.close();
  consoleReader.close();
  socket.close();
```

} }

La classe server è strutturalmente molto simile al client, in questo caso abbiamo però bisogno di una ServerSocket per le connessioni. Sulla ServerSocket otteniamo una Socket da cui ricaviamo gli stream di byte da collegare a stream orientati ai caratteri:

public class SocketServer {

```text
public static void main(String\[\] args) throws IOException {

    System.out.println("Echo Server attesa connessione ...");
    ServerSocket serverSocket = new ServerSocket(4444);
    Socket socket = serverSocket.accept();
    System.out.println("Connessione accettata");

    InputStreamReader socketInStream = new InputStreamReader(socket.getInputStream());
    BufferedReader socketReader = new BufferedReader(socketInStream);

    OutputStreamWriter socketOutStream = new OutputStreamWriter(socket.getOutputStream());
    BufferedWriter socketWriter = new BufferedWriter(socketOutStream);
}
```

}

Proseguaimo quindi con un ciclo simile al precedente per leggere le stringhe provenienti dal client e restituirle come risposta:

public class SocketServer {

```text
public static void main(String\[\] args) throws IOException {

    System.out.println("Echo Server attesa connessione ...");
    ServerSocket serverSocket = new ServerSocket(4444);
    Socket socket = serverSocket.accept();
    System.out.println("Connessione accettata");

    InputStreamReader socketInStream = new InputStreamReader(socket.getInputStream());
    BufferedReader socketReader = new BufferedReader(socketInStream);

    OutputStreamWriter socketOutStream = new OutputStreamWriter(socket.getOutputStream());
    BufferedWriter socketWriter = new BufferedWriter(socketOutStream);

    String lineRead;

    do{
        lineRead=socketReader.readLine();
        System.out.println("Server>"+lineRead);
        socketWriter.write(lineRead);
        socketWriter.newLine();
        socketWriter.flush();
    } while(!lineRead.equals("exit"));

    serverSocket.close();
    socket.close();
    socketInStream.close();
    socketReader.close();
    socketOutStream.close();
    socketWriter.close();
}
```

}

Il nostro lavoro è terminato e possiamo finalmente lanciare gli applicativi per testarne il funzionamento avendo cura di eseguire il server prima del client. Lato client l’output in console sarà simile al seguente:

Socket Client Demo Client&gt;Ciao Server Server&gt;Ciao Server Client&gt;Echo demo Server&gt;Echo demo Client&gt;

Mentre il corrispondente lato server:

Echo Server attesa connessione ... Connessione accettata Server&gt;Ciao Server Server&gt;Echo demo

