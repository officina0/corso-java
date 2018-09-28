Lo Stream Java (`java.util.Stream`) rappresenta una sequenza di elementi sui cui eseguire **operazioni intermedie o terminali**. Le terminali generano un risultato di un determinato tipo mentre le intermedie un nuovo Stream su cui invocare altre operazioni dando vita ad una catena di chiamate a metodi.

Gli Stream sono creati a partire da una sorgente che può essere rappresentata da una qualsiasi classe compatibile con l’interfaccia `java.util.Collection`. Le mappe non sono supportate. Un aspetto interessante riguarda la possibilità di eseguire operazioni su uno Stream in modo sequenziale o parallelo.

Stream e operazioni intermedie
------------------------------

Costruiamo una lista che rappresenti un mazzo di carte da poker privo dei jolly, lo scopo è estrarre un sottoinsieme di carte (nuovo Stream) in base ad un filtro:

```
List<String> mazzoDiCarte = Arrays.
				asList("A-C","A-P","A-F","A-Q",
				       "K-C","K-P","K-F","K-Q",
				       "Q-C","Q-P","Q-F","Q-Q",
			               "J-C","J-P","J-F","J-Q",
				       "10-C","10-P","10-F","10-Q",
				       "9-C","9-P","9-F","9-Q",
				       "8-C","8-P","8-F","8-Q",
				       "7-C","7-P","7-F","7-Q",
				       "6-C","6-P","6-F","6-Q",
			               "5-C","5-P","5-F","5-Q",
				       "4-C","4-P","4-F","4-Q",
			               "2-C","2-P","2-F","2-Q");
		mazzoDiCarte.
		stream().
		filter((s)->s.startsWith("A")).
		forEach(System.out::println);
```

L’invocazione del metodo `stream()` consente di ottenere uno Stream sulla lista `mazzoDiCarte`. Invochiamo poi sull’oggetto Stream ottenuto il metodo `filter` che accetta in input un tipo `Predicate<T>`.

Possiamo ottenere un oggetto `Predicate` sfruttando quanto appreso sulle espressioni Lambda. `Predicate` è un’interfaccia funzionale e richiede l’implementazione del metodo `test()`, la `signature` di questo metodo richiede un parametro di un generico tipo `T` e restituisce un booleano. Nel nostro caso l’espressione Lambda:

```
(s) -> s.startsWith("A")
```

permette di costruire di un oggetto `Predicate<String>` in quanto il valore per il parametro passato è di tipo `String`.

Il compilatore deduce il tipo basandosi sullo Stream costituito da oggetti di tipo `String.` Il blocco di istruzioni del metodo `test()` è rappresentato dalla sola istruzione `startsWith()`. Lo Stream ottenuto dall’invocazione del metodo `filter()` è una nuova lista di elementi che verificano il criterio specificato: le stringhe che iniziano con il carattere “A”.

Vediamo un esempio di ordinamento degli elementi di uno Stream. Realizziamo preliminarmente un comparatore di stringhe per le carte da gioco. Il comparatore deve ordinare le carte in base ad un valore numerico, assegniamo quindi un numero anche alle carte J, Q, K e A in modo da poterle confrontare con le altre. La classe di comparazione estende `java.util.Comparator` ed effettua il confronto tra due carte utilizzando i primi due caratteri delle stringhe provenienti dallo Stream:

```
public class CartaComparator implements Comparator<String> {
	@Override
	public int compare(String s1, String s2) {
		int v1 = getValue(s1);
		int v2 = getValue(s2);
		if( v1==v2 ) return 0;
		else if( v1>v2 ) return 1;
		else return -1;
	}
	private static int getValue(String s){
		StringBuilder s1Sub = new StringBuilder(s.substring(0, 2));
		int v1;
		if(Character.isDigit(s1Sub.charAt(0))) {
			v1 = Integer.parseInt(s1Sub.toString());
		} else {
			switch (s1Sub.charAt(0)){
				case 'A': v1=14; break;
				case 'K': v1=13; break;
				case 'Q': v1=12; break;
				case 'J': v1=11; break;
				default : throw new RuntimeException("Carta non valida");
			}
		}
		return v1;
	}
}
```

Utilizzando `sorted()` sullo Stream e passando come parametro un oggetto comparatore della classe creata possiamo ottenere uno Stream che rappresenta un mazzo di carte ordinate  
in modo crescente in base al valore:

```
mazzoDiCarte.stream().
sorted(new CartaComparator()).
forEach(System.out::println);
```

Un altro esempio è la possibilità di applicare un metodo a tutti gli elementi di uno Stream ottenendo un nuovo Stream di oggetti modificati. Utilizzando `map()` possiamo fare in modo che il metodo `toUpperCase()` della classe `String` sia applicato a tutte le stringhe di uno Stream:

```
List<String> rubrica = Arrays.
asList("Rossi Mario","Bianchi Luigi","Verdi Laura");
rubrica.stream().
map(String::toUpperCase).
sorted().
forEach(System.out::println);
```

Nell’esempio `sorted()` ordina gli elementi in base al loro criterio naturale di ordinamento.

Stream e operazioni terminali
-----------------------------

Vediamo ora una dimostrazione di Stream con operazioni terminali. L’operazione terminale `anyMatch()` ottenuta da uno Stream richiede come input un oggetto `Predicate<T>` come per `filter()`; possiamo ottenere ciò attraverso una Lambda Expression:

```
boolean test = rubrica.stream().
anyMatch((s) -> s.startsWith("Rossi"));
System.out.println(test);
```

In questo caso il valore generato da `anyMatch()` sarà `true` se lo Stream contiene una stringa che inizia con la sottostringa “Rossi”, altrimenti `false`. Altri metodi utili sono:

*   `allMatch()`: restituisce `true` se tutti gli elementi verificano la condizione.
*   `noneMatch()`: restituisce `true` se nessun elemento verifica la condizione.

Riduzione di uno Steam
----------------------

Attraverso la **Riduzione** di uno Stream possiamo ottenere un oggetto di classe `java.util.Optional` che incapsula il valore del risultato delle Riduzione. Come esempio pensiamo di voler ridurre lo Stream della mini-rubrica vista in precedenza ad un unica stringa costituita dalla concatenazione delle stringhe dello Stream stesso:

```
List<String> rubrica = Arrays.
     asList("Rossi Mario","Bianchi Luigi","Verdi Laura");
     Optional<String> ridotta =
                rubrica
                .stream()
                .sorted()
                .reduce((s1, s2) -> s1 + "," + s2);
     ridotta.ifPresent(System.out::println);
```

La classe `Optional` è molto utile per la prevenzione dell’eccezione `NullPointerException`. Può essere vista come un container per un valore uguale o meno a `null`. Si pensi ai casi nei quali un metodo di una classe può generare un valore specifico oppure `null`, invece di restituire `null` può restituire un oggetto `Optional`.