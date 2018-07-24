# 033 Java 8 Thread E Executor

A partire da Java 8 le API per la concorrenza hanno avuto interessanti modifiche con l’introduzione degli **Executor** come strato di più alto livello nella gestione dei **Thread**. Gli Executor sostituiscono la modalità di realizzazione diretta di Thread vista nel capitolo precedente, consentendo l’esecuzione di task asincroni e pool di Thread.

Ogni Thread all’interno del pool è riutilizzabile, un Executor infatti non termina autonomamente la sua esecuzione ma rimane in attesa dell’esecuzione di nuovi task. Per terminare un Executor dobbiamo esplicitamente invocare su di esso uno dei metodi di shutdown offerti dalla classe. Iniziamo con un esempio di un semplice Executor che realizza un pool di Thread di dimensione 1:

import java.util.concurrent.ExecutorService; import java.util.concurrent.Executors; import java.util.concurrent.TimeUnit;

public class DemoSingleExecutor { public static void main\(String\[\] args\){

ExecutorService executor = Executors.newSingleThreadExecutor\(\); executor.submit\(new Runnable\(\){ public void run\(\){ String nomeThread = Thread.currentThread\(\).getName\(\); System.out.println\("Executor con un solo Thread: " + nomeThread\); } }\);

try { System.out.println\("Tentativo shutdown executor"\); executor.shutdown\(\); executor.awaitTermination\(10, TimeUnit.SECONDS\); } catch \(InterruptedException e\) { System.err.println\(e.getMessage\(\)\); } finally { if \(!executor.isTerminated\(\)\) { System.err.println\("Shutdown normale non riuscito nel tempo stabilito"\); System.err.println\("Invocazione shutdownNow\(\)"\); } executor.shutdownNow\(\); System.out.println\("Shutdown completato"\); } } }

La classe Executor fornisce diversi metodi di factory per il recupero di un suo oggetto. Nell’esempio l’Executor utilizza un solo Thread implementato attraverso classe anonima con Runnable, ed aggiunto all’Executor con il metodo `submit()`. Sono disponibili diversi altri metodi come:

Metodo

Descrizione

**newCachedThreadPool\(\)**

Per un pool di Thread che può crescere dinamicamente riutilizzando i Thread creati precedentemente.

**newFixedThreadPool\(\)**

Per un pool di Thread a dimensione fissa riutilizzabili dall’Executor.

**newScheduledThreadPool\(\)**

Per un pool di Thread che eseguono task dopo un certo intervallo di tempo o periodicamente.

Volendo realizzare un pool di 2 Thread possiamo modificare il codice iniziale nel seguente modo:

.... ExecutorService executor = Executors.newFixedThreadPool\(2\); executor.submit\(new Runnable\(\){ public void run\(\){ String threadName = Thread.currentThread\(\).getName\(\); System.out.println\("Executor Thread A: " + threadName\); } }\); executor.submit\(new Runnable\(\){ public void run\(\){ String threadName = Thread.currentThread\(\).getName\(\); System.out.println\("Executor Thread B: " + threadName\); } }\); ....

Un esempio che mostra l’esecuzione di un task dopo un intervallo di 3 secondi è ottenibile con il seguente codice come parte iniziale:

... import java.util.concurrent.ScheduledExecutorService; import java.util.concurrent.ScheduledFuture; ... ScheduledExecutorService executor = Executors.newScheduledThreadPool\(1\); ScheduledFuture&lt;?&gt; future = executor.schedule\(new Runnable\(\){ public void run\(\){ System.out.println\("Scheduling "+Thread.currentThread\(\).getName\(\)\); } }, 3, TimeUnit.SECONDS\);

long tempoRimanente = 0; while\(\(tempoRimanente=future.getDelay\(TimeUnit.MILLISECONDS\)\)&gt;0\){ System.out.printf\("Tempo rimanente all'esecuzione: %sms ", tempoRimanente\); System.out.println\(\); } ....

Dove con `schedule()` scheduliamo il task dopo un intervallo di 3 secondi e, successivamente, con un ciclo `while` utilizziamo l’oggetto `Future` per stampare sulla console il tempo rimanente all’esecuzione. Un Executor, oltre a riferimenti Runnable, è in grado di utilizzare anche riferimenti di tipo **Callable**. La differenza rispetto a Runnable è che l’interfaccia Callable ha un metodo che ritorna una valore:

... ExecutorService executor = Executors.newSingleThreadExecutor\(\); Future future = executor.submit\(new Callable \(\){

```text
public Integer call() throws Exception {
       String threadName = Thread.currentThread().getName();
       System.out.println("Executor con un solo Thread utilizzando Callable: " + threadName);
       TimeUnit.SECONDS.sleep(1);
       return 1;
}

});
```

while\(!future.isDone\(\)\){ System.out.println\("Polling sul Thread: " + future.isDone\(\)\); }

try { System.out.println\("Il Thread ha completato il suo lavoro: "+future.get\(\)\); } catch \(InterruptedException e1\) { e1.printStackTrace\(\); } catch \(ExecutionException e1\) { e1.printStackTrace\(\); } ...

L’aspetto interessante dell’utilizzo di _Callable_ è la possibilità di monitorare lo stato di esecuzione di un Thread in un modo semplice ed elegante sfruttando il metodo `isDone()` dell’oggetto `Future` restituito dal `submit()` dell’Executor.

