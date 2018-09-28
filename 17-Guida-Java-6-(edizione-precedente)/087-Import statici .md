Uno dei principali obiettivi nella sviluppo delle ultime due versioni di Java è stato quello di portare delle migliorie e facilitazioni nella scrittura del codice e nella sua manutenzione ordinaria. L’introduzione degli import statici aggiunge delle piccole semplificazioni.

Iniziamo con il più classico degli esempi: l’utilizzo degli stream di output e di errore (`System.out` e `System.err`). Noi utilizziamo gli oggetti out e err sulla classe System in quanto oggetti dichiarati statici (e pubblici). Facciamo un ragionamento più astratto e parliamo di tutte le istanze statiche che utilizziamo. Durante la stesura di un programma, capita di invocare decine e decince di volte questi oggetti, facendo riferimento al package (java.lang è importato di default), alla classe e ai metodi o alle variabili da utilizzare.

L’**import statico** ci viene in aiuto, consentendoci di importare gli elementi statici ci permette quindi di citarne il riferimento, omettendo il namespace (appena importato). Nel nostro caso potremo importare staticamente gli elementi `out` e `err` della classe _java.lang.System_ ed utilizzarli riferendoli direttamente:

Listato 10.1. Esempio di Import statico

package it.html.tiger;  
import static java.lang.System.out;  
import static java.lang.System.err;

public class ImportTest {  
  public static void main(String[] args) {  
    //Chiamiamo direttamente la variabile out  
    out.println(“Salve!”);  
    //e la variabile err  
    err.println(“Esecuzione eccezionale”);  
  }  
}

Ovviamente non si tratta di una grande rivoluzione, ma aiuta, e di molto, a ridurre il tempo di scrittura, in particolare quando abbiamo di fronte classi e metodi statici di utilità. Quanto detto, infatti, vale sia con variabili che con metodi statici. Pensiamo alle classi di utilità di _java.lang.Math_ che ha decine di metodi statici, utilizzati per effettuare funzioni matematiche di base.

Listato 10.2. Import statico del metodo max presente nella libreria Math

import static java.lang.Math.max;  
//..  
  int y = max (10,33);  
//..

Nel caso in cui il nome del metodo presenti più casi (per overload) l’import verrà esteso a tutti i metodi. Quando invece ci sono stessi nomi di metodi (statici), associati a diversi oggetti, varrà il principio di associazione che si basa sui parametri con cui chiamiamo la funzione.

C’è da dire che spesso vedremo la funzione di import statico insieme alla definizione e all’uso di enum (altra nuova caratteristica di cui parleremo in seguito).

L’import statico ha lo stesso significato (e sintassi) dell’import normale, con la differenza che questo è associato a tipi definiti statici. La presenza del carattere jolly (*), quindi, ha lo stesso significato dell’import normale. Nell’esempio che segue vediamo infatti come importare tutta la libreria Math e come richiamarne in maniera diretta i metodi:

Listato 10.3. Esempio di importazione della libreria Math

import static java.lang.Math.*;  
//..  
  int y = max (10,33);  
  int z = abs(-3);  
  double s = sqrt(99);  
//..