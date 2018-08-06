**JDBC** (_Java Database Connectivity_) è la libreria Java per accedere in modo uniforme ai database relazionali. In questo capitolo inizieremo con il vedere come configurare un driver di database.

Configurazione del driver JDBC
------------------------------

Il driver è una libreria specifica per un particolare database messa a disposizione, sotto forma di file `jar`. Per reperire un driver JDBC è quindi sufficiente visitare il sito del produttore ed effettuarne il download.

Per la trattazione di JDBC faremo uso del database **Postgres** liberamente scaricabile dal seguente: [link](https://www.postgresql.org/download/). Per il driver JDBC visitiamo invece [questa pagina](
https://jdbc.postgresql.org/download.html) e scarichiamo l’ultima versione per Java 8 (PostgreSQL JDBC 4.0 Driver, 42.1.1 alla data corrente).

La configurazione per un applicativo Java standalone è relativamente semplice, dobbiamo infatti aggiungere al `classpath` il percorso completo che punta al file `jar`. Se indichiamo con _path-to_jar_ il percorso di directory che dalla root del file system conduce al file, su un OS Unix o Unix-like lanceremo un comando simile al seguente:

export CLASSPATH=/path\_to\_jar/postgresql-42.1.1.jre6.jar

Mentre su Windows avremo:

set CLASSPATH=C:\\path\_to\_jar\\postgresql-42.1.1.jre6.jar

Durante la fase di sviluppo potremmo avere la necessità di avere a disposizione il driver di database nel `classpath`. La configurazione in questo caso dipende dall’IDE che si sta utilizzando, ad esempio nel caso di **Eclipse** possiamo accedere al _Build Path_ del progetto ed aggiungere il file come mostrato nella figura seguente:

Figura 1. Jdbc Driver

![Jdbc Driver](http://www.html.it/wp-content/uploads/2017/07/jdbc_driver.png)

Connessione al database
-----------------------

Con il driver correttamente configurato siamo in grado di stabilire una connessione di test attraverso poche linee di codice. Il primo step è comprendere la stringa di connessione del database che stiamo utilizzando, infatti la stringa relativa all’URL di connessione varia da database a database.

Per recuperare l’URL di connessione la stringa di cui abbiamo bisogno ha la seguente forma generale:

jdbc:postgresql://host:port/database

Supponendo che un database di nome `postgres` sia in esecuzione sulla macchina locale utilizzando la porta 5432, avremo per l’URL la stringa:

jdbc:postgresql://localhost:5432/postgres

Successivamente specifichiamo user e password di un utente creato in fase di installazione oppure utilizzando il client **pgadmin** di Postgres e concludiamo aprendo una connessione attraverso la classe `java.sql.DriverManager`.

Il codice per il test di connessione è quindi il seguente:

package it.html.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcDemo1 {

	public static void main(String\[\] args) throws SQLException {
		
		String url = "jdbc:postgresql://localhost:5432/postgres";
		Properties props = new Properties();
		props.setProperty("user","postgres");
		props.setProperty("password","postgres");

		Connection conn = DriverManager.getConnection(url, props);

		System.out.println("Test di connessione avvenuto con successo");
		
		conn.close();
	}

}

Nel successivo capitolo approfondiremo la gestione delle connessioni avendo come IDE di riferimento Eclipse.