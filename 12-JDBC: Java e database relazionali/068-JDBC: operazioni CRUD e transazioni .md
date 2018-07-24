Le operazioni di inserimento, cancellazione e aggiornamento di record sono note con il nome di operazioni **CRUD** (_Create_, _Read_, _Update_, _Delete_). In linea di principio è lecito seguire quanto fatto nei capitoli precedenti per le normali query: creare ed eseguire uno Statement (o **PreparedStatement**) facendo uso di una query SQL di tipo CRUD in luogo di una query di interrogazione.

Il risultato è l’apertura automatica di una transazione verso il database per l’esecuzione del nostro statement. Nella situazione di default una connessione aperta ha il flag di COMMIT impostato sul valore `AUTO`, questo significa che ogni operazione CRUD viene eseguita con un COMMIT automatico all’interno di una propria singola transazione.

Tale comportamento non è accettabile nel caso in cui una transazione coinvolga più statement. L’obiettivo del presente capitolo è quindi quello di modificare la classe `AbstractDAO` in modo tale che la gestione di una transazione sia centralizzata nella classe stessa, ed ereditata dalle varie sottoclassi `DAO`.

Per incapsulare la logica di gestione di una transazione, possiamo realizzare una classe membro all’interno di `AbstractDAO` contenente il seguente codice:

     protected abstract class Transaction {

		private List<Statement> statements;
		private Connection conn = null;

		public void execute() throws DAOException {
			try {
				conn = getConnection();
				boolean autocommit = conn.getAutoCommit();
				int isolation = conn.getTransactionIsolation();
				
				conn.setAutoCommit(false);
				conn.setTransactionIsolation(Connection.TRANSACTION\_REPEATABLE\_READ);
				
				buildStatements(conn);

				conn.commit();
				conn.setAutoCommit(autocommit);
				conn.setTransactionIsolation(isolation);

			} catch (SQLException e) {
				e.printStackTrace();
				try {
					if (conn != null)
						conn.rollback();
					throw new DAOException(e);
				} catch (SQLException e1) {
					e1.printStackTrace();
					throw new DAOException(e1);
				}
			} finally {

				try {
					for (Statement st : statements) {
						st.close();
					}
					if (conn != null) conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}

				if (statements != null) statements.clear();
			}
		}
		
		public PreparedStatement getPreparedStatement(String query) throws DAOException {
			if (statements == null)
				statements = new ArrayList<PreparedStatement>();
			PreparedStatement statement;

			try {
				statement = conn.prepareStatement(query);
				statements.add(statement);
			} catch (SQLException e) {
				e.printStackTrace();
				throw new DAOException(e);
			}
			return statement;
		}

		public abstract void buildStatements(Connection conn) throws DAOException;
	}
	

La classe è dotata di un campo `Connection` e di un campo `List` per il mantenimento dei PreparedStatement creati ed utilizzati all’interno della transazione. `execute()` esegue la transazione attraverso i seguenti passaggi:

*   Recupero di una connessione.
*   Salvataggio dei valori correnti di autocommit e isolamento.
*   Impostazione dell’autocommit su `false`.
*   Impostazione di un livello di isolamento adeguato.
*   Invocazione del metodo `buildStatement()` contenente il corpo della transazione.
*   Esecuzione del commit.
*   Ripristino dell’isolamento e dell’autocommit al valore originario.

Una transazione può non concludersi con successo, i blocchi `catch` e `finally` gestiscono quindi rollback e rilascio delle risorse. `getPreparedStatement()` è utilizzato all’interno delle sottoclassi per ottenere un statement sotto la gestione automatica della classe `AbstractDAO`, mentre `buildStatements()` viene implementato dalla specifica sottoclasse.

L’architettura permette di non riscrivere la logica comune di gestione di una transazione, favorendo il riuso e la stesura di un quantitativo minore di linee di codice.

Esaminiamo il codice per l’operazione CRUD di inserimento di una persona della classe `PersonaDAO`:

	public void inserisciPersona(Persona persona) throws DAOException {
		Transaction transaction = new Transaction() {
			@Override
			public void buildStatements(Connection conn) throws DAOException {
				try {
					PreparedStatement pstmt = getPreparedStatement(getQuery("inserisci_persona"));
					pstmt.setInt(1, persona.getId());
					pstmt.setString(2, persona.getNome());
					pstmt.setString(3, persona.getCognome());
					pstmt.setString(4, persona.getProfessione());
					pstmt.setInt(5, persona.getEta());
					pstmt.executeUpdate();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new DAOException(e);
				}
			}
		};
		transaction.execute();
	}
	

Una classe `DAO` deve quindi istanziare un oggetto `Transaction`, implementare `buildStatements()` ed eseguire `execute()`. All’interno di `buildStatements()` inseriamo tutta la logica di utilizzo delle operazioni CRUD eseguite nel contesto di una sola transazione.

In allegato trovate il codice completo con le query inserite nel file `properties`.