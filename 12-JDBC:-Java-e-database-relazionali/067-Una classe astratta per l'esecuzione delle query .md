Continuiamo la realizzazione dell’architettura introdotta nel  
capitolo precedente con la realizzazione di una classe astratta  
(`AbstractDAO`) come implementazione base del pattern **DAO**.

Desideriamo  
che `AbstractDAO` incapsuli una logica di servizio comune a tutte le  
operazioni di interazione con i database: costruzione di oggetti  
connessione e di oggetti per l’esecuzione di query e operazioni CRUD.

La classe sarà di tipo generico e specializzata dalle  
sottoclassi in base alle tabelle e alle funzionalità. `AbstractDAO`  
utilizzerà inoltre la classe `DBManager` per fornire i servizi base  
richiesti dalle sottoclassi.

Si delinea così un’architettura a  
strati alla cui base abbiamo `DBManager` sulla quale  
poggia `AbstractDAO` che, a sua volta, è il mattone base per  
tutte le classi DAO utilizzate dagli strati superiori del  
sistema.

Il codice di `AbstractDAO` è il seguente:

```
public abstract class AbstractDAO<T> {
	private DBManager dbManager;
	public AbstractDAO() {
		try {
			dbManager = DBManager.getInstance();
		} catch (DBManagerException e) {
			System.err.println("AbstractDAO:" + e.getMessage());
		}
	}
	protected Connection getConnection() throws SQLException {
		if (dbManager != null) {
			return dbManager.getConnection();
		}
		return null;
	}
	protected PreparedStatement prepareStatement(Connection conn, String queryId) throws SQLException {
		return conn.prepareStatement(dbManager.getQuery(queryId));
	}
	protected  List<T> rsReader(ResultSet rs) throws SQLException {
	   ArrayList<T> beans = new ArrayList<>();
	   while(rs.next())  beans.add(makeBean(rs));
	   return beans;
	}
	protected String getQuery(String queryId) {
		return dbManager.getQuery(queryId);
	}
	protected abstract T makeBean(ResultSet rs) throws SQLException;
}
```

`T` indica classe bean legata ad una specifica tabella. Notiamo come essa utilizzi `DBManager` in modo trasparente alle sottoclassi  
per fornire servizi di connessione, costruzione di oggetti  
`PreparedStatement` per query da compilare, lettura di oggetti `ResultSet`  
risultato di query per la costruzione di liste Java e recupero delle  
stringhe di query.

Alle sottoclassi è richiesta l’implementazione del  
metodo `makeBean()` per la costruzione dell’oggetto Java  
rappresentando un record del risultato di  
interrogazione.

Supponiamo di avere una tabella  
`persona` i cui campi siano in corrispondenza per nome e tipo con la  
seguente classe `Persona`:

```
public class Persona {
		private int    id;
		private String nome;
		private String cognome;
		private String professione;
		private int    eta;
		public Persona(String nome, String cognome, String professione, int eta) {
			this.nome = nome;
			this.cognome = cognome;
			this.professione = professione;
			this.eta = eta;
		}
		..
	}
```

Editiamo il file `query.properties` in modo che contenga le query:

```
lista_persone=select nome, cognome, professione, eta from persona
lista_persone_eta_maggiore=select nome, cognome, professione, eta from persona where eta>=?
```

Specializziamo la classe `AbstractDAO<T>` come `AbstractDAO<Persona>` dando vita alla seguente classe DAO:

```
public class PersonaDAO extends AbstractDAO<Persona> {
	@Override
	protected Persona makeBean(ResultSet rs) throws SQLException{
		return new Persona(rs.getString("nome"), rs.getString("cognome"),
				rs.getString("professione"), rs.getInt("eta"));
	}
	public List<Persona> getPersone(){
		try( Connection conn = getConnection();
			 Statement  stmt = conn.createStatement()){
			 return rsReader(stmt.executeQuery(getQuery("lista_persone")));
		} catch(SQLException e){
			e.printStackTrace();
		}
		return new ArrayList<Persona>();
	}
	public List<Persona> getPersoneByEtaMag(int eta){
		try( Connection conn = getConnection();
			 PreparedStatement  pstmt =
					 conn.prepareStatement(getQuery("lista_persone_eta_maggiore"))){
			pstmt.setInt(1, eta);
			return rsReader(pstmt.executeQuery());
		} catch(SQLException e){
			e.printStackTrace();
		}
		return new ArrayList<Persona>();
	}
}
```

`makeBean()` realizza l’oggetto `Persona` leggendo i dati dal `ResultSet`. `getPersone()` ottiene una connessione tramite `getConnection()` ereditato da `AbstractDAO` e recupera la stringa  
della query da eseguire con il metodo `getQuery()` sempre ereditato da `AbstractDAO`.

La classe `Statement` consente l’esecuzione  
della query restituendo un oggetto `ResultSet` che per la navigazione dei record contenuti nel risultato.  
Un ResultSet mette infatti a disposizione il metodo `next()` per muovere il cursore in avanti nella lista  
dei record.