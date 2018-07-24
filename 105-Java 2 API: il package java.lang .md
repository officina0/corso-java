Abbiamo già incontrato **il package java.lang**, ma in questa lezione lo esaminiamo più a fondo: anzitutto c’è da dire che questo è uno dei package più importanti dell’API di Java, contiene multissime classi e interfacce fondamentali per la programmazione Java, tanto che questo package viene automaticamente incluso in tutti i programmi.

Nel primo capitolo abbiamo visto le classi involucro per i tipi primitivi incluse in questo package, in questa sezione esamineremo alcune classi del package che fanno delle cose più complesse.

[In fondo alla pagina](#contenuto "Guarda il contenuto del package java.lang") riportiamo tutto il suo contenuto (interfacce, classi, eccezioni ed errori).

La classe System
----------------

La prima classe che vediamo è la classe **System**, la quale interfaccia il nostro programma Java con il sistema operativo sul quale sussiste la virtual machine. Iniziamo esaminando tre attributi statici:

static PrintStream err
static InputStream in
static PrintStream out

Rappresentano rispettivamente gli i flussi (stream) di informazioni scambiati con la console (`standard error`, `standard input` e `standard output`), ciascuno di essi è un oggetto e sfrutta i metodi della classe relativa. Ad esempio in scrittura invochiamo il metodo `println` della classe `PrintStream`, perché `out` è di tipo `PrintStream`.

System.out.println("Ciao");

Nei nostri esempi abbiamo usato indifferentemente le espressioni:

System.out.println("Ciao");
System.out.println(true);
System.out.println(10);
System.out.println('C');

In generale in Java non è possibile invocare un metodo passando un parametro attuale di un tipo diverso rispetto al parametro formale (quello che si trova nella firma). È possibile però invocare il metodo `println` con tutti questi tipi di dato diversi, perché `PrintStream` contiene tutti i metodi `println` dichiarati con il parametro formale adatto, sono definiti infatti:

`void println()`

`void println(boolean x)`

`void println(char x)`

`void println(char[] x)`

`void println(double x)`

`void println(float x)`

`void println(int x)`

`void println(long x)`

`void println(Object x)`

`void println(String x)`

Lo stesso vale per `err`, anch’esso di tipo `PrintStream`.

**L’oggetto ‘err’** viene usato per segnalare gli errori che avvengono durante l’esecuzione del programma, non è raro trovare nei programmi espressioni del tipo:

try {
  System.out.println(stringa+" vs "+altrastringa+" = " +stringa.compareTo(altrastringa));
}
catch (nullPointerException e) {
  System.err.println("ERRORE: La seconda stringa stringa è nulla");
};

**L’oggetto ‘in’** è invece di tipo `InputStream` e come abbiamo detto serve a ricervere il flusso di informazioni che il sistema operativo serve come “standard input”, tipicamente si tratta dell’input di una tastiera o di un terminale. Pertanto leggiamo il flusso in arrivo con i **metodi read**:

abstract int read() 
int read(byte\[\] b) 
int read(byte\[\] b, int off, int len)

Trattandosi di un oggetto di tipo `InputStream` esso legge dei `byte` o dei blocchi di `byte`, per ottenere dati in formati particolari dobbiamo lavorare un po’, come nell’esempio seguente che chiamiamo `StandardIO.java`:

import java.io.*;

class StandardIO 
{
  public static void main(String\[\] argomenti)
  {
    System.out.println("Scrivi più righe e premi \[INVIO\] per separarlen Per uscire dal programma scrivi 'end'");
    
    // in 'a' prendiamo il flusso di standard input
    InputStreamReader a = new InputStreamReader(System.in);

    // sfruttiamo un BufferedReader per contenere il flusso 
    // di byte
    BufferedReader IN = new BufferedReader(a);
	
    // e convertirlo in una stringa
    String s = new String();
	
    // finché non si verifica la condizione di uscita
    while(s.compareTo("end")!=0)
    {
      try { 
        s= IN.readLine(); // Legge il contenuto della riga
		
      } catch (IOException e) {
        s="ERRORE DI LETTURA";
      };

      // e lo stampa
      System.out.println("Letto: "+s);
    }
  }
}

La classe System contiene diversi metodi, in fondo alla pagina si può consultare un breve [elenco dei metodi](#systemclass "Elenco dei metodi della classe System") principali.

Troviamo dei metodi per riassegnare gli standard input, output ed err, in altri flussi, ad esempio per scrivere o leggere file, solo impostando in modo nuovo gli oggetti `err`, `in` e `out`.

static void setErr(PrintStream err) 
static void setIn(InputStream in) 
static void setOut(PrintStream out)

I seguenti metodi invece servono ad impostare le proprietà del sistema:

static void setProperties(Properties props)
static String setProperty(String key, String value)

Abbiamo usato degli oggetti di tipo **Properties**, si tratta di specializzazioni di tabelle hash di java, semplificando possiamo pensarle come tabelle che reappresentano coppie chiave-valore (in realtà sono delle strutture molto usate in informatica per il reperimento veloce di informazioni).

Un elenco con le [properties di sistema](#systemprop "Guarda la tabella con le properties di sistema") è riportato in basso, intanto vediamo come scrivere un programmino che scriva in un file le informazioni sul sistema, lo chiamiamo `Sistema.java`:

import java.io.*;

public class Sistema
{
  public static void main(String\[\] arg)
  {
    // Cambio lo standard output, uso il file Sistema.txt
    File outFile=new File("Sistema.txt");
    FileOutputStream fw;
    
	try { fw=new FileOutputStream(outFile) ; }
    catch (IOException e) { fw=null; };

    PrintStream Output=new PrintStream(fw);
    System.setOut(Output);

    // Scrivo sul nuovo standard output:
    // Tempo:
	long tempo = System.currentTimeMillis();
	System.out.println("Tempo in millisecondi: "+tempo);
	
	long t1=tempo/1000; 
	System.out.println("Tempo in secondi: "+t1);
	
	long sec = t1%60; 
	long t3  = t1/60;
	long min = t3%60;
	long t4  = t3/60;
	System.out.println("Tempo in ore h"+t4+" m"+min+" s"+sec);
   
    System.out.println("nÈ il tempo passato dal 1/1/1970 ad oran");
	
	// Proprietà del sistema:
	System.out.println("nProprieta' del sistema:n");
	String tmp;
	System.out.println("ntJAVAn");
	tmp=System.getProperty("java.version ");
	System.out.println("Versione dell'ambiente di Java Runtime: "+tmp);
	tmp=System.getProperty("java.vendor");
	System.out.println("Distributore dell'ambiente di Java Runtime: "+tmp);
	tmp=System.getProperty("java.vendor.url");
	System.out.println("URL del distributore di Java: "+tmp);
	tmp=System.getProperty("java.home");
	System.out.println("Directory in cui è installato Java: "+tmp);
	tmp=System.getProperty("java.vm.specification.version");
	System.out.println("Versione delle specifiche della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.vm.specification.vendor");
	System.out.println("Distributore delle specifiche della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.vm.specification.name");
	System.out.println("Nome delle specifiche della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.vm.version");
	System.out.println("Versione della implementazione della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.vm.vendor" );
	System.out.println("Distributore della implementazione della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.vm.name");
	System.out.println("Nome della implementazione della Java Virtual Machine: "+tmp);
	tmp=System.getProperty("java.specification.version");
	System.out.println("Versione dell'ambiente di Java Runtime: "+tmp);
	tmp=System.getProperty("java.specification.vendor");
	System.out.println("Distributore dell'ambiente di Java Runtime Java Runtime: "+tmp);
	tmp=System.getProperty("java.specification.name" );
	System.out.println("Nome dell'ambiente di Java Runtime: "+tmp);
	
	System.out.println("ntCLASSIn");
	tmp=System.getProperty("java.class.version");
	System.out.println("Versione delle classi di Java: "+tmp);
	tmp=System.getProperty("java.class.path");
	System.out.println("Pathname delle classi di Java: "+tmp);
	
	System.out.println("ntSISTEMA OPERATIVOn");

	tmp=System.getProperty("os.name");
	System.out.println("Nome Sistema Operativo: "+tmp);
	tmp=System.getProperty("os.arch");
	System.out.println("Architettura del Sistema Operativo: "+tmp);

	tmp=System.getProperty("os.version");
	System.out.println("Versione del sistema Operativo: "+tmp);
	tmp=System.getProperty("file.separator");
	System.out.println("Separatore di File: "+tmp);

	tmp=System.getProperty("path.separator");
	System.out.println("Separatore di Pathname: "+tmp);

	tmp=System.getProperty("line.separator");
	System.out.println("New Line: "+tmp);
	System.out.println("ntUTENTEn");
	tmp=System.getProperty("user.name");
	System.out.println("Account name dell'utente: "+tmp);
	tmp=System.getProperty("user.home");
	System.out.println("Home directory dell'utente: "+tmp);
	tmp=System.getProperty("user.dir");
    System.out.println("Working directory dell'utente: "+tmp);
  }
}

Una volta lanciata l’esecuzione viene creato un file di testo che conterrà qualcosa di simile a questo:

Tempo in millisecondi: 1317306980661
Tempo in secondi: 1317306980
Tempo in ore h365918 m36 s20

È il tempo passato dal 1/1/1970 ad ora


Proprieta' del sistema:


	JAVA

Versione dell'ambiente di Java Runtime: null
Distributore dell'ambiente di Java Runtime: Sun Microsystems Inc.
URL del distributore di Java: http://java.sun.com/
Directory in cui è installato Java: C:Program FilesJavajre6
Versione delle specifiche della Java Virtual Machine: 1.0
Distributore delle specifiche della Java Virtual Machine: Sun Microsystems Inc.
Nome delle specifiche della Java Virtual Machine: Java Virtual Machine Specification
Versione della implementazione della Java Virtual Machine: 19.1-b02
Distributore della implementazione della Java Virtual Machine: Sun Microsystems Inc.
Nome della implementazione della Java Virtual Machine: Java HotSpot(TM) 64-Bit Server VM
Versione dell'ambiente di Java Runtime: 1.6
Distributore dell'ambiente di Java Runtime Java Runtime: Sun Microsystems Inc.
Nome dell'ambiente di Java Runtime: Java Platform API Specification

	CLASSI

Versione delle classi di Java: 50.0
Pathname delle classi di Java: E:EclipseUsersAndreaworkspaceprovabin

	SISTEMA OPERATIVO

Nome Sistema Operativo: Windows Vista
Architettura del Sistema Operativo: amd64
Versione del sistema Operativo: 6.0
Separatore di File: 
Separatore di Pathname: ;
New Line: 

	UTENTE
	
Account name dell'utente: Andrea
Home directory dell'utente: C:UsersAndrea
Working directory dell'utente: E:EclipseUsersAndreaworkspaceprova

La classe Object
----------------

Un’altra classe molto importante del package **java.lang** di Java è la classe **Object**, questa è la classe da cui ereditano tutte le altre classi di Java: in altre parole ogni altra classe di Java è una estensione della classe `java.lang.Object`.

La class Object non contiene alcuna variabile, contiene gli 11 metodi che vengono ereditati dalle altre classi di Java, e che possono essere quindi utilizzati in qualsiasi oggetto. tra questi citiamo:

Metodo

Descrizione

**clone**

crea una copia dell’oggetto identica a quella di cui è stato invocato il metodo. In particolare è da notare che il programma Java stesso è un oggetto, quindi si possono creare un numero arbitrario di programmi tutti identici. Affinchè possa essere invocato il metodo `clone()` di un oggetto, questo deve implementare l’interfaccia **Cloneable**

getClass

restituisce un oggetto di tipo `Class`, che rappresenta la classe di appartenenza dell’oggetto di cui è stato invocato il metodo

toString

trasforma l’oggetto in una stringa, questo metodo deve essere implementasto negli oggetti creati dall’utente, ma è molto utile

Prendiamo questa istruzione:

String somma="Sommando"+10+" e "+11+", ottengo "+(10+11);

Si crea una stringa usando delle stringhe e degli `int`, automaticamente Java in questo caso ne invoca il metodo `toString()` della classe involucro corrispondente. Questo accade in ogni espressione che coinvolge stringhe e oggetti del tipo `"Stringa"+Oggetto`.

La classe Class
---------------

L’altra classe del package nominata sopra è **Class**: rappresenta le classi del linguaggio ed è molto importante perché ha più di trenta metodi che servono per gertire le classi a runtime. Oggetti di questo tipo, detti “descrittori di classe” (ma sono anche descrittori di interfacce) vengono automaticamente creati e associati agli oggetti a cui si riferiscono.

Oggetti di tipo Class, Object e altri tipi, sono molto importanti a runtime, perché permettono di gestire il programma che è in esecuzione come se fosse una collezione di dati su cui è possibile lavorare.

Java è un linguaggio dalle potenzialità impressionanti, si pensi che stesso in `java.lang`, c’è una classe chaimata **Compiler**, che contiene metodi per compilare sorgenti Java, vi sono altre classi, come `ClassLoader` e `Runtime`, che permettono di caricare nuove classi a runtime, di eseguirle, e di modificare il programma stesso mentre è eseguito, potenzialmente in Java è possibile scrivere del codice che si automodifica (che si evolve).

La classe Math
--------------

Per terminare la nostra trattazione del `java.lang`, comunque parziale vista l’immensità del package, esaminiamo brevemente la classe **java.lang.Math** (diversa dalla classe java.math). Questa classe serve per fare calcoli matematici e ha due attributi:

*   `static double E;` // è la ‘e’ di Eulero
*   `static double PI;` // è la PI Greca

I metodi sono ovviamente tutte le funzioni matematiche calcolabili, tra cui:

*   I valori assoluti di valori double, float, int e long:
*   static double abs(double a)
*   static float abs(float a)
*   static int abs(int a)
*   static long abs(long a)

Le funzioni trigonometriche

*   static double acos(double a) – arcocoseno
*   static double asin(double a) – arcoseno
*   static double atan(double a) – arcotangente
*   static double cos(double a) – coseno
*   static double sin(double a) – seno
*   static double tan(double a) – tangente

trasformazioni di angoli

*   static double toDegrees(double angrad) – converte radianti in gradi
*   static double toRadians(double angdeg) – converte gradi in radianti
*   static double atan2(double a, double b) – converte coordinate cartesiane (b,a) in coordinate polari (r,theta)

Funzioni esponenziali e logaritmiche

*   static double exp(double a) – e elevato alla a.
*   static double log(double a) – logaritmo in base e di a

Massimi e minimi tra valori double, float, int e long

*   static double max(double a, double b)
*   static float max(float a, float b)
*   static int max(int a, int b)
*   static long max(long a, long b)
*   static double min(double a, double b)
*   static float min(float a, float b)
*   static int min(int a, int b)
*   static long min(long a, long b)

Potenze e radici

*   static double pow(double a, double b) – calcola a elevato alla b, da notare che se b è <1, questa è
*   una radice (1/b)-esima di a
*   static double sqrt(double a) – calcola la radice quadrata di a.

Numeri pseudocasuali

*   static double random() – spara un numero casuale tra 0 e 1

Arrotondamenti

*   static double rint(double a) – parte intera bassa di a, è un intero
*   static long round(double a) – a arrotondato, è un long
*   static int round(float a) – a arrotondato, è un intero

Visto che abbiamo nominato il package `java.math`, diciamo che questo contiene due classi, `BigInteger` e `BigDecimal`. La prima classe serve per trattare numeri interi di grandezza arbitraria, si pensi ad esempio al calcolo del fattoriale di un numero molto grande, questo sarà un numero spaventosamente grande. `BigDecimal` fa lo stesso per numeri decimali.

Contenuto del package java.lang
-------------------------------

Interfacce

Classi

Cloneable

Boolean

Comparable

Byte, StringBuffer, String

Runnable

Character, Character.Subset, Character.UnicodeBlock,

Class, ClassLoader, Object, Package

Double, Float, Integer, Long, Short, Number

InheritableThreadLocal

Math

Compiler

Process, System, Runtime, Thread, ThreadGroup, ThreadLocal, RuntimePermission

SecurityManager, Throwable

Void

Character

Eccezioni

Errori

ArithmeticException

AbstractMethodError

ArrayIndexOutOfBoundsException

ClassCircularityError

ArrayStoreException

ClassFormatError

ClassCastException

Error

ClassNotFoundException

ExceptionInInitializerError

CloneNotSupportedException

IllegalAccessError

Exception

IncompatibleClassChangeError

IllegalAccessException

InstantiationError

IllegalArgumentException

InternalError

IllegalMonitorStateException

LinkageError

IllegalStateException

NoClassDefFoundError

IllegalThreadStateException

NoSuchFieldError

IndexOutOfBoundsException

NoSuchMethodError

InstantiationException

OutOfMemoryError

InterruptedException

StackOverflowError

NegativeArraysizeException

ThreadDeath

NoSuchFieldException

UnknownError

NoSuchMethodException

UnsatisfiedLinkError

NullPointerException

UnsupportedClassVersionError

NumberFormatException

VerifyError

RuntimeException

VirtualMachineError

SecurityException

StringIndexOutOfBoundsException

UnsupportedOperationException

Metodi della classe System
--------------------------

Metodo

Descrizione

`static long currentTimeMillis()`

restituisce il tempo in millisecondi, passato dalla mezzanotte del 01/01/1970 UTC

`static void exit(int status)`

determina l’uscita dalla Java Virtual Machine, con un codice

`static void gc()`

Java alloca tanti oggetti, e li disalloca quando non sono più usati e serve nuova memoria, per farlo usa un cosidetto **Garbage Collector**, questo metodo esegue in qualsiasi momento il garbage collector per liberare la memoria ancora allocata dal programma non usata, può essere molto utile in grosse applicazioni

`static Map<String,String> getenv()`

ritorna una collezione di coppie chiave-valore con informazioni dell’ambiente ( sistema operativo )

`static String getenv(String name)`

ritorna il valore di una particolare variabile di ambiente

`static Properties getProperties()`

fornisce informazioni sulle proprietà del sistema

`static String getProperty(String key)`

ritorna il valore della proprietà sepcificata nel parametro `key`

`static String getProperty(String key, String def)`

come la precedente ma se non esiste la proprietà ritorna un valore di default (`def`)

`static void load(String filename)`

carica in memoria il codice contenuto in nel file `filename`, che è una libreria dinamica

`static void loadLibrary(String libname)`

carica la libreria di sistema indicata da libname

`static String mapLibraryName(String libname)`

scrive il nome di una libreria in una stringa che la rappresenta

Proprietà di sistema
--------------------

Chiave

Descrizione del valore associato

java.version

Versione dell’ambiente di Java Runtime

java.vendor

Distributore dell’ambiente di Java Runtime

java.vendor.url

URL del distributore di Java

java.home

Directory dove è installato Java

java.vm.specification.version

Versione delle specifiche della Java Virtual Machine

java.vm.specification.vendor

Distributore delle specifiche della Java Virtual Machine

java.vm.specification.name

Nome delle specifiche della Java Virtual Machine

java.vm.version

Versione della implementazione della Java Virtual Machine

java.vm.vendor

Distributore della implementazione della Java Virtual Machine

java.vm.name

Nome della implementazione della Java Virtual Machine

java.specification.version

Versione dell’ambiente di Java Runtime

java.specification.vendor

Distributore dell’ambiente di Java Runtime Java Runtime

java.specification.name

Nome dell’ambiente di Java Runtime

java.class.version

Versione delle classi di Java

java.class.path

Pathname delle classi di Java

os.name

Nome Sistema Operativo

os.arch

Architettura del Sistema Operativo

os.version

Versione del sistema Operativo

file.separator

Separatore di File (“/” su UNIX, “” su Windows)

path.separator

Separatore di Path (“:” su UNIX, “;”su Windows)

line.separator

New Line (“n” su UNIX e Windows)

user.name

Account name dell’utente

user.home

Home directory dell’utente

user.dir

Working directory dell’utente