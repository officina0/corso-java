Eseguire query SQL con JDBC significa essenzialmente utilizzare  
le classi `Statement`, `PreparedStatement` e `ResultSet` delle API  
all’interno di codice Java.

Piuttosto che addentrarci in esempi sparsi  
di utilizzo di queste classi cerchiamo di progettare una semplice  
architettura di accesso al database che ne incorporerà l’utilizzo. Il  
nostro obiettivo è realizzare una classe che incapsuli tutta la  
gestione di apertura delle connessioni ed il recupero di query da file  
applicativi. I benefici di questa progettazione saranno  
chiari quando introdurremo il pattern DAO  
(_Data Access Object_).

Il nostro scopo iniziale è inserire in un file  
di properties tutti i parametri di connessione verso il database  
Postgres, in questo modo avremo la libertà di impostarli in modo molto  
semplice e leggibile senza toccare il codice Java.

La classe di  
gestione dovrà quindi accedere al file e recuperare i  
valori necessari. Sebbene sia possibile costruire query SQL  
all’interno di codice Java attraverso concatenazione di stringhe, è  
una pratica che ci sentiamo di non incoraggiare per almeno quattro  
motivi:

1.  **Leggibilità**: query complesse sono di difficile lettura per  
    altri programmatori.
2.  **Bug fixing**: difficoltà nell’individuare errori logici o  
    sintattici.
3.  **Manutenibilità**: difficoltà nell’estrarre una query dal codice  
    per l’escuzione in un client di database o per la collaborazione con un DBA.
4.  **Visibilità di insieme**: se le query sono all’interno del  
    codice, dove sono collocate? Quali sono le classi che le usano?

Per queste ragioni preferiamo indicare una soluzione che porta  
ad inserire le query SQL in un file specifico che  
può essere di qualunque tipo: XML, properties o TXT. Per la  
soluzione mostrata nel presente capitolo, e nel successivo,  
utilizzeremo un file di properties.

Indichiamo con il nome di `DBManager` la classe alla quale  
affidiamo il caricamento dei file di properties `db.properties` e  
`query.properties`. Questi file sono collocati nel package  
`it.html.jdbc.dao` e contengono, rispettivamente, i parametri di  
connessione al database e tutte le stringhe di interrogazione SQL ed  
operazioni CRUD.

La classe `DBManager`, di cui sarà possibile recuperare una sola istanza, è la seguente:

```
public class DBManager {
	private static DBManager dbManager;
	private Properties queryProperties;
	private Properties dbProperties;
	private String postgresUrl;
	private String user;
	private String password;
	private DBManager() throws DBManagerException {
		try (InputStream dbPropFile = getClass().getResourceAsStream("db.properties");
			 InputStream queryPropFile = getClass().getResourceAsStream("query.properties");) {
			queryProperties = new Properties();
			dbProperties = new Properties();
			dbProperties.load(dbPropFile);
			queryProperties.load(queryPropFile);
			postgresUrl = "jdbc:postgresql://" + dbProperties.getProperty("host") + ":"
					+ dbProperties.getProperty("port") + "/" + dbProperties.getProperty("dbName");
			user = dbProperties.getProperty("user");
			password = dbProperties.getProperty("password");
		} catch (IOException e) {
			throw new DBManagerException(e);
		}
	}
	public static DBManager getInstance() throws DBManagerException {
		if (dbManager == null)
			dbManager = new DBManager();
		return dbManager;
	}
    public Connection getConnection() throws SQLException{
		return DriverManager.getConnection(postgresUrl, user, password);
    }
	public String getQuery(String queryId) {
		return queryProperties.getProperty(queryId);
	}
}
```

Il costruttore della classe legge i file di properties salvando  
i riferimenti nelle variabili di istanza `queryProperties` e  
`dbProperties`. Con i dati del file `db.properties` viene costruita la  
stringa di connessione al database e salvati `user` e `password`  
all’interno delle relative variabili di istanza.

Il metodo  
`getIstance()` ritorna l’unica istanza sulla quale possiamo invocare il  
metodo `getConnection()`, per ottenere una connessione, ed il metodo  
`getQuery()` per recuperare la stringa di una query.