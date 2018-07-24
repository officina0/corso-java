La struttura I/O di Java è sostanzialmente basata sul concetto di **Stream** tramite classi che supportano lettura e scrittura di dati su file e all’interno di applicazioni in rete. Possiamo pensare ad uno Stream come ad una sequenza ordinata di dati, analizzaremo quindi gli Stream nei quali il dato elementare della sequenza è il byte.

Le classi di I/O in Java forniscono un approccio strutturato del seguente tipo:

1.  Uno Stream di basso livello che legge byte da una sorgente di input (es. un file).
2.  Un filtro Stream di alto livello che legge byte da uno Stream di basso livello al fine di estrarre dati come numeri e caratteri.
3.  Un Reader, Stream orientato al carattere, per la lettura di stringhe UTF in formato Unicode.
4.  Uno Stream di basso livello che scrive byte verso una destinazione di output (es. un file).
5.  Un filtro Stream che scrive dati di alto livello (caratteri, numeri…) verso uno Stream di basso livello.
6.  Un Writer, Stream orientato al carattere, per la scrittura di stringhe UTF in formato Unicode.

Le classi per la lettura e scrittura di dati in Stream orientati al byte estendono le classi astratte `java.io.InputStream` e `java.io.OutputStream`. Esempi di queste classi sono:

*   `FileInputStream`, `DataInputStream` e `BufferedInputStream`;
*   `FileOutputStream`, `DataOutputStream` e `BufferedOutputStream`.

Utilizziamo queste classi per implementare alcuni semplici programmi Java per la copia di file partendo dal più semplice per arrivare a quello più performante. Iniziamo con una semplice copia che legga e scriva un byte alla volta.

In questo caso realizziamo una cartella `source` ed una cartella `destination` all’interno della directory `C:`. Proseguiamo inserendo l’immagine `java-logo.png` nella cartella `source`. L’obiettivo è realizzare un codice che legga i byte dell’immagine per copiarla in `destination`. Chiamiamo questa prima classe demo con il nome di `ByteStreamsDemo1` e inseriamo due costanti relative ai path sorgente e destinazione:

public class ByteStreamsDemo1 {

    private static final String sourcePath = "/source/java-logo.png";
    private static final String destinationPath = "/destination/java-logo.png";

    public static void main(String\[\] args) throws IOException {
    }
}

Proseguiamo con il codice che definisce gli Stream di lettura dal file sorgente e scrittura verso il file di destinazione:

public class ByteStreamsDemo1 {

    private static final String sourcePath = "/source/java-logo.png";
    private static final String destinationPath = "/destination/java-logo.png";

    public static void main(String\[\] args) throws IOException {
     FileInputStream sourceStream = new FileInputStream(sourcePath);
     FileOutputStream destStream = new FileOutputStream(destinationPath);
    }
}

E aggiungiamo la parte che legge i bytes dal `java-logo.png` in `source`, scrivendoli contemporaneamente nel file di copia `java-logo.png` in `destination`:

	
public class ByteStreamsDemo1 {
    private static final String sourcePath = "/source/java-logo.png";
    private static final String destinationPath = "/destination/java-logo.png";

    public static void main(String\[\] args) throws IOException {
     FileInputStream sourceStream = new FileInputStream(sourcePath);
     FileOutputStream destStream = new FileOutputStream(destinationPath);
     int singleByte;
	 
     while ((singleByte = sourceStream.read()) != -1) {
       destStream.write(singleByte);
     }
		
     destStream.close();
     sourceStream.close();
     }
}

Il codice legge un byte alla volta attraverso il metodo `read()` della classe `FileInputStream` che restituisce il byte stesso attraverso un tipo `int`. Quando `read()` restituisce `-1` abbiamo raggiunto la fine dello Stream e quindi la fine del file. Il metodo `write()` di `FileOutputStream` non fa altro che scrivere il byte letto sul file di destinazione.

L’esempio appena visto non rappresenta il modo ottimale di effettuare un’operazione di I/O come la copia di un file che può richiedere del tempo. Possiamo migliorare il codice in modo tale che recuperi, e successivamente scriva, più bytes alla volta:

public class ByteStreamsDemo2 {

	private static final String sourcePath = "/source/java-logo.png";
	private static final String destinationPath = "/destination/java-logo.png";

	public static void main(String\[\] args) throws IOException {
		FileInputStream sourceStream = new FileInputStream(sourcePath);
		FileOutputStream destStream = new FileOutputStream(destinationPath);
		byte\[\] bytes = new byte\[1000\];
		int bytesRead=0;
		while ( (bytesRead=sourceStream.read(bytes)) != -1) {
			destStream.write(bytes, 0, bytesRead);
		}
		
		destStream.close();
		sourceStream.close();
	}
}

Questa seconda demo si differenzia soltanto nell’uso del metodi `read()` e `write()` che accettano un array di byte piuttosto che un singolo byte. In questo caso infatti riusciamo a leggere e scrivere 1000 byte alla volta. `read()` legge un quantitativo di bytes dallo Stream di input con l’intento di riempire tutto l’array di byte passato come parametro, restituisce `-1` quando si raggiunge la fine dello Stream e quindi del file.

`write()` scrive il corretto quantitativo di byte sullo Stream di output leggendolo dall’array bytes di input con un offset `0` (inizio dell’array), ed un quantitativo di bytes a partire da questo offset pari al valore letto con il metodo `read()`.

Questa seconda classe migliora notevolmente le prestazioni ma è possibile fare di meglio, possiamo utilizzare **Stream che impiegano buffer di I/O**:

public class ByteStreamsDemo3 {

	private static final String sourcePath = "/source/java-logo.png";
	private static final String destinationPath = "/destination/java-logo.png";

	public static void main(String\[\] args) throws IOException {
		FileInputStream sourceStream = new FileInputStream(sourcePath);
		BufferedInputStream sourceBufferStream = new BufferedInputStream(sourceStream);
		FileOutputStream destStream = new FileOutputStream(destinationPath);
		BufferedOutputStream destBufferStream = new BufferedOutputStream(destStream);

	
		byte\[\] bytes = new byte\[1000\];
		int bytesRead=0;
		
		while ((bytesRead=sourceBufferStream.read(bytes)) != -1) {
			destBufferStream.write(bytes,0,bytesRead);
		}
		sourceBufferStream.close();
		destBufferStream.close();
		destStream.close();
		sourceStream.close();
	}
}

Questo versione riprende la precedente implementando la lettura e scrittura di 1000 bytes alla volta migliorandola attraverso il collegamento degli Stream di basso livello con Stream di più alto livello che aggiungono funzionalità di bufferizzazione allo Stream base.

In questo caso infatti leggiamo e scriviamo utilizzando un `BufferedInputStream` ed un `BufferedOutputStream` che sono collegati rispettivamente ad un `FileInputStream` e ad un `FileOutputStream`.