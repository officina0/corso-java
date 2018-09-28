Può capitare che la logica di interazione con un database  
risieda all’interno di una **stored procedure** definita all’interno del  
database stesso e di avere l’esigenza di invocarla all’interno del  
codice Java di un’applicazione.

JDBC fornisce il supporto  
all’invocazione di stored procedures attraverso la classe  
`CallableStatement`. Vediamo un semplice esempio di procedura con soli  
parametri di input.

Supponiamo di disporre di una stored procedure per  
l’inserimento di un record nella tabella `Persona`, sul database  
Postgres potremmo definire tale procedura attraverso il seguente  
codice SQL:

```
CREATE LANGUAGE plpgsql;
    CREATE OR REPLACE FUNCTION add_persona(id INTEGER, nome VARCHAR(70), cognome VARCHAR(70), professione VARCHAR(70), eta INTEGER)
    RETURNS void AS $$
    BEGIN
      INSERT INTO persona VALUES (id, nome, cognome, professione, eta);
    END;
    $$ LANGUAGE plpgsql;
```

Inseriamo la sintassi per l’invocazione della procedura  
all’interno del file `query.properties` secondo lo schema architetturale  
introdotto nei capitoli precedenti:

```
inserisci_persona_proc={call add_persona(?,?,?,?,?)}
```

La sintassi generale prevede l’utilizzo delle parentesi graffe per  
racchiudere la stringa di chiamata, segue poi la parola chiave `call`, il nome della procedura e infine la lista di parametri di input ciascuno specificato  
attraverso un punto interrogativo.

La richiesta di esecuzione della procedura  
`add_persona()` è molto semplice, la illustriamo definendo un nuovo  
metodo all’interno della classe `PersonaDAO` con il nome  
`inserisciPersonaProc()`:

```
public void inserisciPersonaProc(Persona persona) throws DAOException {
		Transaction transaction = new Transaction() {
			@Override
			public void buildStatements(Connection conn) throws DAOException {
				try {
					CallableStatement cs = conn.prepareCall(getQuery("inserisci_persona_proc"));
					cs.setInt(1, persona.getId());
					cs.setString(2, persona.getNome());
					cs.setString(3, persona.getCognome());
					cs.setString(4, persona.getProfessione());
					cs.setInt(5, persona.getEta());
					cs.execute();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new DAOException(e);
				}
			}
		};
		transaction.execute();
	}
```

Il primo passaggio è ottenere un `CallableStatement` utilizzando  
la stringa di chiamata inserita nel file `query.properties`,  
successivamente definiamo i parametri di input in modo simile a quanto  
fatto con oggetti `Statement`, ed infine eseguiamo la chiamata con il  
metodo `execute()` all’interno della transazione corrente.

Prima di  
eseguire il codice di chiamata di test all’interno della classe  
`DAOTest`, modifichiamo il blocco `finally` all’interno del metodo  
`execute()` della classe `Transaction` nel seguente modo:

```
finally {
      try {
          if(statements != null) {
             for (PreparedStatement st : statements) {
                  st.close();
             }
          }
          if (conn != null) conn.close();
      } catch (SQLException e) {
          e.printStackTrace();
      }
      if (statements != null) statements.clear();
    }
```

Il cambiamento consiste nel gestire una lista di Statements non  
inizializzata nel caso di invocazioni di procedure di database.

In Postgres sono possibili sia procedure che funzioni, entrambe prevedono la sintassi di creazione  
vista precedentemente, ciò che definisce effettivamente una procedura è il tipo `void` da esso restituito.  
Nel caso di una funzione il tipo restituito è chiaramente un tipo del linguaggio lato database.

In altri database,  
come ad esempio Oracle, la differenza è più netta a livello di creazione con parole chiave che identificano  
procedure e funzioni.