# 031-Gestione date in Java 8 e il package java.time

Le **API Java 8 per la gestione delle date** risiedono nel package `java.time` e nei suoi sotto-package. Iniziamo con la classe `java.time.LocalDate` che consente di gestire una data priva di orario.

Come mostra il seguente codice, possiamo ottenere un oggetto `LocalDate`, che rappresenta una specifica data, invocando il metodo statico `of()` della classe specificando come input anno, mese e giorno. A differenza di quanto visto con la classe `Calendar`, **i mesi sono adesso indicizzati a partire dal valore 1**, fattore che rende decisamente naturale la costruzione di una data:

LocalDate localDate = LocalDate.of\(2016, 1, 1\);

Per formattare una data, la classe `LocalDate` mette a disposizione il metodo `format()` che riceve come input un oggetto della classe `java.time.format.DateTimeFormatter`. La classe `DateTimeFormatter` incapsula la logica di formattazione di una data. Ad esempio con il seguente codice creiamo un `DateTimeFormatter` per stampare date nel formato giorno/mese/anno:

DateTimeFormatter formatter = DateTimeFormatter.ofPattern\("dd/MM/yyyy"\).withLocale\(Locale.ITALY\); System.out.println\(localDate.format\(formatter\)\);

Il codice stamperà in console la stringa:

01/01/2016

Possiamo eseguire le stesse formattazioni SHORT, MEDIUM, LONG e FULL viste con la classe `java.text.DateFormat`. In questo caso le costanti di formattazione sono presenti nella classe `FormatStyle`.

DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate\(FormatStyle.FULL\).withLocale\(Locale.ITALY\); System.out.println\(localDate.format\(formatter\)\);

Espandiamo l’esempio aggiungendo alla date anche ore, minuti e secondi. Nelle nuove API l’orario è adesso rappresentato separatamente da una specifica classe: `java.time.LocalTime`. La costruzione di un suo oggetto è molto semplice, è sufficiente invocare il metodo `of()` come nel seguente esempio \(dove abbiamo specificato l’orario ore=12, minuti=30, secondi=0\):

LocalTime localTime = LocalTime.of\(12,30,0\);

Possiamo utilizzare in modo congiunto gli oggetti `LocalDate` e `LocalTime` per creare un oggetto della classe `java.time.LocalDateTime`:

LocalDateTime localDateTime = LocalDateTime.of\(localDate, localTime\);

`LocalDateTime` incapsula una data con informazioni di orario. Con `LocalDateTime` possiamo introdurre un altro interessante e completo esempio di formattazione che faccia uso di internazionalizzazione \(`Locale.ITALY`\) e fuso orario \(`TimeZone Europe/Rome`\):

ZoneId europe = ZoneId.of\("Europe/Rome"\); DateTimeFormatter formatter = DateTimeFormatter.ofPattern\("dd/MM/yyyy hh:mm:ss"\).withZone\(europe\).withLocale\(Locale.ITALY\); System.out.println\(localDateTime.format\(formatter\)\);

La classe `java.time.ZoneId` permette di definire il fuso orario\(TimeZone\):

01/01/2016 12:30:00

Possiamo quindi notare come non sia necessario ricorrere ad un’ulteriore classe come `SimpleDateFormat` per ottenere formattazioni più complesse. Le classi viste mettono a disposizione diversi metodi per recuperare singoli campi di una data e metodi per manipolare date e orari ottenendo nuovi oggetti.:

localDateTime.getDayOfMonth\(\); localDateTime.getMonth\(\); localDateTime.getYear\(\); localDateTime.getHour\(\); localDateTime.getMinute\(\); localDateTime.getSecond\(\);

Quindi, se volessimo aggiungere 5 minuti alla data di esempio:

localDateTime = localDateTime.plusMinutes\(5\);

Oltre alle date e agli orari il package `java.time` offre una visione continua del tempo attraverso la classe `Istant`. Un suo oggetto rappresenta il numero di secondi dalla mezzanotte del 1 Gennaio 1970 UTC fornendo una precisione dell’ordine dei nanosecondi e può essere utilizzata in sostituzione di `System.currentTimeMillis()`. Volendo calcolare il tempo richiesto per processare un blocco di istruzioni scriveremmo qualcosa del tipo:

Instant start = Instant.now\(\); Instant end = Instant.now\(\); int timeElapsed = end.getNano\(\)-start.getNano\(\);

