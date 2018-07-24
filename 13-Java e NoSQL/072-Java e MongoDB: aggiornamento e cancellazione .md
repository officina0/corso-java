Le operazioni **CRUD**, in ambiente MongoDB si traducono nella possibilità di inserire, aggiornare e cancellare documenti all’interno di una collezione. Nel capitolo precedente abbiamo anticipato l’operazione d’inserimento, vedremo adesso come eseguire le operazioni di aggiornamento e cancellazione di un singolo documento.

Aggiornamento
-------------

Iniziamo con il creare una nuova classe di test che eseguiremo solamente dopo aver eseguito preliminarmente la classe `MongoPopulateCollectionTest` avendo cosi la certezza di avere a disposizione la collezione `Persona` con un documento al suo interno. Denominiamo la nuova classe come `MongoUpdateAndDeleteTest` e iniziamo con il classico blocco di codice per il recupero della collezione `Persona`:

	..
        Logger mongoLogger = 
               Logger.getLogger( "org.mongodb.driver" ); 
        mongoLogger.setLevel(Level.SEVERE);
		
        MongoClient mongoClient = new MongoClient("localhost", 27017);
        MongoDatabase database = mongoClient.getDatabase("testdb");
        
        MongoCollection<Document> personaCollection = 
        		database.getCollection("Persona");
        ..
   

Procediamo quindi con il codice per l’estrazione di un particolare documento:

   	..
        //Recupero di un documento
        HashMap<String,Object> filterMap = new HashMap<>();
        filterMap.put("Nome", "Mario");
        filterMap.put("Cognome", "Rossi");
        
        //Costruzione documento filtro
        Bson filter = new Document(filterMap);
        FindIterable<Document> docIterator = personaCollection.find().filter(filter);
      
        MongoCursor<Document> mongoCursor = docIterator.iterator();
        
        /*Procediamo subito con il next per questo test,
          sappiamo di trovare un solo documento*/
        Document personaDoc = mongoCursor.next();
        ..
  

Per poter recuperare un documento abbiamo l’esigenza di costruire un filtro inserendo gli opportuni campi per una identificazione univoca. Supponiamo, per semplicità, che per l’identificazione di un documento sia sufficiente specificare nome e cognome di una persona. Nel nostro caso inseriamo i parametri di filtro, nome e cognome, all’interno di una Map Java.

Successivamente creiamo un documento di filtro in formato Bson e passiamo questo documento come parametro al metodo `filter()` invocato sull’oggetto `FindIterable<Document>` ottenuto dal metodo `find()`.

In sostanza applichiamo un filtro di selezione sulla lista dei documenti presenti nella collezione. Otteniamo come risultato un iteratore sul quale possiamo invocare direttamente il metodo `next()` in quanto sappiamo di aver ottenuto un solo documento, quello corrispondente alla persona “Mario Rossi”. Una volta recuperato il documento desiderato, possiamo procedere con l’operazione di aggiornamento:

     ..
     //Costruzione documento filtro
     Bson newValue = new Document("Nome", "Aldo");
     //Costruzione documento di aggiornamento
     Bson updateDocument = new Document("$set", newValue);
     //Aggiornamento 
     personaCollection.updateOne(personaDoc, updateDocument);
     ..
    

In questo caso il documento di filtro identifica il campo che intendiamo aggiornare compreso del nuovo valore. Dobbiamo successivamente costruire un nuovo documento che rappresenta l’operazione di aggiornamento.

L’operazione di aggiornamento è specificata da `$set`, il valore di aggiornamento è invece rappresentato dal documento costruito come filtro di aggiornamento sul campo (`newValue`). Per aggiornare il documento originale, in cui compare `Nome=Mario`, con il nuovo valore `Nome=Aldo`, non dobbiamo fare altro che passare al metodo `updateOne()` della collezione Persona, il documento originale e il documento che rappresenta l’operazione di aggiornamento.

Cancellazione
-------------

Concludiamo illustrando la cancellazione di un documento. A tal fine recuperiamo il documento aggiornato ed invochiamo il metodo `deleteOne()` sulla collezione `Persona`. L’operazione è mostrata dal seguente codice che segue la falsa riga di quanto visto precedentemente:

    	..
        /\*Costruzione nuovo documento filtro\*/
        filterMap.clear();
        filterMap.put("Nome", "Aldo");
        filterMap.put("Cognome", "Rossi"); 
        filter = new Document(filterMap);
        
        /\*Recupero documento modificato\*/
        docIterator = personaCollection.find().filter(filter);
        mongoCursor = docIterator.iterator();
        
        /\*Cancellazione documento\*/
        if(mongoCursor.hasNext()) {
         personaDoc = mongoCursor.next();
         personaCollection.deleteOne(personaDoc);
         System.out.println("Documento cancellato");
        }  
        ..