Come abbiamo già detto nella lezione precedente, in Java per ogni tipo primitivo esiste un corrispondente **Simple Data Object** o, come si suol dire, una **Classe Wrapper**.

Dato un tipo primitivo (i cui nomi iniziano tutti rigorosamente con la prima lettera minuscola) si ottiene il corrispondente Data Object sostanzialmente capitalizzando il nome come mostrato nella tabella seguente:

Tipo primitivo

Classe Wrapper

byte

Byte

short

Short

int

Integer

long

Long

float

Float

double

Double

char

Character

boolean

Boolean

Anche se ad un primo sguardo può sembrare che ci sia poca differenza tra un tipo primitivo e la sua controparte ‘wrapped’ (anche e spesso detta ‘boxed’) tra le due c’è una fondamentale distinzione: i tipi primitivi non sono oggetti e non hanno associata alcuna classe e quindi devono essere trattati in modo diverso rispetto agli altri tipi (ad esempio non è possibile utilizzarli nelle collezioni, che saranno argomento di future lezioni) e non possono avere metodi.

Per ovviare a questa distinzione Java mette dunque a disposizione delle classi preconfezionate per contenere, **“wrappare” i tipi primitivi**. Possiamo infatti pensare ad una classe wrapper esattamente come un involucro (wrap) che ha l’unico scopo di contenere un valore primitivo rendendolo da un lato un oggetto e dall’altro “ornandolo” con metodi che altrimenti non avrebbero una loro naturale collocazione.

Tutte le classi wrapper sono definite nel package _java.lang_ e sono qualificate come `final`, perciò non è possibile derivare da loro. Inoltre tutte queste classi sono immutabili, cioè non è possible dopo la costruzione cambiarne il valore.

Mentre `Boolean` e `Character` derivano direttamente da `Object`, tutti i Data Object di tipo numerico derivano da **Number**, che a sua volta è un discendente diretto di `Object`.

![](https://tbm-html.s3.amazonaws.com/app/uploads/2014/09/wrapper.png)

Dai tipi semplici ai Data Object
--------------------------------

Anche se Java 1.5 ha introdotto il concetto di ‘autoboxing/unboxing’, che approfondiremo in una apposita lezione, passare da un tipo primitivo alla sua versione wrappata è in linea di principio semplice ma laborioso:

```
int val = 44;
//dato un tipo primitivo si crea la classe wrapper
Integer value = new Integer(val); 
//dalla classe wrapper è possibile "estrarre" il valore
int valueBack = value.intValue();
```

Metodi speciali per il parsing
------------------------------

Le classi wrapper sono in Java anche il posto in cui trovano posto gli utilissimi metodi che servono per fare il parsing di stringe e convertirle in valori numerici, ad esempio:

```
String quarantatre = "43";
Integer q = new Integer(quarantatre);
```

Quando utilizziamo questi metodi per la conversione occorre gestire una eventuale generazione di errori. Infatti non tutte le possibili sequenze di caratteri sono numeri, prendiamo ad esempio il seguente snippet:

```
String quarantaquattro = "quarantaquattro";
Integer q = new Integer(quarantaquattro);
```

Il parser non sarebbe in grado di processare con successo la stringa e otterremmo un errore che la JVM segnala attraverso una eccezione di tipo [NumberFormatException](http://docs.oracle.com/javase/8/docs/api/java/lang/NumberFormatException.html) (più avanti approfond

String
------

Tra i tipi wrapper potremo annoverare anche il tipo String che con i Simple Data Object condivide moltissime caratteristiche (ad esempio l’immutabilità). Ma data l’importanza delle stringhe riserveremo a loro un’intera lezione.

Link utili

*   Documentazione del [costruttore Integer(String)](http://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#Integer-java.lang.String-).