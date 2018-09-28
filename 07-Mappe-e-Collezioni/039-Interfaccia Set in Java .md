L’interfaccia `java.util.Set<E>` definisce il tipo di dato **Insieme**. Un Insieme non ammette duplicati e non definisce un ordinamento  
per i suoi elementi. L’interfaccia che permette la realizzazione di insiemi dotati anche di un ordinamento sugli elementi è `java.util.SortedSet`  
che estende `java.util.Set`. Le implementazioni di Set sono tali che non consentono l’aggiunta di un elemento se questo è già presente nell’insieme.

Il confronto tra oggetti avviene attraverso i metodi `equals()` ed `hashCode()` che ogni classe eredita dalla superclasse `Object`.  
Un’implementazione dell’interfaccia Set è la classe `HashSet`, vediamo un suo esempio di utilizzo per collezionare oggetti di una classe  
`Point`:

```
public class Point {
	private int x;
	private int y;
	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
	public int getX() {
		return x;
	}
	public void setX(int x) {
		this.x = x;
	}
	public int getY() {
		return y;
	}
	public void setY(int y) {
		this.y = y;
	}	
	public boolean equals(Object point){
		if( point instanceof Point){
			Point p = (Point)point;
			return (x==p.x && y==p.y)?true:false;
		} else return false;
	}
	public int hashCode(){
		return 1;
	}
}
```

L’aspetto più importante è la ridefinizione dei metodi `equals()` ed `hashCode()` per implementare la logica di uguaglianza  
tra oggetti `Point` rispettando il contratto previsto per la loro implementazione. In particolare il contratto previsto da `equals()`  
prevede che se per due oggetti A e B, `A.equals(B)` restituisca true. Allora il valore restituito da `A.hashCode()` deve essere uguale  
al valore restituito da `B.hashCode()`.

In generale invece due oggetti possono restituire lo stesso valore attraverso  
il metodo `hashCode()` ma non essere uguali secondo `equals()`. Inoltre il contratto di `equals()` prevede che valgano le seguenti proprietà:

Proprietà

Descrizione

**Riflessiva**

Un oggetto è uguale a sé stesso: `A.equals(A)` restituisce true.

**Simmetrica**

Se `A.equals(B)` restituisce true, allora anche `B.equals(A)` deve restituire true.

**Transitiva**

Se `A.equals(B)` e `B.equals(C)` restituiscono true, allora anche A.equals(C) deve restituire true.

Con queste regole siamo in grado di gestire correttamente oggetti  
che vengono aggiunti a classi che implementano Set. La classe `Point` realizza correttamente le regole possiamo quindi  
realizzare un frammento di codice dimostrativo usando la classe `HashSet`:

```
HashSet<Point> pointsSet = new HashSet<Point>();
Point p1 = new Point(1,2);
pointsSet.add(p1);
pointsSet.add(p1);
System.out.println("Prova, aggiungo elemento in Set duplicato:");
for(Point point : pointsSet){
	System.out.println(point.getX()+"-"+point.getY());
}
Point p2 = new Point(1,3);
pointsSet.add(p2);
System.out.println("Prova, aggiungo elemento in Set distinto:");
for(Point point : pointsSet){
	System.out.println(point.getX()+"-"+point.getY());
}
```

Il codice stamperà il seguente risultato:

```
Prova, aggiungo elemento in Set duplicato:
1-2
Prova, aggiungo elemento in Set distinto:
1-2
1-3
```

che mostra come un elemento duplicato non sia stato correttamente aggiunto alla HashSet.

Consideriamo ora la classe `TreeSet`  
che, implementando `SortedSet`, consente di definire un ordinamento sugli elementi aggiunti. Quando iteriamo  
un TreeSet gli elementi verranno sempre restituiti secondo il criterio che  
abbiamo definito.

Il mantenimento di un ordine ha un costo che non risulta comunque essere eccessivo. Infatti la quantità di tempo  
richiesta da un TreeSet per compiere le operazioni base è proporzionale al logaritmo del numero dei suoi elementi. Un TreeSet  
richiede che tutti gli elementi ad esso aggiunti implementino l’interfaccia `Comparable` per il criterio di ordinamento  
che verrà applicato. In alternativa è possibile istanziare una TreeSet utilizzando  
i costruttori che richiedono un oggetto di una classe che implementa l’interfaccia `Comparator`.

L’interfaccia `java.lang.Comparable` prevede l’implementazione del solo metodo:

```
public int compareTo(Object x);
```

che ritorna un numero positivo se l’oggetto corrente è maggiore di quello passato come parametro, il valore  
0 se oggetto corrente ed oggetto passato sono uguali, e un numero negativo se l’oggetto corrente è più  
piccolo di quello passato. Riprendiamo la classe Point e facciamo in modo che implementi l’interfaccia `Comparable`:

```
public class Point implements Comparable<Point> {
....
 @Override
 public int compareTo(Point p) {
	 if(x==p.x && y==p.y) return 0;
	 else if(x>p.x || y>p.y) return 1;
	 else return -1;
 }
}
```

Il criterio di ordinamento definito per due punti, prevede che essi siano uguali se hanno coordinate coincidenti,  
il punto corrente sia più grande di quello passato come parametro se situato a destra o in alto rispetto al secondo,  
il punto corrente più piccolo di quello corrispondente al parametro in tutti gli altri casi.  
I punti saranno ordinati in modo crescente:

```
TreeSet<Point> pointsSet = new TreeSet<Point>();
	Point p1 = new Point(1,2);
	Point p2 = new Point(1,3);
	Point p3 = new Point(1,4);
	pointsSet.add(p1);
	pointsSet.add(p2);
	pointsSet.add(p3);
	System.out.println("Elementi TreeSet:");
	for(Point point : pointsSet){
		System.out.println(point.getX()+"-"+point.getY());
	}
```

Il codice stamperà il risultato:

```
Elementi TreeSet:
1-2
1-3
1-4
```

Un ordinamento coerente con il criterio che abbiamo definito, infatti i punti (1,3) ed (1,4) sono più in alto del punto (1,2) ed il punto (1,4) è più in alto del punto (1,3).