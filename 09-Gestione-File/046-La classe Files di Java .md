La classe `java.nio.file.Files` è dotata esclusivamente di metodi statici che operano su file e directory. In questo capitolo vedremo come eseguire  
operazioni analoghe a quelle fatte con la classe File esplorando alcune funzionalità delle classe Files per la lettura di  
caratteri e bytes da un file.

Visualizzare il contenuto di una directory
------------------------------------------

Iniziamo con la prima demo che implementa la visualizzazione del contenuto di una directory. Il codice riprende la struttura delle dimostrazioni precedenti cambiando il contenuto del metodo `main`:

```
String folder = "myDir";
     Stream<Path> stream = Files.list(Paths.get(ROOT + FS + folder));
     Iterator<Path> paths = stream.iterator();
     while(paths.hasNext()){
        Path path = paths.next();
        System.out.print(path.getFileName());
        System.out.print(SPACE);
        System.out.print(Files.isDirectory(path)?"d":"f");
        System.out.print(SPACE);
        System.out.print( (Files.isReadable(path)?"R":"") +
                          (Files.isWritable(path)?"W":"")+
                          (Files.isExecutable(path)?"X":""));
        System.out.print(SPACE);
        /*Secondo definizione standard*/
        System.out.print(Files.size(path)/1000+" KB");
        System.out.println();
      }
      stream.close();
```

Utilizzando `Files` un file o una directory è rappresentato tramite un oggetto della classe `Path`. **Non possiamo istanziare oggetti  
di questa classe**, dobbiamo infatti utilizzare la classe `Paths` con il metodo `get()` che accetta path a directory o file costruite come per la classe `File`.

L’oggetto `Path` restituito dal metodo `get()` della classe `Paths`, restituisce a sua volta un oggetto che rappresenta la path che conduce alla directory `myDir` sul File System. Passiamo questo oggetto al metodo `list()` della classe `Files` per ottenere uno stream che rappresenta il contenuto della directory `myDir`.

Sullo stream possiamo ottenere un iteratore sugli oggetti `Path` associati al contenuto di `myDir`. Per ogni oggetto iterato recuperiamo il nome del file o della directory, le informazioni (file o directory), i permessi e la dimensione.

Creare, rinominare e cancellare file o directory
------------------------------------------------

Anche la classe `Files` consente operazioni di creazione, rinomina e cancellazione di file:

```
String folder = "myDir";
     String file1 = "myFile1.txt";
     String file2 = "myFile2.txt";
     String file3 = "myFile3.txt";
     Path source = Paths.get(ROOT + FS + folder + FS + file1);
     Files.createFile(source);
     Files.setAttribute(source, "dos:readonly", true);
     Files.setAttribute(source, "dos:archive", true);
     Path destination = Paths.get(ROOT + FS + folder + FS + file2);
     Files.move(source, destination);
     Path copy = Paths.get(ROOT + FS + folder + FS + file3);
     Files.copy(source, copy);
```

Il frammento di codice all’interno del `main` crea un primo file `myFile1.txt` attraverso `createFile()` (classe `Files`) e ne imposta alcuni attributi attraverso `setAttribute()`. Successivamente definisce un oggetto `Path` che rappresenta il file di destinazione `myFile2.txt` per lo spostamento attraverso il metodo `move()`. In questo particolare caso, avendo `path` sorgente e destinazione coincidenti, lo spostamento si traduce nella rinomina di `myFile1.txt` in `myFile2.txt`.

L’ultima parte illustra il metodo `copy()` attraverso la copia del file `myFile2.txt` in `myFile3.txt` all’interno della stessa directory.

Ricercare file con find()
-------------------------

Un metodo interessante di `Files` è `find()`. Mostriamo un esempio che utilizzi questo metodo per ricercare file con una determinata estensione, all’interno della directory `myDir` e in tutte le sue sottodirectory. Per la ricerca specificheremo un livello massimo di profondità. Andiamo avanti aggiungendo una costante a livello di classe per l’estensione del file:

```
private static String filter = "txt";
```

Completiamo quindi il metodo `main` con la seguente porzione di codice:

```
String folder = "myDir";
         Path source = Paths.get(ROOT + FS + folder);
         Stream<Path> stream = Files.find(source,3,
				 new BiPredicate<Path,BasicFileAttributes>(){
					@Override
					public boolean test(Path path, BasicFileAttributes attributes) {
						return path.getFileName().toString().endsWith(filter);
					}
         });
         Iterator<Path> paths = stream.iterator();
         while(paths.hasNext()){
             Path path = paths.next();
             System.out.println(path.getFileName());
         }
         stream.close();
```

In questo caso definiamo un oggetto di classe `Path` che punta alla directory `myDir`, invochiamo successivamente il metodo `find()` passando come parametri l’oggetto `Path`, un intero che indica la massima profondità nell’albero di ricerca, e un oggetto di una classe anonima che implementa l’interfaccia `BiPredicate`.Quest’ultima consente di creare attraverso `test()` il criterio di filtro della ricerca. Nel caso in esame il metodo restituisce `true` ogni volta che si presenta un oggetto `Path` che corrisponde ad un file di testo. Il risultato è che l’iteratore ottenuto sullo stream restituirà oggetti Path corrispondenti a file txt individuati durante la ricerca in `myDir` e in tutte le sue sottodirectory.

Con la classe `Files` abbiamo a disposizione metodi che consentono la lettura di un file in modo tale da ottenerne informazioni come un array di byte[] oppure come una lista di stringhe, `List<String>`, rappresentante le righe di testo del file stesso (se testuale).

Nel primo caso il formato è il seguente:

```
public static byte[] readAllBytes(Path path) throws IOException
```

Nel secondo caso abbiamo:

```
public static List<String> readAllLines(Path path)  throws IOException
```

La lettura del file testuale viene realizzata con il **CHARSET UTF-8**.