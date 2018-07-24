Nel precedente capitolo abbiamo visto come poter utilizzare il nuovo Collection Framework con i Generics associati. Vedremo in questo capitolo come creare una **nostra classe che accetti Generics Type**. Prima di fare ciò, vediamo però come si presenta l’interfaccia Collection a partire dalla versione jdk 1.5:

Listato 8.1. Esempio di Interfaccia Collection

package java.util;  
  
public interface Collection<E> extends Iterable<E> {  
  //..  
  Iterator<E> iterator();  
  <T> T\[\] toArray(T\[\] a);  
  boolean add(E e);  
  boolean containsAll(Collection<?> c);  
  boolean addAll(Collection<? extends E> c);  
  boolean removeAll(Collection<?> c);  
  boolean retainAll(Collection<?> c);  
}

Ho riportato solo i metodi che sono stati modificati dopo l’introduzione dei Generics. Per prima cosa vediamo come l’interfaccia faccia riferimento al parametro `<E>` quando viene definita; lo stesso dicasi dell’interfaccia `Iterable<E>` che estende (vedremo il suo uso nel ciclo for-each).

In questo modo si sta informando il compilatore che la classe utilizzerà un parametro di un tipo generico che, d’ora in poi (nei metodi e nel body della classe o interfaccia), verrà riferito come E. Ovviamente si sarebbe potuto utilizzare qualsiasi altro marker per definire il parametro, qui hanno scelto E (ed anche T più avanti nel codice) per definire tipi di cui necessitiamo astrazione (che verranno scelti dallo sviluppatore che utilizzerà questa classe).

Altra cosa che ci rimane da capire è `<? extends E>`. Il significato è intuitivo comprenderlo: accetteremo parametri che estendono dalla classe E.

L’esempio ci chiarisce:

Listato 8.2. Esempio di utilizzo delle Collection

Collection<Number> list = null;  
Collection<Double> ld = null;  
//…  
list.addAll(ld);

Accettiamo come parametro solo una collezione, il cui tipo estende quello definito per la collezione stessa in fase di scrittura del codice (`E = Number`, quindi accettiamo solo tipi che estendono Number).

A questo punto siamo pronti per definire la nostra struttura dati Generics. Non ci inventiamo nulla per non complicare troppo la comprensione dell’argomento. Creiamo una pila che accetti solo tipi numerici (classe Number) e derivati (tutte le classi wrapper).

Listato 8.3. Esempio di una struttura Generics

package it.html.generics;  
  
import java.util.Iterator;  
import java.util.Stack;  
  
public class Pila<N extends Number> implements Iterable<N>{  
    
  private Stack<N> list;  
    
  public Pila(){  
    list=new Stack<N>();  
  }  
    
  public void add(N element){  
    list.push(element);  
  }  
    
  public N remove(){  
    return list.pop();  
  }  
  
  boolean isEmpty(){  
    return list.isEmpty();  
  }  
  
  @Override  
  public Iterator<N> iterator() {  
    return list.iterator();  
  }  
}

Definiamo la classe preoccupandoci di implementare l’interfaccia Iterable. Per non complicarci la vita con l’implementazione utilizziamo la classe Stack già fornita da java (definendo il parametro da utilizzare `N`, che comunque non conosciamo). Siccome vogliamo che `N` sia un numero definiamo la sua estensione dalla classe Number.

Codifichiamo quindi i metodi `add` e `remove` utilizzando come parametro N, che dinamicamente sarà scelto da chi utilizzerà questa classe. Infine implementiamo `iterator()` aggiungendo l’annotazione `@Override` per essere sicuri di sovrascrivere il metodo corretto.

Listato 8.4. Utilizza la pila vista in precedenza

public static void main(String\[\] args) {  
  Pila<Integer> p2=new Pila<Integer>();    
  for(int i=0;i<50;i++){  
    p2.add(i);  
  }  
  //..  
  while(!p2.isEmpty()){  
    System.out.println(p2.remove());  
  }  
  //..  
}

Lo spezzone di codice sopra effettua un semplice uso della pila, utilizzando i metodi `add` e `remove`. All’inizio definiamo il tipo, Integer (che estende Number) e dopo dovremo utilizzare solo tipi Integer. Nel primo for, per autoboxing, gli int vengono trasformati in Integer, nel while li stampiamo a video.

Come abbiamo visto, la definizione di Generics è piuttosto semplice e risulta davvero importante, soprattutto una volta che diventerà naturale utilizzarle. Quello che dobbiamo sottolineare è come a livello di bytecode non cambi assolutamente nulla. Quello che cambia è dare un indicazione al compilatore per effettuare dei controlli sui tipi e quindi evitare tanti possibili eccezioni di casting durante l’esecuzione, errori che altrimenti avremmo incontrato solo durante l’esecuzione del programma.