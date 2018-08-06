L’interfaccia `java.util.Map<K,V>` permette di definire la struttura dati **Mappa**. `Map` non estende `Collection` e non può essere considerata un tipo collezione sebbene mantenga insiemi di coppie di elementi contraddistinti da una chiave e dal valore ad essa associato. La chiave è ciò che identifica univocamente un elemento in una mappa: non è possibile avere due elementi in una mappa con lo stesso valore di chiave. I metodi più importanti esposti dall’interfaccia Map sono:

V put(K key, V value);
V get(Object key);
boolean containsKey(Object key);
void clear();
Set<K> keySet();
Collection<V> values();

Il metodo `put()` consente l’inserimento di un valore mentre `get()` ne permette il recupero, specificando una chiave. `containsKey()` e `clear()` permettono rispettivamente di verificare la presenza di un elemento con una determinata chiave e di cancellare tutti gli elementi della mappa. Con `keySet()` possiamo ottenere l’insieme delle sole chiavi, mentre con `values()` i soli valori presenti nella mappa.

Come per `Set` sulle chiavi di una `Map` non è definito alcun ordinamento, se desideriamo averlo, e solo a livello di chiavi, possiamo utilizzare classi che implementano l’interfaccia `java.util.SortedMap` impiegando oggetti `Comparable` o `Comparator` per definirne il criterio.

Esempio pratico: gestire una classifica con una mappa
-----------------------------------------------------

Immaginiamo di voler gestire una classifica di squadre di calcio attraverso una mappa in cui ciascuna entry sia rappresentata da una chiave che identifica il posizionamento della squadra e da un valore (il nome della squadra):

  HashMap<Integer,String> classifica = new HashMap<Integer,String>();
  classifica.put(1, "Juventus");
  classifica.put(2, "Napoli");
  classifica.put(3, "Roma");
  classifica.put(4, "Inter");
		
  for(Integer key : classifica.keySet()) {
		System.out.println(classifica.get(key));
  }

Con una Map non abbiamo garanzie del recupero degli elementi secondo un preciso ordine, quindi non possiamo essere certi di un risultato come:

Juventus
Napoli
Roma
Inter

La classe `Integer` implementa l’interfaccia `Comparable` fornendo un criterio di ordinamento crescente. Per avere certezza del corretto ordine di posizionamento delle squadre possiamo utilizzare una `TreeMap`:

  TreeMap<Integer,String> classifica = new TreeMap<Integer,String>();
  classifica.put(1, "Juventus");
  classifica.put(2, "Napoli");
  classifica.put(3, "Roma");
  classifica.put(4, "Inter");
		
  for(Integer key : classifica.keySet()) {
		System.out.println(classifica.get(key));
  }

Il test di uguaglianza sulle chiavi di una Map avviene attraverso l’utilizzo del metodo `equals()` cosi come visto per il tipo `Set`.

java.util.Collections e java.util.Arrays
----------------------------------------

Concludiamo la trattazione delle API per collezioni e mappe Java con una rapida analisi delle classi `java.util.Collections` e `java.util.Arrays`.

La classe `Collections` è dotata di una serie di metodi di utilità che semplificano diverse operazioni sulle collezioni:

static boolean disjoint(Collection c1, Collection c2)
static int frequency(Collection c, Object obj)
static Object max(Collection c)
static Object min(Collection c)
static void reverse(List list)
static void shuffle(List list)
static void sort(List list)

`disjoint()` restituisce true se le due collezioni in input non hanno elementi in comune. `frequency()` restituisce il numero di elementi nella collezione in input uguali all’oggetto `obj`. I metodi `max()` e `min(`) restituiscono l’elemento massimo e minimo della collezione in accordo con l’ordinamento naturale definito sui suoi elementi.

`reverse()` inverte l’ordinamento degli elementi della lista in input mentre `shuffle()` definisce un nuovo ordinamento del tutto casuale. `sort()` consente il riordinamento naturale degli elementi della lista.

La classe `Arrays` supporta l’ordinamento e la ricerca di elementi all’interno di array Java fornendo anche un metodo che consente la conversione di un array in un tipo `List`:

static List asList(Object\[\] array)

Alcuni metodi di utilità generale della classe `Arrays` sono i seguenti:

static void sort(byte\[\] array)
static int binarySearch(byte\[\] array, byte key)
static boolean equals(Object\[\] array1, Object\[\] array2)

Il metodo `sort()`, di cui esistono in `Arrays` diverse altre versioni, riordina gli elementi del vettore, `binarySearch()` consente di recuperare l’indice del vettore dove è posizionato l’elemento key, ed infine `equals()` consente di verificare se due array sono uguali, ovvero hanno la stessa lunghezza ed uguali gli elementi corrispondenti.