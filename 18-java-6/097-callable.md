# 097 Callable

I due limiti principali della programmazione concorrente risiedono nella signature del metodo `run()` di un Thread. Esso non può essere modificato per poter essere gestito dallo scheduler della JVM e quindi non avremo mai la possibilità di avere un thread che ci restituisce un risultato con un’interrogazione diretta o sollevare _checked exceptions_. Ovviamente, prima delle novità che andremo a vedere in questo capitolo, c’era il modo per ottenere il risultato di un esecuzione di un thread interrogando una variabile di risultato, ad esempio, attendendo il termine del thread con un metodo `join()`.

Queste limitazioni però facevano sì che il programmatore dovesse arrovellarsi non poco, con il risultato di un codice farraginoso e complicato da comprendere o manutenere. Il package _java.util.concurrent_ presenta allora una serie di nuove interfacce per permettere a un thread di comportarsi come una classe sincrona, dove la gestione della concorrenza \(quindi l’attesa se il risultato non è disponibile\) è effettuata all’interno delle classi che andremo a spiegare.

La prima interfaccia che vediamo è l’interfaccia `Callable<T>` che ha un semplice metodo, `<T> call()`, la cui esecuzione effettua un’attività asincrona e restituisce il risultato definito come parametro dell’interfaccia:

Listato 22.1. Esempio di utilizzo di Callable

package it.html.threads;

import java.util.Collection;  
import java.util.NoSuchElementException;  
import java.util.Vector;  
import java.util.concurrent.Callable;  
import java.util.concurrent.TimeUnit;

public class CallableTask implements Callable&gt; {

String keyword;

public CallableTask\(String key\){  
this.keyword = key;  
}

@Override  
public Collection call\(\) throws EmptyListException{  
Collection toRet = new Vector\(\);  
//… simuliamo la presenza di oggetti in un db  
toRet.add\(“Product \#CADASD432”\);  
toRet.add\(“Product \#CADAS322”\);  
//… se non ci sono oggetti solleviamo un’eccezione  
if \(toRet.isEmpty\(\)\)  
throw new EmptyListException\(“Nessun elemento trovato!”\);  
//… simuliamo il tempo di ricerca  
try {  
TimeUnit.SECONDS.sleep\(5\);  
} catch \(InterruptedException e\) {  
// NOOP  
}

```text
return toRet;  
```

}

In questo esempio immaginiamo di avere un task che restituisca il risultato di una ricerca su un catalogo prodotti, in base ad una chiave di ricerca. Per ottimizzare il processo di ricerca facciamo in modo che la ricerca sia asincrona, quindi creiamo un Callable concreto di tipo `Collection<String>`.

In questo caso restituiamo un risultato concreto oppure solleviamo l’eccezione **EmptyListException**, per segnalare che la ricerca non ha dato esito \(è una checked exception\). Vediamo il resto dell’esempio \(un main di test e l’eccezione\):

…  
public static void main\(String args\[\]\) throws InterruptedException{  
CallableTask task = new CallableTask\(“games”\);

```text
System.out.println(“Search result (dovrebbero passare 5 secondi prima di vedere il risultato…):”);  
try {  
  showResult(task.call());  
} catch (EmptyListException e) {  
  System.out.println(“Nessun oggetto trovato”);  
}  

System.out.println(“… fine della computazione”);  
```

}

class EmptyListException extends Exception{

public EmptyListException\(String string\) {  
super\(string\);  
}

}

Creiamo un task passando la keyword di ricerca e stampiamo la lista di risultati attesi oppure una segnalazione di errore se la lista è vuota.

Come vediamo dall’esecuzione dell’esempio, l’uso dell’oggetto Callable è di sicuro interesse in quanto ci dà la possibilità di avere un’esecuzione asincrona con un tipo di ritorno e una lista di eccezioni che desideriamo. In realtà, come potete vedere, di asincrono c’è ben poco, in quanto l’esecuzione del metodo `call()` arresta l’esecuzione del flusso del main, in attesa del risultato.

