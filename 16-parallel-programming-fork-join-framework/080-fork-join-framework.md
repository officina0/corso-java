# 080 Fork Join Framework

Il framework Fork/Join è comparso per la prima volta con la versione 7 di Java come strumento per sfruttare al meglio le CPU multicore utilizzando un approccio “dividi e conquista”. Il framework divide \(_fork_\) ricorsivamente il problema principale in sotto-task più piccoli fino a quando sono sufficientemente semplici da poter essere eseguiti in modo asincrono. La fase di _Join_ raccoglie ricorsivamente i risultati di ciascun sotto-task unendoli nel risultato finale. Per fornire l’esecuzione parallela, il framework utilizza un pool di Thread chiamato `ForkJoinPool` che gestisce Threads del tipo `ForkJoinWorkerThread`.

Il ForkJoinPool è un’implementazione dell’`ExecutorService` che gestisce quelli che vengono chiamati **Worker Threads**. Essi eseguono un task alla volta ed il `ForkJoinPool` non crea un Thread separato per ogni task, invece ogni Thread nel pool ha la sua coda di task da eseguire. Un Worker Thread recupera i suoi task dalla testa della coda, ma quando quest’ultima è vuota il Thread può recuperare task dalla parte finale della coda di altri Worker Thread occupati nell’elaborazione.

Questo tipo di approccio minimizza la possibilità che i Threads debbano competere per l’esecuzione di Task e riduce il numero di volte che un Thread deve cercare del lavoro da svolgere. Questo processo è noto come **Work Stealing Algorithm**.

Come esempio pratico, utilizziamo il Fork/Join framework per implementare l’algoritmo `MinMax` che riceve in input un vettore di interi e restituisce il valore minimo e massimo del vettore. La sua implementazione è ricorsiva e si articola nei seguenti step:

1. Se la dimensione del vettore è 2 allora determina il minimo ed il massimo confrontando i valori e restituisci il risultato.
2. Altrimenti dividi ricorsivamente il vettore in due sottovettori, applica il passo 1 a ciascun vettore e prosegui con il passo successivo.
3. Attendi i risultati dei due sotto-task.
4. Confronta i risultati determinando il minimo ed il massimo dai minimi e massimi dei sotto-task e restituisci il nuovo risultato.

Applichiamo l’algoritmo ad un vettore di esempio di 8 elementi indicizzato quindi da 0 a 7. Lo step 1 ha esito negativo, il vettore viene allora suddiviso in due parti da 4 e per ciascuna di esse si esegue il passo 1 attendendo i risultati \(punto 3\). Abbiamo quindi effettuato un fork del vettore di partenza e attendiamo i risultati dei due sotto-task sui vettori da 4 elementi per effettuare il join dei risultati.

Ciascun sotto-task sul vettore da 4 elementi effettua un nuovo fork \(passo 1 ha esito negativo\) ottenendo finalmente due sottovettori da 2 elementi. Abbiamo quindi 4 vettori da 2 elementi sui ciascuno dei quali il passo 1 ha finalmente esito positivo, ognuno di questi sotto-task ritorna quindi un risultato min-max. I task in attesa del loro completamento ricevono i risultati e li utilizzano \(join\) per comporre il nuovo risultato min-max, il processo di join continua a ritroso nella fase ricorsiva ottenendo il risultato finale min-max dell217;algoritmo:

Figura 1. Fork-Join algoritmo Min-Max.

![Fork-Join algoritmo Min-Max](http://www.html.it/wp-content/uploads/2017/11/minMax.png)

