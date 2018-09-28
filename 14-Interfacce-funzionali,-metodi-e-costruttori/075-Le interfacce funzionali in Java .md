Programmazione funzionale
-------------------------

La **programmazione funzionale** si basa sul concetto di funzione matematica. Le funzioni dei linguaggi  
di programmazione imperativi non utilizzano tale termine in questo senso, ma rappresentano delle procedure  
che eseguono una logica e restituiscono valori.

Una funzione non esegue calcoli ma rappresenta una legge che associa  
ad un elemento di un insieme A uno e un solo elemento di un insieme B. In matematica, una funzione reale di variabile  
reale, viene spesso indica con `y=f(x)`. La variabile `x`, il cui valore appartiene all’insieme di valori dell’insieme A,  
viene chiamata variabile indipendente, mentre `y`, il cui valore appartiene all’insieme B, è invece chiamata  
variabile dipendente, A e B vengono detti, rispettivamente, Dominio e Codominio della funzione.

A e B possono  
coincidere con l’insieme R dei numeri reali, ed un esempio di funzione può essere il seguente: `y=2x`. In esso  
la funzione associa ad ogni numero reale un altro numero reale che rappresenta il doppio del primo.

Interfacce funzionali
---------------------

Java 8 ha introdotto delle  
caratteristiche che permettono con una certa misura una programmazione in stile funzionale. **Un’interfaccia funzionale è  
un’interfaccia con un solo metodo**. Queste interfacce, nel contesto di un linguaggio orientato agli oggetti,  
rappresentano ciò che più si avvicina a una funzione.

Abbiamo visto esempi di interfacce funzionali nei capitoli precedenti, ad esempio durante  
la trattazione dei Thread con l’interfaccia `Runnable`. Seppur non obbligatoria, l’annotation `@FunctionalInterface` può  
essere aggiunta a livello di nome di interfaccia per ribadire che l’interfaccia è funzionale.  
La specifica di questa annotazione ha l’utilità di segnalare un errore di compilazione se si tenta di aggiungere ulteriori metodi.

Mostriamo un semplice esempio di interfaccia funzionale che modella un concetto astratto di azione:

```
@FunctionalInterface
public interface IAction {
	public void doWork();
}
```

Nel package `java.util.function` sono state introdotte molte interfacce funzionali. Un esempio  
è l’interfaccia `Predicate<T>` con il solo metodo `boolean test(T)`. `Predicate` rappresenta un predicato, una funzione  
ad un solo argomento che restituisce un valore booleano.

Realizziamo un esempio di utilizzo dell’interfaccia funzionale `Predicate` per implementare una semplice logica dei predicati:

```
per ogni x se Bird(x) -> ToFly(x)
```

Con la formula precedente, se `x` è un uccello allora (semplificando) `x` è in grado di volare. `Bird()` è il predicato che restituisce `true` se il suo argomento è un tipo uccello, `ToFly()` è invece un predicato che restituisce `true` se il suo argomento è sempre un tipo di volatile.

Realizziamo quindi le classi `Bird` e `ToFly` per i predicati introdotti e le classi `Animal` e `Bird` che rappresentano il concetto generale di animale e quello più specifico di uccello:

```
public class Animal {}
public class Bird extends Animal {}
public class IsBird implements Predicate<Animal>{
	@Override
	public boolean test(Animal animal) {
		return animal instanceof Bird;
	}
}
public class ToFly implements Predicate<Animal>{
	@Override
	public boolean test(Animal animal) {
		return new IsBird().test(animal);
	}
}
```

Concludiamo l’esempio con un programma dimostrativo:

```
public class FInterfacesDemo {
	public static void main(String[] args) {
		Animal cat = new Animal();
		Animal eagle = new Bird();
		IsBird isBirdPredicate = new IsBird();
		ToFly  toFlyPredicate = new ToFly();
		System.out.println("Is a bird:"+
		isBirdPredicate.test(eagle) + " To fly:"+ toFlyPredicate.test(eagle));
		System.out.println("Is a bird:"+
				isBirdPredicate.test(cat) + " To fly:"+ toFlyPredicate.test(cat));
	}
}
```

Le interfacce funzionali in Java consentono di specificare metodi statici e metodi non statici (marcati con la keyword `default`) entrambi con la caratteristica di essere dotati di implementazione all’interno dell’interfaccia. Questa caratteristica è nota come **metodi di estensione**. L’interfaccia funzionale `Predicate<T>` presenta diversi metodi di questo tipo:

```
..
default Predicate<T> and(Predicate<? super T> other);
static <T> Predicate<T>	isEqual(Object targetRef);
default Predicate<T> negate();
default Predicate<T> or(Predicate<? super T> other);
..
```