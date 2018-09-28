Per costruire una qualsiasi  
query, sia essa di interrogazione o di aggiornamento del database, abbiamo due scelte possibili:  
**Statement** o **PreparedStatement**. Entrambi sono ottenuti da un oggetto `Connection` ma si differenziano  
per la tipologia di query che permettono di gestire. Mentre `Statement` va utilizzato per query che non richiedono  
parametri, un `PreparedStatement` è invece l’ideale per la gestione di query che richiedono  
uno o più parametri (**query precompilate**).

La stringa di una query precompilata ha la caratteristica di presentare  
dei punti interrogativi in corrispondenza dei quali si attendono dei valori. I punti interrogativi sono  
indicizzati a partire dal valore 1, da sinistra verso destra  
nella stringa della query. L’indice consente quindi di referenziare la posizione di un punto interrogativo  
in una query precompilata. Ad esempio nella query:

```
select nome, cognome, professione, eta from persona where eta>=? and nome=?
```

Il primo punto interrogativo ha indice 1 mentre il secondo indice 2.

La classe `PreparedStatement` mette a disposizione  
diversi metodi del tipo `setXXX()` per impostare un valore specificando il suo indice posizionale. Nel caso della query precedente,  
possiamo sostituire un valore intero al primo punto interrogativo ed una stringa al secondo:

```
..
      PreparedStatement pstmt = conn.prepareStatement("select nome, cognome, professione, eta from persona where eta>=? and nome=?");
      pstmt.setInt(1, 30);
      pstmt.setString(2, "Mario");
	..
```

Con il metodo `executeQuery()` delle interfacce `Statement` o `PreparedStatement`, mandiamo in esecuzione la query verso il database.  
Otteniamo successivamente una risposta sotto forma di oggetto `ResultSet`. Quest’ultimo viene utilizzato generalmente in un ciclo  
`while`:

```
ResultSet rs = ..;
	 while( rs.next()) { .. }
```

Il metodo `next()` posiziona il cursore sul successivo record da processare restituendo il valore booleano `true` se tale record esiste o  
`false` in caso contrario.

Essendo un’operazione comune, nella nostra architettura il ciclo di elaborazione di un `ResultSet`  
è stato inserito nel metodo `rsReader()` della classe `AbstractDAO`. Al fine di recuperare il valore in corrispondenza  
di una particolare colonna del record corrente, all’interno del ciclo `while` possiamo invocare, su un `ResultSet`,  
diversi metodi del tipo `getXXX()` dove “XXX” indica un particolare tipo Java.

Consideriamo l’esecuzione della query:

```
select nome, cognome, professione, eta from persona
```

Ciascun record restituito nel `ResultSet` avrà 4 campi: i primi 3 di tipo `String` ed il quarto di tipo `int`. All’interno  
del blocco `while` avremo quindi un codice del tipo:

```
ResultSet rs = ..;
	 while( rs.next()) {
	   String nome = rs.getString(1);
	   String cognome = rs.getString(2);
	   String professione = rs.getString(3);
	   int eta = rs.getInt(4);
	   ..
	 }
```

Nel codice di esempio si è fatto uso, nei metodi `getXXX()`, dell’indice posizionale delle colonne in luogo del nome della colonna; in generale, questa modalità è consigliata perchè più performante, realizziamo quindi una classe di test che utilizza  
la nostra classe `DAO` e che mostra i benefici del pattern.

Definiamo quindi una classe `DAOTest` che ottiene un riferimento alla classe `PersonaDAO` invocando successivamente i metodi esposti:

```
public static void main(String[] args) {
       PersonaDAO personaDAO =new PersonaDAO();
       for(Persona persona : personaDAO.getPersone()){
        	System.out.println(persona.getNome());
        	System.out.println(persona.getCognome());
        	System.out.println(persona.getProfessione());
        	System.out.println(persona.getEta());
        	System.out.println();
       }
       for(Persona persona : personaDAO.getPersoneByEtaMag(32)){
            System.out.println(persona.getNome());
            System.out.println(persona.getCognome());
            System.out.println(persona.getProfessione());
            System.out.println(persona.getEta());
            System.out.println();
       }
     }
```