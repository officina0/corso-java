Abbiamo concluso il precedente capitolo con un semplice programma per aprire una connessione verso Postgres. L’apertura di una connessione, ma anche altre operazioni come vedremo in seguito, possono provocare delle **SQLException** per errori come l’impossibilità di comunicare con il database, credenziali non corrette, statement di interrogazione o modifica costruiti in modo errato.

Queste eccezioni devono essere gestite correttamente per evitare il crash dell’applicazione e possibili perdite di dati. Vediamo due possibili tecniche di gestione partendo da quella probabilmente più conosciuta.

try-catch
---------

Modifichiamo il codice del programma precedente come segue:

package it.html.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcDemo2 {

	public static void main(String\[\] args) {

		String url = "jdbc:postgresql://localhost:5432/postgres";
		Properties props = new Properties();
		props.setProperty("user", "postgres");
		props.setProperty("password", "postgres");

		Connection conn = null;

		try {
		    conn = DriverManager.getConnection(url, props);
			System.out.println("Test di connessione avvenuto con successo");
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
}

Questa tecnica di gestione prevede l’utilizzo di blocchi **try-catch** innestati per la gestione delle eccezioni e la corretta chiusura delle risorse utilizzate.

Preliminarmente dobbiamo dichiarare ed inizializzare a `null` gli oggetti relativi alle risorse che andremo ad utilizzare, nel nostro esempio è presente soltanto un oggetto `Connection`. Successivamente racchiudiamo in un blocco `try-catch` il tentativo di apertura di una connessione. Nel caso di esito negativo avremo una `SQLException` che verrà gestita attraverso il relativo blocco `catch`.

Il blocco `final` viene eseguito sia in caso di esito positivo che negativo, al suo interno trova quindi posto il rilascio della connessione con il metodo `close()`. Anche `close()` può lanciare una `SQLException`, per questa ragione deve qessere racchiuso in un blocco `try-catch`.

Riassumendo lo schema generale per la gestione delle eccezioni JDBC è del tipo:

try {
 //Esecuzione task di database
} catch (SQLException e) {
 //Gestione eccezioni
} finally {
  try {
   //Rilascio Connessioni e Statements
  } catch(SQLException e) {
   //Gestione eccezioni
  }
}

try-with-resources
------------------

In Java 8 abbiamo la possibilità di utilizzare un metodo alternativo che si differenzia dal precedente per la gestione automatica del rilascio delle risorse. Riprendiamo il codice del programma e modifichiamolo nel modo seguente:

package it.html.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcDemo3 {

	public static void main(String\[\] args) {

		String url = "jdbc:postgresql://localhost:5432/postgres";
		Properties props = new Properties();
		props.setProperty("user", "postgres");
		props.setProperty("password", "postgres");

		try ( Connection conn = DriverManager.getConnection(url, props);) {	
			System.out.println("Test di connessione avvenuto con successo");
		} catch (SQLException e) {
			e.printStackTrace();
		} 
	}
}

Questa soluzione prevede di inserire la creazione di connessioni e statements a livello del blocco `try` ed è nota con il nome di `try-with-resources`.

In questo caso non abbiamo più bisogno del blocco `finally` in quanto la chiusura delle risorse utilizzate avverrà automaticamente. Con Java 8 possiamo scrivere del codice più compatto rispetto alle versioni precedenti. In allegato trovate il codice sorgente degli esempi JDBC all’interno del package `it.html.jdbc`