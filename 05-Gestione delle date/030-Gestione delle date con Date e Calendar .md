Delle varie classi di utilità presenti nel package `java.util` delle API Java descriviamo ora `Date` e `Calendar`. Un oggetto della classe `Date` rappresenta uno specifico istante di tempo con una precisione al millisecondo e, nella programmazione in Java, si utilizza frequentemente per rappresentare delle date.

Questa classe rappresenta un intervallo di tempo espresso in millisecondi che va dal 1 gennaio 1970 al momento di creazione dell’oggetto `Date`. La classe `Date` ha diversi metodi per la manipolazione dello stato di un’istanza che sono però deprecati e non devono essere utilizzati.

Nella costruzione delle date l’utilizzo congiunto con la classe `Calendar` consente di creare correttamente oggetti `Date`. Partendo dalla classe `Date` possiamo utilizzare il costruttore senza argomenti e il costruttore con argomento `long`, che esprime il tempo in millisecondi, per creare correttamente istanze di `Date`:

	Date data1 = new Date();
	Date date2 = new Date(System.currentTimeMillis());
 

Entrambe le linee di codice istanziano un oggetto che incapsula la data corrente. Tutti gli altri costruttori, che consentono maggiore flessibilità nella costruzione di una data, sono deprecati, questo significa che il metodo corretto per costruire una data risiede nell’uso di altre classi.

Iniziamo con l’analizzare la classe `DateFormat` che consente di ottenere un rappresentazione stringa di una data e, viceversa, un oggetto `Data` da una stringa formattata correttamente per rappresentare una data. `DateFormat` consente la gestione dei seguenti formati di data:

Tipo

Descrizione

**SHORT**

dd/mm/yy

**MEDIUM**

dd-prefisso del mese-yyy

**LONG**

dd \[nome completo del mese\] yyyy

**FULL**

nome completo del giorno\] dd \[nome completo del mese\] yyyy

Dove la lettera `d` indica una cifra del giorno, la lettera `m` una cifra del mese e la lettera `y` una cifra dell’anno. Alcuni esempi che rispettano i formati sono i seguenti:

03/09/16
3-set-2016
3 settembre 2016
sabato 3 settembre 2016

Partiamo dalla classe `Calendar` ottenendo una sua istanza con `java.util.TimeZone(fuso orario)` riferito a Europe-Rome e località `java.util.Locale.ITALY(internazionalizzazione)`, creando successivamente un oggetto `java.util.Date` attraverso l’istanza `java.util.Calendar` appena ottenuta:

 Calendar calendar = Calendar.getInstance(TimeZone.getTimeZone("Europe/Rome"),Locale.ITALY);
 Date today = calendar.getTime();
 

Per comodità definiamo un array per i tipi di formato offerti dalla classe `DateFormat`:

   int\[\] formats = {DateFormat.SHORT, DateFormat.MEDIUM, DateFormat.LONG, DateFormat.FULL};

Utilizziamo l’array `formats` per stampare sulla console tutti i formati dell’oggetto Date `today`. La stampa dei formati avviene attraverso la costruzione di un oggetto `DateFormat` per il formato corrente ed invocando su questo oggetto il metodo `format()` che accetta un Date come parametro:

 for(int format : formats){
    DateFormat dateFormat =DateFormat.getDateInstance(format, Locale.ITALY);
    System.out.println(dateFormat.format(today));
 }
 

Il codice produrrà un output simile a quello appena visto per la data 03/09/2016.

Vediamo ora come creare oggetti `Date` che facciano riferimento ad una data qualsiasi. Un possibile metodo consiste nel definire una stringa rispettando i formati visti in precedenza ed utilizzando `DateFormat` per ricavarne un oggetto `Date`:

    String myDateStr="01/01/16";
    try {
     DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.SHORT, Locale.ITALY);
     Date myDate = dateFormat.parse(myDateStr);
     System.out.println(dateFormat.format(myDate));
    } catch (ParseException e) {
      e.printStackTrace();
    }

Un metodo migliore è quello di manipolare l’oggetto `Calendar` per variare la data che rappresenta:

   calendar.set(2016, 0, 1);
   Date myDate = calendar.getTime();

Il frammento di codice cambia la data sull’oggetto `Calendar` al valore 01/01/2016. Il primo parametro del metodo `set()` rappresenta l’anno, il secondo il mese indicizzato partendo dal numero zero (Gennaio=0, Febbraio=1,.. Dicembre=11), il terzo il giorno del mese.

La classe `Calendar` ha diversi metodi utili che si suggerisce di esplorare. Ad esempio possiamo sottrarre giorni ad un data ottenendone una diversa:

   calendar.add(Calendar.DAY\_OF\_MONTH, -5);

Il limite della formattazione delle date con `DateFormat` risiede nella poca flessibilità sul formato stringa consentito. Il package `java.text`, in cui risiede DateFormat, fornisce la classe `SimpleDateFormat` che ci consente di superare tale limitazione grazie alla varietà di pattern utilizzabili. Ad esempio, supponiamo di voler stampare un oggetto `Date` secondo il formato dd/MM/yyyy, sappiamo che con `DateFormat` ciò non è possibile, ma con `SimpleDateFormat` lo è:

SimpleDateFormat simpleDateFormat = new SimpleDateFormat("dd/MM/yyyy");
System.out.println(simpleDateFormat.format(calendar.getTime()));

Come vedremo nel successivo capitolo, con Java 7 e 8, la gestione delle date è state rivista per far fronte ai seguenti problemi:

*   Le date date sono gestite con oggetti mutabili, mentre si desidera in genere che una data sia immutabile.
*   I mesi sono indicizzati a partire da 0: questa indicizzazione non è naturale è genera confusione.
*   `Date` e `Calendar` **non sono thread-safe** e quindi inadatte ad essere utilizzate in applicazioni concorrenti.