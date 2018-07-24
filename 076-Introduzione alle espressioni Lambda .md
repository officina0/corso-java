Le **espressioni Lambda** rappresentano una caratteristica introdotta dalla versione 8 di Java che consente la scrittura di codice più compatto migliorandone quindi la leggibilità. Nei capitoli precedenti, in particolare durante la trattazione dei Thread e degli Executor, abbiamo visto come ottenere un oggetto di una classe anonima che implementa l’interfaccia _Runnable_ attraverso istruzioni simili a quella mostrata dal seguente frammento di codice:

   ExecutorService executor = Executors.newFixedThreadPool(2);
   executor.submit(new Runnable(){
	  public void run(){
		     String threadName = Thread.currentThread().getName();
		     System.out.println("Executor Thread A: " + threadName);
	  }
	});

Con una Lambda expression possiamo raggiungere lo stesso risultato attraverso un blocco di codice più compatto:

   ExecutorService executor = Executors.newFixedThreadPool(2);
   executor.submit(() -> {
             String threadName = Thread.currentThread().getName();
		     System.out.println("Executor Thread A: " + threadName);
	        }
   );

La sintassi Lambda ha la forma generale

( Lista Parametri metodo interfaccia funzionale) -> { Blocco istruzioni metodo interfaccia funzionale }

Nel codice d’esempio la lista parametri è vuota poichè il metodo `run` non accetta parametri, il blocco contiene invece le istruzioni che desideriamo vengano eseguite per avere un funzionamento esattamente identico al precedente ma con meno codice. Le parentesi graffe possono essere omesse nel caso in cui il blocco di codice sia costituito da un’unica istruzione.

Vediamo un altro esempio. Supponiamo di voler ordinare una lista di interi costruita utilizzando, a scopo dimostrativo, le classi `Arrays` e `Collections`. Prima di Java 8 avremmo scritto qualcosa del genere:

	public static void main(String\[\] args) {
	 List<Integer> numbers =	Arrays.asList(3,2,1,4,5);
	 Collections.sort(numbers, new Comparator<Integer>(){
		@Override
		public int compare(Integer o1, Integer o2) {
			return o1.compareTo(o2);
		}	 
	 });
	 
	 for(Integer number : numbers) {
		 System.out.println(number);
	 }
	}

`Comparator` è un’interfaccia funzionale che prevede un solo metodo da implementare, `compare()`, con una signature che richiede due oggetti da confrontare. Nel nostro caso il tipo generico è impostato sulla classe wrapper `Integer` in quanto desideriamo confrontate numeri interi. Con una Lambda expression possiamo sostituire l’_anonymous inner class_ di `Comparator` dell’esempio precedente, con un’espressione più breve:

public static void main(String\[\] args) {
	 List<Integer> numbers = Arrays.asList(3,2,1,4,5);
	 Collections.sort(numbers, (Integer o1, Integer o2) -> {
		 return o1.compareTo(o2);
	 });
	 
	 for(Integer number : numbers) {
		 System.out.println(number);
	 }
	}

Il blocco è costituito da una sola istruzione, possiamo quindi scrivere il codice in una forma ancora più compatta:

 public static void main(String\[\] args) {
  List<Integer> numbers = Arrays.asList(3,2,1,4,5);
  Collections.sort(numbers, (Integer o1, Integer o2) -> o1.compareTo(o2)); 
  for(Integer number : numbers) {
	  System.out.println(number);
  }
 }
 

L’accesso a variabili al di fuori dello scope di una Lambda Expression è molto simile a quello di una _anonymous inner class_. Possiamo accedere a variabili locali dichiarate _final_, a variabili di istanza e statiche della classe.