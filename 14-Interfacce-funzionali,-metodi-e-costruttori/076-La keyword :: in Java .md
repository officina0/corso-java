La parola chiave `::`,  
introdotta con Java 8, consente di scrivere codice più compatto per l’invocazione  
di un metodo o costruttore di una classe. Le sintassi generali di utilizzo del simbolo ::  
sono le seguenti:

```
NomeClasse::nomeMetodo
oggetto::nomeMetodo
NomeClasse::new
```

Nel primo caso l’alias `nomeMetodo` è il nome del metodo statico, al quale intendiamo riferirci, della classe identificata dal nome `NomeClasse`.  
La seconda sintassi illustra invece l’invocazione di un metodo non statico di un oggetto istanza di una particolare classe.  
La terza mostra invece l’invocazione di un costruttore della classe `NomeClasse`.

Se il nome del metodo, o del costruttore, coincide con la _signature_ del metodo di un’interfaccia funzionale, possiamo ottenere un’implementazione  
dell’interfaccia che utilizzi il metodo referenziato con l’operatore `::` che, a sua volta, si collega ai parametri di input ed output  
del metodo funzionale.

Vediamo subito un semplice esempio utilizzando l’interfaccia funzionale `IAction` del capitolo precedente:

```
@FunctionalInterface
public interface IAction {
	public void doWork();
}
```

Vogliamo un oggetto di questa interfaccia che fornisca una semplice implementazione del metodo `doWork` che consista nell’invocazione del  
metodo `static println()` della classe `System.out` al fine di stampare una riga vuota sulla console di output. Raggiungeremo l’obiettivo  
utilizzando l’operatore `::`:

```
IAction action = System.out::println;
System.out.println("Blank space a seguire");
action.doWork();
System.out.println("Fine");
```

La keyword `::` permette di riferirsi al metodo `println()` e di incorporare la sua chiamata all’interno di un oggetto di una classe anonima  
che implementa `IAction`. Il risultato è l’oggetto `action` che stampa una riga vuota.

Proseguiamo con un semplice ma più interessante esempio di invocazione di un metodo statico. Definiamo la seguente interfaccia funzionale:

```
@FunctionalInterface
public interface IMath {
	public int max(int value1, int value2);
}
```

e utilizziamo le seguenti righe di codice per ottenere un oggetto che la implementa e che inserisca al suo interno  
la chiamata al metodo `max()` della classe `Math`:

```
IMath math = Integer::max;
System.out.println("Il max: "+math.max(5, 10));
```

Consideriamo ora l’invocazione di un metodo non statico su una particolare istanza. Per questo esempio realizziamo l’interfaccia funzionale:

```
@FunctionalInterface
public interface IString {
	public String subString(int start, int end);
}
```

Intendiamo ottenere un oggetto di una classe anonima che inserisca come  
implementazione del metodo `subString()` di `IString` il metodo `substring()` invocato sull’oggetto istanziato  
della classe `String`:

```
String dummyString = "Pluto";
     IString str2 = dummyString::substring;
     System.out.println("substring 2-4:"+ str2.subString(2, 4));
```

Concludiamo la trattazione dell’operatore `::` mostrando un suo esempio d’uso per l’invocazione di costruttori di una classe. Per  
questa dimostrazione realizziamo la seguente classe rappresentativa di un File generico:

```
public class File {
	public File(String name, String path){
		System.out.println("Costruttore a due argomenti");
	}
	public File(String name){}
}
```

Successivamente una nuova interfaccia funzionale che lavori come creatrice di oggetti `File`:

```
@FunctionalInterface
public interface IFileCreator {
  public File create(String name, String path);
}
```

Desideriamo, come nei precedenti casi, una classe anonima che implementi `IFileCreator` ed inserisca all’interno del metodo  
`create()`, che è in match a livello di _signature_ con uno dei costruttori della nostra classe `File`, la creazione e restituzione  
di un oggetto `File`:

```
IFileCreator fileCreator = File::new;
fileCreator.create("FileName", "FilePath");
```

In questo caso abbiamo creato un riferimento ad un costruttore di un oggetto `File` attraverso `File::new`. Il compilatore Java sceglie automaticamente il costruttore corretto attraverso il processo della _signature_ del metodo `create()`.