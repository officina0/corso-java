Dopo aver analizzato la procedura base per il recupero di documenti, vediamo come poter applicare ulteriori tecniche di filtro sui dati.  
MongoDB mette a disposizione diversi operatori di query, per la nostra trattazione vedremo esempi d’uso  
degli operatori:

```
$gt=greater than
$lt=less than
$eq=equal
$ne=not equal
```

Per utilizzare questi operatori, al fine di costruire filtri per le query, dobbiamo avere come riferimento la costruzione  
di un documento di filtro basato su di essi:

```
..
      Bson condition = new Document(operatore, valore);
      Bson filter = new Document(campo, condition);
      ..
```

Seguendo la falsa riga degli esempi precedenti, costruiamo una nuova classe di test a cui diamo il nome `MongoQueryTest`.  
Iniziamo quindi il suo editing con il codice iniziale che popola la collezione `Persona` con tre documenti:

```
..
  Logger mongoLogger =
  Logger.getLogger( "org.mongodb.driver" );
  mongoLogger.setLevel(Level.SEVERE);
  MongoClient mongoClient = new MongoClient("localhost", 27017);
  MongoDatabase database = mongoClient.getDatabase("testdb");
  MongoCollection<Document> personaCollection = database.getCollection("Persona");
  if(personaCollection.count()==0) {
	        Document document1 = new Document();
	        document1.put("Nome", "Mario");
	        document1.put("Cognome", "Rossi");
	        document1.put("Età", 34);
	        document1.put("Lavoro", "Impiegato");
	        Document document2 = new Document();
	        document2.put("Nome", "Luigi");
	        document2.put("Cognome", "Binachi");
	        document2.put("Età", 45);
	        document2.put("Lavoro", "Avvocato");
	        Document document3 = new Document();
	        document3.put("Nome", "Laura");
	        document3.put("Cognome", "Belli");
	        document3.put("Età", 28);
	        document3.put("Lavoro", "Commercialista");
	        personaCollection.insertOne(document1);
	        personaCollection.insertOne(document2);
	        personaCollection.insertOne(document3);
    }
   ..
```

Proviamo ad estrarre tutte le persone che lavorano come Impiegati:

```
..
    Bson condition = new Document("$eq", "Impiegato");
    Bson filter = new Document("Lavoro", condition);
    for(Document document :  personaCollection.find(filter)){
        System.out.println(document);
    }
    ..
```

Con il risultato:

```
Document{{_id=59774eba11ded10dc431d751, Nome=Mario, Cognome=Rossi, Età=34, Lavoro=Impiegato}}
```

Ora le persone con età superiore ai 28 anni:

```
..
    Bson condition = new Document("$gt", 28);
    Bson filter = new Document("Età", condition);
    for(Document document :  personaCollection.find(filter)){
        System.out.println(document);
    }
    ..
```

La stampa in questo caso sarà la seguente:

```
Document{{_id=59774eba11ded10dc431d751, Nome=Mario, Cognome=Rossi, Età=34, Lavoro=Impiegato}}
Document{{_id=59774eba11ded10dc431d752, Nome=Luigi, Cognome=Binachi, Età=45, Lavoro=Avvocato}}
```

Passiamo alle persone con età inferiore ai 40 anni:

```
..
    Bson condition = new Document("$lt", 40);
    Bson filter = new Document("Età", condition);
    for(Document document :  personaCollection.find(filter)){
        System.out.println(document);
    }
    ..
```

Con risultato stampato in console:

```
Document{{_id=59774eba11ded10dc431d751, Nome=Mario, Cognome=Rossi, Età=34, Lavoro=Impiegato}}
   Document{{_id=59774eba11ded10dc431d753, Nome=Laura, Cognome=Belli, Età=28, Lavoro=Commercialista}}
```

E, infine, le persone che non sono degli impiegati:

```
..
    Bson condition = new Document("$ne", "Impiegato");
    Bson filter = new Document("Lavoro", condition);
    for(Document document :  personaCollection.find(filter)){
        System.out.println(document);
    }
    ..
```

Che mostrerà il risultato:

```
Document{{_id=59774eba11ded10dc431d752, Nome=Luigi, Cognome=Binachi, Età=45, Lavoro=Avvocato}}
Document{{_id=59774eba11ded10dc431d753, Nome=Laura, Cognome=Belli, Età=28, Lavoro=Commercialista}}
```

Con questo capitolo concludiamo l’introduzione a MongoDB in ambiente Java ricordando che tutti gli esempi mostrati trovano il codice completo in allegato.