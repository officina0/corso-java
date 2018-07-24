# 088 String Builder

Nello sviluppo in Java si ha sempre a che fare con tipi di base matematici, logici e catene di testo. Sappiamo bene che in java la classe che si occupa di rappresentare un testo è la classe String \(package _java.lang_\) e probabilmente l’abbiamo utilizzata una miriade di volte senza porci troppi problemi alle performance derivanti dal suo uso.

La prima cosa da sottolineare è che l’oggetto String è un oggetto immutabile, cioè, una volta creato, non può essere modificato. Di fatto, nel momento in cui modifichiamo un oggetto String, in Java, ne stiamo creando un’altra istanza, rimpiazzando quella precedente. Gli stessi operatori di append \(`x+="aaa"`\) non fanno altro che fare l’override dei metodi presenti nella classe StringBuffer con la quale la classe String viene gestita per questioni di performance.

Proprio per questo motivo in ambiente pre java 1.5 il modo più ovvio per gestire catene di testo mutabili era quello di usare direttamente la classe **StringBuffer** ed i relativi metodi per la gestione del testo \(in particolare del metodo append e dei metodi replace\). La classe è una threadsafe, il che consente di poter effettuare le operazioni sull’oggetto StringBuffer condividendolo tra diversi thread.

StringBuilder è identica a StringBuffer, stessi metodi stessa logica, unica differenza: non è threadsafe. Le performance migliorano in maniera netta, in particolare considerando che molti programmi fanno un notevole uso delle stringhe e della loro modifica durante il ciclo di vita del software. L’utilizzo, quindi, d’ora in poi dovrebbe essere scontato a favore della nuova classe introdotta, nel momento in cui non abbiamo da gestire aspetti legati all’accesso concorrente alla risorsa.

In ambiente enterprise ciò risulta sempre, quindi è evidente che in questo caso utilizzeremo sempre oggetti StringBuilder per la gestione di stringhe. Abbiamo misurato le prestazioni delle tre classi con un semplice esempio che potete effettuare voi stessi:

Listato 13.1. Test sull’uso delle Stringhe

package it.html.tiger;

public class StringComparison {  
/_\*  
\_ @param args  
\*/  
public static void main\(String\[\] args\) {  
long start=System.currentTimeMillis\(\);  
testString\(\);  
long end=System.currentTimeMillis\(\);  
System.out.println\(“Tempo di esecuzione testString\(\) “+\(end-start\)+” millis.”\);

```text
start=System.currentTimeMillis();  
testStringBuffer();  
end=System.currentTimeMillis();  
System.out.println(“Tempo di esecuzione testStringBuffer() “+(end-start)+” millis.”);  

start=System.currentTimeMillis();  
testStringBuilder();  
end=System.currentTimeMillis();  
System.out.println(“Tempo di esecuzione testStringBuilder() “+(end-start)+” millis.”);  
```

}

private static void testString\(\) {  
String x = “”;  
for\(int i=0;i&lt;15000;i++\){  
//operazione di append  
x+=i;  
}  
}

private static void testStringBuffer\(\) {  
StringBuffer x = new StringBuffer\(“”\);  
for\(int i=0;i&lt;15000;i++\){  
//operazione di append  
x.append\(i\);  
}  
}

private static void testStringBuilder\(\) {  
StringBuilder x = new StringBuilder\(“”\);  
for\(int i=0;i&lt;15000;i++\){  
//operazione di append  
x.append\(i\);  
}  
}  
}

Ecco il risultato ottenuto:

* Tempo di esecuzione `testString()` 1656 millis.
* Tempo di esecuzione `testStringBuffer()` 16 millis.
* Tempo di esecuzione `testStringBuilder()` 0 millis.

