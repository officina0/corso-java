Dopo aver visto le classi per la realizzazione di Stream orientati al byte, ci spostiamo su quelle orientate ai caratteri in formato **UNICODE**. Tutte  
le classi orientate allo Stream di caratteri in Java estendono le classi astratte `java.io.Reader`  
o `java.io.Writer`; esempi di queste classi sono le seguenti: `FileReader`, `FileWriter`, `BufferedReader`,  
`BufferedWriter`, `InputStreamReader` ed `OutputStreamWriter`.

Iniziamo la nostra trattazione dall’ultimo esempio  
presentato nel precedente capitolo fornendo una versione in grado di leggere correttamente file i cui  
caratteri sono in formato **ASCII**. Utilizziamo le classi bridge `InputStreamReader` e `OutputStreamWriter`  
che rappresentano classi di collegamento tra Stream di byte e Stream di caratteri:

```
public class BytesToCharsDemoUTF8 {
	private static final String sourcePath = "/source/testASCII.txt";
	private static final String destinationPath = "/destination/testASCII.txt";
	public static void main(String[] args) throws IOException {
        FileInputStream sourceStream = new FileInputStream(sourcePath);
        InputStreamReader charSourceStream = new InputStreamReader(sourceStream,"ASCII");
        FileOutputStream destStream = new FileOutputStream(destinationPath);
        OutputStreamWriter charDestStream = new OutputStreamWriter(destStream,"ASCII");
	}
}
```

Per questa nuova demo, facciamo uso di un file di testo in formato ASCII contenuto nel codice allegato.  
L’aspetto importante da notare, oltre al Chain di collegamento tra le due tipologie di Stream, è la specifica  
del tipo di encoding come secondo parametro dei costruttori delle classi `InputStreamReader` e `OutputStreamWriter`.

La parte che completa il codice legge i dati attraverso un buffer di caratteri:

```
public class BytesToCharsDemoUTF8 {
	private static final String sourcePath = "/source/testASCII.txt";
	private static final String destinationPath = "/destination/testASCII.txt";
	public static void main(String[] args) throws IOException {
        FileInputStream sourceStream = new FileInputStream(sourcePath);
        InputStreamReader charSourceStream = new InputStreamReader(sourceStream,"ASCII");
        FileOutputStream destStream = new FileOutputStream(destinationPath);
        OutputStreamWriter charDestStream = new OutputStreamWriter(destStream,"ASCII");
        char[] buffer = new char[100];
        int charactersRead=0;
        while ( (charactersRead=charSourceStream.read(buffer))!=-1) {
          charDestStream.write(buffer,0,charactersRead);
        }
     	charSourceStream.close();
        charDestStream.close();
        destStream.close();
        sourceStream.close();
	}
}
```

Partendo dall’esempio precedente potremmo realizzare un applicativo Client-Server nel quale un’applicazione client invia linee di testo verso un programma server che si limita a restituirle come risposta; nel prossimo capitolo capiremo in che modo sia possibile gestire questo tipo di **eco-server**.