Concludiamo il nostro discorso sulle classi per lo Stream orientate al byte con un esempio dedicato all’uso del concatenamento di Stream per leggere caratteri da un flusso di byte. In questo caso inseriamo nella cartella `source` un file txt riempito con del testo e salvato in formato UNICODE usando un editor di testo, come per esempio WordPad in Windows. Il nostro obiettivo è copiare il file testuale nella cartella `destination` leggendo e scrivendo caratteri.

Sappiamo che i caratteri in Java sono rappresentati in formato UNICODE, questo significa 2 byte per carattere. Per leggere un file di testo in formato UNICODE sarà sufficiente leggere due byte alla volta ed interpretarli come un singolo carattere. Iniziamo con il codice di base che realizza il collegamento degli Stream `FileInputStream` e `FileOutputStream` con gli Stream `DataInputStream` e `DataOutputStream` che consentono la lettura di dati primitivi da uno Stream di byte:

```
public class BytesToCharsDemo {
    private static final String sourcePath = "/source/test.txt";
    private static final String destinationPath = "/destination/test.txt";
    public static void main(String[] args) throws IOException {
        FileInputStream sourceStream = new FileInputStream(sourcePath);
        DataInputStream charSourceStream = new DataInputStream(sourceStream);
        FileOutputStream destStream = new FileOutputStream(destinationPath);
        DataOutputStream charDestStream = new DataOutputStream(destStream);
    }
}
```

Proseguiamo con il ciclo che legge due byte alla volta, attraverso il metodo `readChar()`, interpretandoli come un singolo carattere:

```
public class BytesToCharsDemo {
    private static final String sourcePath = "/source/test.txt";
    private static final String destinationPath = "/destination/test.txt";
    public static void main(String[] args) throws IOException {
        FileInputStream sourceStream = new FileInputStream(sourcePath);
        DataInputStream charSourceStream = new DataInputStream(sourceStream);
        FileOutputStream destStream = new FileOutputStream(destinationPath);
        DataOutputStream charDestStream = new DataOutputStream(destStream);
        while (charSourceStream.available()>=2) {
             charDestStream.writeChar(charSourceStream.readChar());
        }
    }
}
```

Il metodo `available()` consente di verificare l’esistenza, nel nostro caso, di un numero di byte maggiore o uguale a 2 nello stream di input, di leggerli come un carattere, e di scrivere questo carattere nel file di destinazione.

Quando lavoriamo in Java con file di testo è fondamentale considerare la codifica adottata dal file stesso, al fine di operare in sicurezza ed evitare effetti indesiderati.  
Infatti se avessimo editato il file txt con Notepad di Windows, il file sarebbe stato salvato in ASCII, che codifica i caratteri  
usando un byte.

Lanciando il nostro programma puntando ad un file di testo in codifica ASCII potremmo incorrere in un’eccezione, in quanto non avendo più la garanzia di due byte per carattere il tipo di lettura implementato è inadeguato.

Ciò che abbiamo realizzato è un esempio in grado di leggere e scrivere file di testo in formato UNICODE, il formato per le stringhe in Java. Vedremo nel successivo capitolo il metodo migliore per trattare file di testo Java attraverso classi che realizzano un bridge tra gli Stream orientati ai caratteri e gli Stream orientati al byte.