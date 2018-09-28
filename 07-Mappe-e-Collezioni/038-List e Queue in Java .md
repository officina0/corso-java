Estensione di ShoppingCart
--------------------------

Vediamo un esempio pratico di **utilizzo del tipo List**. Supponiamo di voler realizzare un classico carrello della spesa partendo dalla sua definizione astratta attraverso, ad esempio, la seguente classe:

```
public abstract class ShoppingCart<T> {
	protected List<T> products;
	public abstract void add(T product);
	public abstract void delete(T product);
	public abstract int getNumberOfProducts();
	public abstract List<T> getProducts();
}
```

L’utilizzo del riferimento all’interfaccia List (**polimorfismo**) consente alle sottoclassi che estendono `ShoppingCart` di scegliere l’opportuna classe implementativa presente nelle API Java. Realizziamo quindi una classe che rappresenti un semplice prodotto da inserire nel carrello:

```
public class Product {
	protected String description;
	protected float price;
	public Product(String description, float price){
		this.description = description;
		this.price=price;
	}
    //.... get e set
}
```

Proseguiamo con l’implementazione del carrello:

```
public class ShoppingCartImpl extends ShoppingCart <Product> {
	public ShoppingCartImpl() {
		products = new ArrayList<Product>();
	}
	@Override
	public void add(Product product){
		products.add(product);
	}
	@Override
	public void delete(Product product){
		products.remove(product);
	}
	@Override
	public int getNumberOfProducts(){
		return products.size();
	}
	@Override
	public List<Product> getProducts() {
		return products;
	}
}
```

Ed infine, all’interno di una classe `main`, testiamo il tutto:

```
ShoppingCart<Product> shoppingCart= new ShoppingCartImpl(); 
		Product productA = new Product("A",100);
		Product productB = new Product("B",200);
		shoppingCart.add(productA);
		shoppingCart.add(productB);
		List<Product> products = shoppingCart.getProducts();
		Iterator<Product> iterator = products.iterator();
		while(iterator.hasNext()) {
			Product product = iterator.next();
			System.out.println(product .getDescription());
			System.out.println(product .getPrice());
		}
```

Iterazione con Enhanced For
---------------------------

Per completezza mostriamo un modo alternativo di iterare gli elementi di una collezione che faccia uso dell’**Enhanced For**:

```
shoppingCart.delete(productB);
	for(Product product : products) {
			System.out.println(product.getDescription());
			System.out.println(product.getPrice());
	}
```

Le code, realizzabili con le API Java attraverso l’interfaccia `java.util.Queue`, tipicamente ma non necessariamente, ordinano gli elementi secondo le specifiche FIFO (_First in first out_). L’interfaccia Queue estende l’interfaccia `Collection` offrendo i seguenti metodi:

```
boolean add(E e)
 E element()
 boolean offer(E e)
 E peek()
 E poll()
 E remove()
```

I metodi `element()` e `peek()` ritornano l’elemento in testa alla coda senza rimuoverlo, `element()` lancia una `NoSuchElementException` in caso di coda vuota mentre `peek()` restituisce null. Il metodo `offer()` inserisce un elemento se possibile (non violando la capacità specificata per la coda) restituendo true  
altrimenti restituisce false.

Il metodo `add()` compie la stessa operazione di `offer()` ma lancia una `IllegalStateException` in caso di fallimento. I metodi `remove()` e `poll()` rimuovono e restituiscono l’elemento in testa alla coda. L’elemento rimosso dalla coda dipende dalla policy di ordinamento specificata per la coda, che può essere differente da implementazione a implementazione.

Il comportamento di `remove()` e `poll()` è differente soltanto nel caso di coda vuota: il metodo `remove()` lancia un’eccezione, mentre il metodo `poll()` restituisce null. Esempi di implementazioni di Queue sono: `ArrayBlockingQueue<E>`, `ArrayDeque<E>` e `PriorityQueue<E>`.

La classe ArrayBlockingQueue implementa una blocking queue con capacità specificata attraverso un costruttore della classe ordinando gli elementi secondo le specifiche FIFO. Una blocking queue è Thread Safe, non accetta elementi null ed è generalmente pensata per scenari di tipo produttore-consumatore. `ArrayDeque<E>` implementa l’interfaccia `java.util.Deque` che estende Queue.

`ArrayDeque` non ha restrizioni sulle capacità e può crescere secondo le necessità.Non è Thread Safe e non consente elementi null. Questa classe può essere più veloce nelle operazioni della classe Stack quando usata come stack, e più veloce della classe `LinkedList` quando usata come queue.`PriorityQueue<E>` implementa una priority queue basata su un binary heap.

Gli elementi sono ordinati secondo il loro ordinamento naturale o in accordo ad un comparatore fornito in input ai costruttori della classe. Non è limitata nella capacità e non è Thread Safe, nel caso di necessità di accessi concorrenti si può utilizzare la versione sincronizzata `PriorityBlockingQueue<E>`.