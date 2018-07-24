# 087 Override Del Tipo Di Ritorno

Se in una versione di Java 1.4 provavate a modificare il tipo di ritorno di un metodo, in sovrascrittura dalla classe padre, ovviamente vi veniva notificato un errore di tipizzazione. Questo accade tuttora, a meno che il tipo di ritorno non sia un erede del tipo di ritorno atteso.

Creiamo una semplice gerarchia di oggetti, quelli tipici degli esercizi Java per spiegare l’ereditarietà, la classe Point e Point3D; la prima rappresenta le coordinate spaziali bidimensionali, la seconda ne estende il concetto aggiungendo la coordinata di profondità:

Listato 11.1. Esempio con la gerarchia di oggetti

package it.html.covariantreturn;

public class Point {  
int x;  
int y;

public Point\(int x,int y\){  
this.x=x;  
this.y=y;  
}  
}

class Point3D extends Point{  
int z;

public Point3D\(int x,int y,int z\){  
super\(x,y\);  
this.z=z;  
}  
}

Ora immaginiamo che una classe, Figure, utilizzi la classe Point, per rappresentare un generico punto, ed abbia un metodo `onePoint()` che lo restituisca. Immaginiamo una classe Figure3D, erede di Figure che abbia lo stesso metodo ma che restituisca un Point3D. In Java 1.5 questo è lecito, in versioni precedenti no, eravamo forzati al tipo definito nella classe genitore.

Listato 11.2. Effettua l’override del metodo restituendo un tipo derivato da Point

package it.html.covariantreturn;  
public class Figure {  
Point p = new Point\(0,0\);  
//…  
public Point onePoint\(\){  
return p;  
}  
}

class Figure3D extends Figure{  
Point3D p = new Point3D\(0,0,0\);  
//…  
@Override  
public Point3D onePoint\(\){  
return p;  
}  
}

La cosa si ripercuote positivamente nel client che non dovrà più effettuare operazioni di casting per ovviare a quanto succedeva in precedenza. C’è da dire che l’override del metodo di ritorno può esistere a patto che la classe utilizzata sia un erede della classe attesa, altrimenti il compilatore non ci penserà due volte a darvi una segnalazione di errore.

Per completezza vediamo una classe di prova:

Listato 11.3. Classe di prova dell’override

package it.html.covariantreturn;

public class Main {  
public static void main\(String\[\] args\) {  
Figure f = new Figure\(\);  
Figure3D f2 = new Figure3D\(\);

```text
Point p = f.onePoint();  
System.out.println(“\[“+p.x+”, “+p.y+”\]”);  

Point3D p2 = f2.onePoint();  
System.out.println(“\[“+p2.x+”, “+p2.y+”, “+p2.z+”\]”);  

//In Java 1.4 avremmo dovuto fare:  
Point p3 = (Point3D) f2.onePoint();  

//con una possibile eccezione di casting  
```

}  
}

In fondo, come ultimo statement, abbiamo riportato il codice che si sarebbe dovuto utilizzare in ambiente pre Java 1.5. Anche in questo caso vediamo bene come le attenzioni delle ultime versioni di Java siano rivolte a evitare le noiose eccezioni di Runtime dovute a un eccessivo uso di casting e conversioni di tipi \(vedi autoboxing e generics\).

