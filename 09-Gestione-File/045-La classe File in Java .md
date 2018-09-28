Gestione path su Windows e Unix
-------------------------------

La classe **java.io.File** fornisce una rappresentazione astratta per nomi di path  
che si riferiscono a file e directory. Iniziamo da un aspetto  
importante per la costruzione di un path: la differenza tra i vari File System. Un path su Windows  
ha in generale una forma del tipo `c:\directory\file.ext`, mentre lo stesso path su Unix sarebbe  
simile a `/directory/file.ext`. Per gestire la costruzione di path, la classe File  
mette a disposizione un campo statico `separatorChar` che varia in funzione del sistema  
in cui l’applicativo è in esecuzione.

`File.separatorChar`  
restituisce il primo carattere di un altro campo statico della classe File, `separator`; esso  
restituisce `\\` su Windows e `/` su Unix. Cosi `File.separatorChar`  
è utile per la costruzione di stringhe che rappresentano path  
rispetto al sistema corrente.

Vediamo un esempio che consente la costruzione di un path e la relativa stampa su console:

```
public class FileDemo {
	private static String ROOT = "C:";
	private static char   FS = File.separatorChar;
	private static String SPACE = " ";
	static {
		String osName = System.getProperty("os.name");
		if(!osName.startsWith("Win"))
			throw new RuntimeException("Il programma è eseguibile solo su Windows");
	}
	public static void main(String[] args) {
         String folder = "myDir";
         String fileName = "myFile.txt";
         System.out.println("File separator:"+FS);
         File winFile = new File(ROOT + FS + folder + FS + fileName );
         System.out.println("Costruzione file path:");
         System.out.println(winFile);
	}
}
```

Il blocco statico, eseguito al caricamento della classe, verifica se il sistema corrente è Windows. Se il controllo  
ha esito negativo viene lanciata un’eccezione che blocca il programma. Nel metodo `main` costruiamo invece un oggetto della classe `File`  
utilizzando il costruttore che accetta come input una stringa che rappresenta un path di file o directory. Nell’esempio  
si fa uso di una directory `myDir` e di un file `myFile.txt`.

Analisi e visualizzazione dei file
----------------------------------

Proseguiamo la realizzazione del programma esplorando i metodi per l’analisi e la visualizzazione dei file di una directory e aggiungiamo nel metodo `main` il seguente frammento di codice:

```
File winFolder = new File(ROOT + FS + folder);
   for(File fileOrDir : winFolder.listFiles()){
	    System.out.print(fileOrDir.getName());
	    System.out.print(SPACE);
	    System.out.print(fileOrDir.isDirectory()?"d":"f");
	    System.out.print(SPACE);
	    System.out.print( (fileOrDir.canRead()?"R":"") +
                          (fileOrDir.canWrite()?"W":"")+
                          (fileOrDir.canExecute()?"X":""));
	    System.out.print(SPACE);
	    /*Secondo definizione standard per 1 KB = 1000 byte*/
	    System.out.print(fileOrDir.length()/1000+" KB");
	    System.out.println();
   }
```

`listFiles()`, invocato sull’oggetto File `winFolder`, consente di ottenere una lista di oggetti File. Ogni oggetto in lista rappresenta un elemento (directory o file) della directory rappresentata da `winFolder`, da ciascun oggetto File possiamo ottenere diverse informazioni durante il ciclo di iterazione.

Nel caso in esame utilizziamo `getName()` per stampare il nome del file o della directory, `isDirectory()` per verificare se l’oggetto rappresenta un file o una directory,  
e lo stato dei permessi di lettura, scrittura, ed esecuzione utilizzando rispettivamente `canRead()`, `canWrite()` e `canExecute()`. Concludiamo sfruttando `lenght()` che restituisce la dimensione in byte del file.

Creare, rinominare e cancellare file o directory
------------------------------------------------

Attraverso `File` siamo in grado di creare (`createNewFile()` e `mkDir()`), cancellare (`delete()`) e rinominare (`renameTo(File dest)`) file o directory. Modifichiamo quindi il `main` realizzando un nuova classe che illustri l’utilizzo di questi metodi, fatta eccezione di `delete()`, per visualizzare il risultato sul File System:

```
public class FileDemo2 {
	private static String ROOT = "C:";
	private static char   FS = File.separatorChar;
	static {
		String osName = System.getProperty("os.name");
		if(!osName.startsWith("Win"))
			throw new RuntimeException("Il programma è eseguibile solo su Windows");
	}
	public static void main(String[] args) throws IOException {
         String folder = "myDir";
         String fileName = "myFile.txt";
         File winFolder = new File(ROOT + FS + folder );
         if(winFolder.mkdir()){
          System.out.println("Creata la directory "+winFolder.getAbsolutePath());
          File winFile = new File(winFolder,fileName);
          winFile.setReadable(true);
          winFile.setWritable(true);
          winFile.setExecutable(false);
          if(winFile.createNewFile()){
           System.out.println("Creato il file "+winFile.getAbsolutePath());
          }else{
           System.out.println("Impossibile creare il file "+ winFile.getAbsolutePath());
          }
          String newFileName = "myFile2.txt";
          if(winFile.renameTo(new File(winFolder,newFileName))){
             System.out.println("Rinominato file "+ fileName +" in "+ newFileName);
          } else {
             System.out.println("Impossibile rinominare il file");
          }
         } else {
             System.out.println("Impossiabile creare la directory "+winFolder.getAbsolutePath());
         }
     }
 }
```

Il codice inizia definendo un oggetto File `winFolder` che rappresenta la directory da creare. `mkDir()` tenta la creazione di questa directory restituendo `true` in caso di successo o altrimenti `false`. Successivamente un altro costruttore della classe File riceve in input un oggetto File che rappresenta un percorso di directory e una stringa che rappresenta un nome di file.

Con questo costruttore si realizza un nuovo oggetto File `winFile` riferito ad un file contenuto nella directory specificata. Su winFile impostiamo alcuni permessi e tentiamo la creazione con `createNewFile()` che, come `mkDir()`, restituisce `true` in caso di successo, altrimenti `false`. Stampiamo quindi sulla console il path assoluto del file (`c:\myDir\myFile.txt`) ed infine tentiamo un test di ridenominazione del file attraverso `renameTo()` che necessita in input di un oggetto file  
costruito in modo simile a `winFile`. In quest’ultimo caso dobbiamo aver cura di specificare un nome di file differente.