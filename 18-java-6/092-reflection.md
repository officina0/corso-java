# 092 Reflection

L’introduzione dei [tipi Generics](http://java.html.it/guide/lezione/3972/java-generics/) ha portato all’inevitabile cambiamento di diverse classi oltre al framework Collection. In questo capitolo ci occupiamo di descrivere le modifiche apportate al package _java.lang.reflection_. Come sappiamo, la reflection è uno strumento molto potente utilizzato per analizzare e modificare a runtime il comportamento del linguaggio tramite introspezione. L’introduzione dei tipi generici porta quindi al cambiamento di molte interfacce e classi del package attraverso il loro utilizzo.

Anche l’introduzione delle annotazioni porta all’evoluzione del package con l’introduzione di metodi per accedere a runtime alla [definizione delle annotazioni](http://java.html.it/guide/lezione/3966/introduzione-a-java-annotations/). Ad esempio, la classe _java.lang.reflection.Class_ introduce i due metodi:

 A getAnnotation\(Class annotationClass\)  
Annotation\[\] getAnnotations\(\)

Attraverso questi metodi riusciamo a recuperare a runtime il tipo Annotation che dinamicamente possiamo interrogare per gestire il comportamento dinamico definito dall’annotazione.

Non vediamo tutte le modifiche del package perché sono tante ed applicate a tutte le classi, ci limitiamo a fare un riassunto dei principali tipi di modifica in modo da avere una panoramica generale \(per consultare le modifiche basta accedere a Javadocs API\).

Supporto ai tipi generics: tutte le principali classi vengono riviste aggiungendo i tipi generics, laddove possibile, per poter sfruttare questo nuovo paradigma.

Ad esempio, adesso dovremo utilizzare la seguente dichiarazione:

Class x,y,z;  
//..  
Constructor constr = getDeclaredConstructor\(x,y,z\)

Supporto per le annotazioni: anche qui abbiamo anticipato l’introduzione dei metodi per poter accedere dinamicamente alle annotazioni.

Supporto per i tipi enum: anche l’introduzione del tipo enum, apporta modifiche al package per far si di effettuare introspezione dinamica.

Supporto per var args: la possibilità di effettuare di gestire un numero di argomenti variabili necessita una struttura opportuna per la sua gestione.

Metodi di utilità: sono presenti una serie di nuovi metodi di utilità per poter utilizzare al meglio la reflection.

La classe _java.lang.reflect.Class_ è stata resa generica, quindi ora dovremo utilizzarla come segue:

Class c;

