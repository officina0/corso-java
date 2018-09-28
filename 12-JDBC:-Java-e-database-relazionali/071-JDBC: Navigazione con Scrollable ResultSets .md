Gli ordinari `ResultSets` descritti nei capitoli precedenti sono in  
grado di garantire lo scorrimento solo in avanti partendo dalla prima  
posizione. Nelle applicazioni professionali si ha spesso l’esigenza di  
doversi posizionare in un particolare punto del ResultSet  
recuperando, a partire da questa posizione, un determinato numero di  
record. Questa esigenza è alla base dei sistemi di paginazione.

In questo capitolo introduciamo gli **Scrollable ResultSets** attraverso  
la realizzazione di un nuovo metodo `getPersona()`, della classe  
`PersonaDAO`, che accetti come parametri l’indice iniziale e finale del  
gruppo di record da estrarre da un ResultSet.

Iniziamo con il  
presentare il codice del metodo:

```
public List<Persona> getPersone(int startIdx, int endIdx) {
		try (Connection conn = getConnection(); Statement stmt = conn.
				createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
								ResultSet.CONCUR_READ_ONLY))  {
			stmt.setMaxRows(endIdx);
			ResultSet rs = stmt.executeQuery(getQuery("lista_persone"));
			rs.absolute(startIdx-1);
			return rsReader(rs);
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return new ArrayList<Persona>();
	}
```

La costruzione di uno Scrollable ResultSet inizia da uno Statement che permetta di specificare alcune costanti  
relative al tipo di ResultSet e alla gestione della concorrenza. Un  
ResultSet può essere di tre tipi:

Tipologia

Descrizione

**ResultSet.TYPE\_FORWARD\_ONLY**

Rappresenta il default che consente solo lo  
scorrimento in avanti.

**ResultSet.TYPE\_SCROLL\_INSENSITIVE**

Consente il movimento in  
avanti e indietro senza sensibilità agli aggiornamenti che  
possono avvenire durante la navigazione.

**ResultSet.TYPE\_SCROLL\_SENSITIVE**

Differisce dal precedente  
per la sensibilità agli aggiornamenti.

Nel nostro particolare caso abbiamo scelto di non considerare  
eventuali aggiornamenti durante la navigazione del ResultSet inserendo  
quindi la costante `ResultSet.TYPE_SCROLL_INSENSITIVE`. La concorrenza  
per un ResultSet fa riferimento alla possibilità di aggiornare  
i record durante la navigazione attraverso una serie di metodi del  
tipo `updateXXX()`.

Per la nostra implementazione scegliamo di non rendere  
possibile la modifica dei record attraverso la scelta della costante  
`ResultSet.CONCUR_READ_ONLY` (l’alternativa è invece  
rappresentata dalla costante `ResultSet.CONCUR_UPDATABLE`).

Il metodo  
`setMaxRows()` dell’interfaccia `Statement` consente di specificare il  
numero massimo di record che ogni ResultSet creato con lo Statement  
può contenere. Utilizzato congiuntamente con il metodo  
`absolute()`, che consente il posizionamento su un particolare record  
fornendone l’indice, `setMaxRows()` permette la selezione di un gruppo di record fornendo indice di partenza ed  
indice di fine (parametri `startIdx` e `endIdx`).

Non possiamo passare  
direttamente l’indice `startIdx` al metodo `absolute()`,  
dobbiamo infatti ricordare che `rsReader()` effettua una  
chiamata al metodo `next()` del ResultSet che sposterebbe il cursore di  
un’ulteriore posizione in avanti facendoci perdere un record. Per  
queste regioni passiamo ad `absolute()` il valore `startIdx`  
decrementato di 1.

All’interno della classe `DAOTest` possiamo inserire il seguente frammento di codice di test:

```
for(Persona persona : personaDAO.getPersone(2,4)){
            System.out.println(persona.getNome());
            System.out.println(persona.getCognome());
            System.out.println(persona.getProfessione());
            System.out.println(persona.getEta());
            System.out.println();
        }
```

Variando i parametri di `getPersone()` possiamo sperimentare  
l’estrazione di blocchi di record dal ResultSet. Come esempio, sapendo  
che gli indici dei record iniziano dal valore uno, l’esecuzione del  
metodo `getPersone(2,4)` seleziona i record del ResultSet dall’indice 2 all’indice 4  
restituendoli come oggetti Java all’interno di un ArrayList.

Concludiamo sottolineando  
l’importanza di consultare la documentazione dello specifico database che si sta utilizzando al fine di  
verificare il supporto alle caratteristiche JDBC presentate. Il codice allegato è stato testato  
su database Postgres 8.3.