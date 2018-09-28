Analizziamo ora il codice che implementa l’algoritmo utilizzando il framework Fork-Join di Java. Iniziamo dalla realizzazione di una semplice classe per incapsulare il risultato dell’algoritmo:

```
public class TaskResult {
	private int min;
	private int max;
	public int getMin() {
		return min;
	}
	public void setMin(int min) {
		this.min = min;
	}
	public int getMax() {
		return max;
	}
	public void setMax(int max) {
		this.max = max;
	}
}
```

Il singolo task è realizzabile sfruttando una delle classi messe a disposizione del framework, nel nostro caso scegliamo la classe `RecursiveTask<T>` che richiede l’implementazione del metodo `compute()` che svolge il lavoro associato al task. Definiamo quindi la classe dell’algoritmo `MinMaxAlgorithm` come classe che estende `RecursiveTask`:

```
public class MinMaxAlgorithm extends RecursiveTask<TaskResult> {
    private static int THRESHOLD = 2;
    private int size;
    private int[] vector;
    private TaskResult taskResult;
    private MinMaxAlgorithm(int[] vector){
        this.vector = vector;
        this.size = vector.length;
        if(size<2)
			 throw new IllegalArgumentException("Il vettore deve avere almeno lunghezza 2");
        String value = String.valueOf((Math.log(size) / Math.log(2)));
        String[] parts = value.split("\\.");
        if( !parts[1].equals("0") ){
            throw new RuntimeException("Questa versione gestisce solo vettori la cui"
					+ " dimensione è una potenza di due");
        }
    }
    public TaskResult getTaskResult() {
       return taskResult;
    }
    ..
}
```

Al suo interno manteniamo come variabili di istanza il vettore di input iniziale (e sottovettore durante le fasi di fork), la sua dimensione e il risultato del task. Proseguiamo aggiungendo il metodo che implementa il fork di un vettore quando la sua dimensione è superiore alla soglia (`THRESHOLD=2`) restituendo i due sotto-task associati:

```
private List<MinMaxAlgorithm> createSubTask(){
       int newSize = size/2;
       int[] vector1 = new int[newSize];
       int[] vector2 = new int[newSize];
       for(int i=0; i<size; i++) {
          if( i<newSize ){
             vector1[i] = vector[i];
          } else {
             vector2[i-newSize] = vector[i];
          }
       }
       List<MinMaxAlgorithm> subTaskList = new ArrayList<MinMaxAlgorithm>();
       subTaskList.add(new MinMaxAlgorithm(vector1));
       subTaskList.add(new MinMaxAlgorithm(vector2));
       return subTaskList;
    }
```

Abbiamo quindi tutto l’occorrente per implementare il metodo `compute()` del singolo task:

```
@Override
protected TaskResult compute() {
   taskResult = new TaskResult();
   if( size > THRESHOLD ) {
      List<MinMaxAlgorithm> subTasks = createSubTask();
      for(MinMaxAlgorithm subTask : subTasks) subTask.fork();
          TaskResult result1 = subTasks.get(0).join();
          TaskResult result2 = subTasks.get(1).join();
          if(result1.getMin() < result2.getMin()){
             taskResult.setMin(result1.getMin());
          } else {
             taskResult.setMin(result2.getMin());
          }
          if(result1.getMax() > result2.getMax()){
             taskResult.setMax(result1.getMax());
          } else {
             taskResult.setMax(result2.getMax());
          }
      } else {
         if( vector[0] > vector[1] ) {
             taskResult.setMax(vector[0]);
             taskResult.setMin(vector[1]);
         } else {
             taskResult.setMax(vector[1]);
             taskResult.setMin(vector[0]);
         }
       }
       return taskResult;
}
```

Possiamo notare all’interno del blocco `if` le fasi di fork, attesa e join dei risultati, mentre nel blocco `else` il codice relativo al raggiungimento di un vettore di lunghezza 2. L’invocazione del metodo `fork()` provoca l’inserimento del task nella coda associata al Thread corrente, l’invocazione del metodo `join()` sui singoli task impone l’attesa del loro completamento prima di proseguire.

Completiamo la classe con il metodo statico che costruisce un pool e avvia l’esecuzione dell’algoritmo:

```
public static TaskResult minMax(int[] vector){
		ForkJoinPool pool = new ForkJoinPool();
		try {
		 return pool.invoke(new MinMaxAlgorithm(vector));
		} finally {
			pool.shutdown();
		}
	}
```

Concludiamo con l’implementazione della classe `MinMaxDemo` per testarne il funzionamento:

```
public class MinMaxDemo {
	public static void main(String[] args) throws InterruptedException, ExecutionException {
		int[] vector = new int[]{3,2,1,5,23,45,50,102};
		TaskResult taskResult = MinMaxAlgorithm.minMax(vector);
		for(int value : vector){
			System.out.print(value+" ");
		}
		System.out.println();
		System.out.println("Min:"+taskResult.getMin());
		System.out.println("Min:"+taskResult.getMax());
	}
}
```