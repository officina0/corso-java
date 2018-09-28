Testi, messaggi e codici sono solo alcune delle applicazioni che hanno le **stringhe** in programmazione. In Java esse sono rappresentate come sequenze di caratteri unicode ([UTF-16](http://docs.oracle.com/javase/6/docs/api/java/lang/Character.html#unicode "Per approfondire il formato della rappresentazione unicode")) e possiamo crearle e manipolarle grazie alla classe **String**, messa a disposizione nel core di Java (`java.lang.String`).

Vediamo quindi come dichiarare le stringhe ed effettuare su di esse le operazioni più classiche.

Definire una stringa in Java
----------------------------

Il modo più semplice e diretto per creare un oggetto di tipo `String` è assegnare alla variabile un insieme di caratteri racchiusi fra virgolette:

```
String titolo = "Lezione sulle stringhe";
```

questo è possibile in quanto il compilatore crea una variabile di tipo String ogni volta che incontra una sequenza racchiusa fra doppi apici; nell’esempio la stringa `"Lezione sulle stringhe"` viene trasformata in un oggetto String e assegnato alla variabile `titolo`. Questa forma di inizializzazione è detta **string literal**

Oltre alla modalità “literal”, poiché si tratta comunque di oggetti, le variabili di tipo String possono essere inizializzate anche utilizzando la keyword **new** e un costruttore come si vede nell’esempio seguente:

```
public void initString() {
	// Inizializzazione con una new
	String titolo = new String("Titolo dell'opera");
	// Inizializzazione che fa uso di un array di caratteri
	char[] arraySottotitolo = {'s','o','t','t','o','t','i','t','o','l','o','!'};
	String sottotitolo = new String(arraySottotitolo);
}
```

Dalla versione 8 di Java, String conta ben 13 costruttori (oltre a 2 deprecati ed una decina di metodi statici che in qualche modo creano istanze di tipo stringa a partire da altri tipi di variabili).

Length, la lunghezza di una stringa
-----------------------------------

La classe String espone anche numerosi metodi per l’accesso alle proprietà della stringa sulla quale stiamo lavorando; uno di questi è il metodo **length()**, che ritorna il numero di caratteri contenuti nell’oggetto. La sua signature è:

```
int length()
```

Per esempio le seguenti linee di codice:

```
public void printLength() {
	String descrizione = "Articolo sulle stringhe ...";
	int length = descrizione.length();
	System.out.println("Lunghezza: "+length);
}
```

stampano come risultato:

```
Lunghezza: 27
```

In questo esempio è interessante notare anche come il compilatore interpreti automaticamente l’espressione `"Lunghezza: "+length`, creando un oggetto di tipo String ottenuto concatenando la stringa che troviamo in prima posizione e la stringa ottenuta dalla rappresentazione decimale del valore di `length` (variabile di tipo `int`).

In sostanza viene svolta per noi una conversione di tipo senza la quale avremmo dovuto scrivere qualcosa di questo genere:

```
"Lunghezza: " + String.valueOf(length);
```

Il metodo (statico) **valueOf** di String, restituisce la rappresentazione testuale del parametro di tipo `int` che riceve in ingresso.

Stringa = array di caratteri
----------------------------

Possiamo pensare a una stringa esattamente come a un **array di caratteri**, questo significa che possiamo considerare i singoli caratteri come elementi di array. Consideriamo questa stringa:

```
String str = "Ciao HTML.it";
```

Il carattere `'C'` è alla posizione `0`, il carattere `'i'` è alla posizione `1`, … il carattere `'t'` finale è alla posizione `11`, che coincide con `str.length()-1`.

Per **accedere ai singoli caratteri** non possiamo usare l’operatore ‘`[]`‘ come negli array, ma possiamo sfruttare il metodo **charAt**:

```
char charAt(int index);
```

### utf-16

Se vogliamo utilizzare le stringhe come array dovremo porre la massima attenzione alla rappresentazione **unicode/utf-16** che prevede che i cosiddetti “_supplementary characters_” occupino 2 posizioni nell’array; magari questo non ha molta rilevanza con il nostro sistema locale ma le cose potrebbero diventare complicate.

Concatenare le stringhe
-----------------------

L’operazione di concatenazione di stringhe può essere effettuata in modi diversi. La classe String fornisce il metodo **concat** per la concatenazione di stringhe la cui signature è:

```
String concat(String str);
```

Quindi:

```
String str1 = new String("Nome ");
String str2 = new String("Cognome ");
String str3 = str1.concat(str2);
```

assegna a `str3` una nuova stringa formata da `str1` con `str2` aggiunto alla fine; insomma `"Nome Cognome"`.

Avremmo potuto ottenere la stessa cosa in modopiù semplice utilizzando l’operatore ‘`+`‘:

```
String str1 = "Nome";
String str2 = "Cognome";
String str3 = str1+str2;
```

Oppure avremmo potuto costruire la stringa concatenata direttamente tramite literals:

```
String str3 = "Nome"+"Cognome";
```

Substring, estrarre una sottostringa
------------------------------------

Per prelevare e manipolare solo una porzione di una stringa possiamo utilizzare il metodo **substring**, presente in 2 forme (overloaded):

```
String substring(int beginIndex);
String substring(int beginIndex, int endIndex);
```

La prima ritorna una stringa (sottostringa di quella di partenza) a partire dall’indice specificato fino alla fine della stringa; la seconda invece, ritorna una stringa che è anch’essa sottostringa di quella di partenza, ma che parte dall’indice `beginIndex` e termina in `endIndex`. Per esempio:

```
String titolo = "I promessi Sposi";
String a = titolo.substring(2);   // a vale "promessi Sposi"
String b = titolo.substring(12);  // b vale "Sposi"
String c = titolo.substring(2,9); // c vale "promessi"
```

**Nota:** _sia l’operazione di concatenamento sia quella di estrazione di una sottostringa (e tutti i metodi che operano sulle stringhe per la verità), sono caratterizzati dal fatto di non modificare la stringa su cui vengono applicate ma di ritornarne una nuova. Ad esempio `titolo.substring(12)` non modifica `titolo` ma ritorna una nuova variabile di tipo `String` che contiene la sottostringa `"Sposi"`;_

### Stringhe, oggetti “immutabili”

Anche se cercassimo con attenzione non troveremmo come fare l’operazione di ‘estrazione’ direttamente su una stringa: in Java **le stringhe sono oggetti immutabili**, cioè il loro valore non può essere cambiato dopo la loro creazione (come gli array non possono cambiare lunghezza per fare un parallelo).

L’immutabilità dell’oggetto String deve sempre essere tenuta presente ogni volta le si manipolano, non è infatti infrequente cadere in errori come questo:

```
String messaggio = "Ciao XX";
messaggio.replace("XX", "Mondo");
System.out.println(messaggio);
```

nel quale semplicemente il risultato dell’operazione di sostituzione è non utilizzato. Possiamo comunque assegnare il nuovo oggetto literal allo stesso riferimento:

```
messaggio = messaggio("XX", "Mondo");
```

Ma questo significa abbandonare l’oggetto precedente. In altre parole avremo nella memoria il nuovo oggetto `"Ciao Mondo"` puntato dalla variabile `messaggio` e l’oggetto `"Ciao XX"` abbandonato a se stesso senza riferimenti.

Per modificare il contenuto di una stringa di caratteri è consigliabile utilizzare le classi **StringBuffer** o **StringBuilder** che, al contrario di `String`, possono essere modificati senza lasciare oggetti inutilizzati e secondo i casi possono risultare quindi assai piu’ preformanti (e comodi).

I metodi per manipolare le stringhe
-----------------------------------

Oltre al `replace`, la classe `String` mette a disposizione molti altri metodi per manipolare le stringhe, esaminiamone alcuni:

Metodo

Descrizione

`boolean **contains**(CharSequence s)`

ritorna true se e solo se la stringa contiene la sequenza di caratteri specificati dal parametro `s`

`boolean **equals**(Object anObj)`

confronta la stringa con l’oggetto obj specificato

`boolean **isEmpty**()`

ritorna `true` se e solo se la lunghezza della stringa è `0`

`String[] **split**(String regex)`

suddivide la stringa intorno ad ogni occorrenza con l’espressione `regex` e ritorna array con tutte le sottostringhe

`String **trim**()`

ritorna una copia della stringa di partenza eliminando tutti gli spazi bianchi all’inizio e alla fine della stringa

Una lista completa sarebbe lunghissima ed è consultabile nella [documentazione ufficiale](http://docs.oracle.com/javase/8/docs/api/java/lang/String.html "titolo - link esterno") di Oracle.