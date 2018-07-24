In questo capitolo analizzeremo due implementazioni del tipo Mappa: **HashMap** e **HashTable**. Le differenze sono già state introdotte nel capitolo precedente, vogliamo quindi concentrare l’attenzione sui parametri che ne influenzano le performance.

I parametri a cui siamo interessati vengono indicati come **Capacity** e **Load Factor**. La Capacity indica il numero di Buckets all’interno della struttura che implementa una HashMap/HashTable, mentre il Load Factor indica la percentuale di riempimento consentita per la mappa prima che la sua capacità venga ridefinita automaticamente.

Quando il numero di elementi all’interno di una mappa supera il prodotto di “Capacity x Load Factor”, la mappa viene ricostruita con un numero di Bucket approssimativamente pari al doppio del valore precedente. Il valore di default per il Load Factor è 0.75 (75%) mentre per la Capacity è 16 (sempre una potenza di 2).

Per quanto detto, una mappa con Load Factor e Capacity con valori di default verrà ricostruita sull’inserimento dell’elemento numero 13 a causa del superamento del valore limite “Capacity x Load Factor = 16 * 0.75 = 12”. Supponiamo di dover gestire una HashMap in cui chiavi e valori siano stringhe e che mediamente preveda 450 elementi; la mappa potrebbe contenere, ad esempio, le coppie parole/traduzione di un dizionario richieste più frequentemente.

Con le impostazioni di default avremo diversi ridimensionamenti della mappa durante la fase di inserimento degli elementi, mentre agendo sul valore Capacity e lasciando invariato il Load Factor sul 75%, possiamo fare in modo che, mediamente, non avvenga il ridimensionamento.

Nel frammento di codice che andiamo a realizzare istanzieremo due mappe: la prima con valori di default mentre la seconda con il valore di Capacity impostato a 650. La seconda mappa verrà quindi ridimensionata quando il numero di elementi supererà il valore “Capacity x Load Factor = 650 * 0.75 = 487.5”, valore sensibilmente superiore alla media che raramente raggiungeremo o supereremo.

import java.util.HashMap;

public class MapDemo {

	public static void main(String\[\] args) {
	
		HashMap<String,String> defHashMap = 
				new HashMap<String,String>();
		HashMap<String,String> custHashMap = 
				new HashMap<String,String>(650,0.75f);
        
		long start = System.currentTimeMillis();
		
		for(int i=1; i<=450;i++)
		 defHashMap.put(i+"", i+"");
		 
		long end = System.currentTimeMillis();
		
		System.out.println(end-start);
		
		start = System.currentTimeMillis();
		
		for(int i=1; i<=450;i++)
		 custHashMap.put(i+"", i+"");
		 end = System.currentTimeMillis();
		
		System.out.println(end-start);
	}

}
 

Confrontando i valori relativi con il tempo di esecuzione della fase d'inserimento all'interno di una mappa, possiamo effettuare una valutazione dei tempi medi di esecuzione su, ad esempio, un totale di 10 esecuzioni del programma demo:

Esecuzioni con valori di Default: 11 6 9 12 7 6 5 9 6 6
Esecuzioni con valori personalizzati: 3  2 2 2  3 2 4 4 3 3

Ottenendo un tempo medio di esecuzione di 7.7 ms utilizzando una HashMap con valori di default e un, decisamente migliore, 2.8 ms personalizzando i parametri della HashMap.

Ovviamente occorre prestare attenzione anche alla quantità di memoria occupata da una HashMap scegliendo i valori di Load Factor e Capacity per raggiungere il giusto compromesso tra complessità di tempo e di spazio. Il valore 0.75 per il Load Factor, insieme all'opportuna Capacity, è generalmente il valore che consente di raggiungere questo obiettivo.

Sostituendo alla classe `HashMap` la classe `Hashtable` otteniamo la versione del programma che fa uso di una mappa con implementazione differente:

	..
	Hashtable<String,String> defHashtable = 
				new Hashtable<String,String>();
	Hashtable<String,String> custHashtable = 
				new Hashtable<String,String>(650,0.75f);
	..
				

Essendo `Hashtable` una classe sincronizzata, possiamo utilizzarla nei casi di programmazione concorrente, in tutti gli altri casi è opportuno utilizzare `HashMap`.