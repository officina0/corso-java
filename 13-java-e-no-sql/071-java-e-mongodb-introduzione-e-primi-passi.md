# 071 Java E Mongo DB Introduzione E Primi Passi

Nel capitolo precedente abbiamo concluso l’interazione con un database relazionale attraverso JDBC in Java. In questo capitolo introduciamo l’interazione con un database NoSQL avendo come riferimento **MongoDB** 3.4.

MongoDB è un database orientato ai documenti che si allontana dalla struttura tradizionale basata su tabelle dei database relazionali in favore di documenti di un tipo JSON-like caratterizzati da uno **schema dinamico** che MongoDB realizza con il formato **BSON** rendendo l’integrazione di dati più semplice e veloce.

Per schema dinamico intendiamo la possibilità di creare record senza una struttura predefinita, questo significa che possiamo avere record che condividono un certo numero di campi cosi come record che hanno campi in più o in meno. Possiamo semplificare la visione dei concetti fondamentali per la persistenza dei dati attraverso il seguente schema:

* Ad una tabella relazionale corrisponde una collezione in MongoDB.
* Ad un record di una tabella relazionale corrisponde un documento in una collezione MongoDB.
* Ad una colonna di una tabella relazionale corrisponde un campo \(_field_\) di un documento MongoDB.
* Operazioni di _Join_ vengono realizzate attraverso documenti collegati.

Per maggiori informazioni sull’uso di MongoDB rimandiamo alla [Guida MongoDB](http://www.html.it/guide/guida-mongodb/) e alla [documentazione](https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-windows/) sull’installazione in Windows utile per l’esecuzione del codice presentato.

Per poter utilizzare MongoDB da Java abbiamo la necessità di recuperare il relativo driver. Possiamo scaricare il `jar` da inserire nel `classpath` al seguente URL: [URL](https://mvnrepository.com/artifact/org.mongodb/mongo-java-driver/3.4.2).

Con MongoDB in esecuzione iniziamo realizzando un frammento di codice per la connessione verso un database di test già presente con l’installazione di MongoDB:

package it.html.nosql.mongo; import com.mongodb.MongoClient; import com.mongodb.client.MongoDatabase; import java.util.logging.Level; import java.util.logging.Logger;

public class MongoClientTest {

```text
public static void main(String\[\] args) {
    Logger mongoLogger = Logger.getLogger( "org.mongodb.driver" ); 
    mongoLogger.setLevel(Level.SEVERE);

    MongoClient mongoClient = new MongoClient("localhost", 27017);
    MongoDatabase database = mongoClient.getDatabase("testdb");

    for (String name : database.listCollectionNames()) {  
        System.out.println(name);
    }
    mongoClient.close();
}
```

}

Nel codice disabilitiamo preliminarmente il logging del driver MongoDB, otteniamo un client di connessione, recuperiamo un riferimento a database e listiamo tutte le collezioni presenti nel database stesso. L’esecuzione di questo primo applicativo di test potrebbe non stampare nulla a causa della possibilità che il database di test non abbia ancora collezioni definite.

Modifichiamo il codice creando una collezione `Persona` se non presente:

```text
    ..
    boolean exists = false;
    for (String name : database.listCollectionNames()) {  
        System.out.println(name);
        if(name.equals("Persona")) exists=true;
    }
    if(!exists) database.createCollection("Persona");
    mongoClient.close();
    ..
```

La collezione appena creata è priva di documenti, modifichiamo ulteriormente il codice creando un documento nella collezione `Persona` se la collezione stessa è inizialmente vuota \(metodo `count()`\):

```text
  if(!exists) database.createCollection("Persona");

    MongoCollection<Document> personaCollection = 
            database.getCollection("Persona");

    if(personaCollection.count()==0) {
        Document document = new Document();
        document.put("Nome", "Mario");
        document.put("Cognome", "Rossi");
        document.put("Età", "34");
        document.put("Lavoro", "Impiegato");
        personaCollection.insertOne(document);
    }

    for(Document document : personaCollection.find()){
        System.out.println(document);
    }
```

Se eseguimo nuovamento il codice di test dovremmo ottenere un output del tipo:

Persona Document

Che mostra la struttura JSON del documento inserito in precedenza. Facciamo notare l’uso del metodo `find()` per il recupero dei documenti presenti nella collezione.

