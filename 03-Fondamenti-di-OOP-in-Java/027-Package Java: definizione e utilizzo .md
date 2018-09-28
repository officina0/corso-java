All’inizio di questa sezione abbiamo elencato quattro elementi caratterizzanti della programmazione Object Oriented, rispettivamente:

*   Astrazione;
*   Encapsulation;
*   Gerarchia;
*   Modularità.

I primi 3 della lista sono realizzabili utilizzando classi, inheritance e polimorfismo; mentre l’ultimo, la _Modularità_, riguardando il partizionamento “ad alto livello” di un problema, non sembra essere affrontabile con i soli strumenti introdotti finora. Serve qualcosa che permetta di raggruppare più classi sotto un medesimo dominio e di partizionare così un problema secondo un qualche criterio. Questo strumento sono proprio i **packages**.

Un **package** può essere definito esattamente come uno strumento per raggruppare tipi in qualche modo legati fra di loro (dove con “tipi” si intendono classi, interfacce, enumerazioni ed annotazioni (che incontreremo più avanti in questa guida).

In breve, i packages sono utilizzati in Java per classificare, organizzare, evitare conflitti, controllare l’accesso/posizione a classi in modo più semplice.

Ad esempio, i tipi che fanno parte della piattaforma Java sono elementi di diversi packages che riuniscono le classi in base alla loro funzione: il package _java.lang_ contiene le classi fondamentali del linguaggio (e.g. `String` e `Number`), _java.io_ quelle per input ed output e così via.

Oltre a sfruttare quelli forniti dalla JVM, possiamo naturalmente creare i nostri package e raggruppare classi; la creazione di package nella strutturazione di grandi progetti è in generale una buona norma in quanto aiuta nella definizione delle operazioni, nel raggruppamento delle sezioni sviluppate da diversi team e per evitare conflitti fra i nomi delle classi create.

Creare un package
-----------------

Un package si crea in Java semplicemente definiendo delle classi che appartengono ad esso, cioè utilizzando l’espressione:

```
package a.b.c.d;
```

ed inserendo le stesse in opportune directory (in cui il separatore ‘`.`‘ va sostituito con ‘`/`‘).

**La keyword package** deve essere usata in testa al file, come prima istruzione, ed all’interno di ogni file può esserci un’unica dichiarazione del package (che si applica ad ogni classe definita all’interno del file stesso).

La dichiarazione del package può anche essere omessa, e questo significa che la classe sarà inserita in un **package anonimo**; tuttavia, anche se questo è lecito, lasciare le classi nel package anonimo è da considerarsi una cattiva pratica che andrebbe evitata o che comunque andrebbe applicata solamente per piccoli progetti o, ancora meglio, solo temporaneamente durante lo sviluppo.

Un package va pensato come la **path in un albero di directory** dentro al quale vengono mantenuti i files (.java) in cui sono definite le classi ed analogamente in cui verranno creati i files .class che conterranno i compilati.

L’utilizzo di un’IDE come _eclipse_ per sviluppare rende la gestione dei packages quasi trasparente allo sviluppatore ma ogni cambiamento nel package di appartenenza di una classe corrisponde anche ad un cambiamento nella path in cui il java file della classe deve essere mantenuto.

Per **la scelta dei nomi dei package** devono essere seguite le medesime regole che si applicano ai nomi delle variabili ma, come al solito in Java, ci sono delle convenzioni che andrebbero seguite: solitamente i nomi dei package sono scritti tutti con lettere minuscole, iniziano con un carattere e non contengono alcun carattere ‘speciale’ (e.g. ‘-‘, ‘_’ etc.).

Solitamente, aziende, associazioni o gruppi che hanno un dominio web, utilizzano come package name proprio il loro dominio ribaltato; per esempio se il dominio è prova.com il package name potrebbe essere _com.prova.mypackage_.

Package e visibilità
--------------------

I package non devono essere però considerati come solo un mero strumento per la classificazione dei files java e class in directory ma hanno un loro significato anche circa come (e se) nel codice si può fare riferimento alle classi in essi contenute.

Vale innanzitutto la regola che se per classi, metodi e field non viene esplicitamente dichiarata la visibilità (con le opportune keyworkd `public` , `protected`, etc.) il compilatore assegna loro la visibilità `default`, ciò significa che saranno accessibili solo dalle altre classi appartenenti al medesimo package: insomma per default Java considera che le classi appartenenti al medesimo package hanno un grado di ‘parentela’ più stretto rispetto a quelle che appartengono a package diversi.

Anche l’accesso agli elementi pubblici che fanno parte di un package diverso da quello in cui una classe si trova deve avvenire in modo diverso che non semplicemente facendo riferimento al nome della classe e deve avvenire in uno nei seguenti modi:

*   Facendo riferimento all’oggetto attraverso il suo **fully qualified name**: quindi con il nome della classe al quale viene preposto il nome completo del package: `com.prova.mypackage.MyClass` invece che semplcemente `MyClass`
*   **Importando una classe membro del package**: inserire all’inizio del file java l’istruzione
    
    `import com.prova.mypackage.MyClass;`
    
    che istruisce il compilatore ad ‘importare’, quindi rendere disponibile la classe MyClass che quindi potrà essere utilizzata come se stesse nel package corrente omettendo il package name (ma non cambiando le regole delal visibilità).
    
*   **Importando l’intero package** con l’istruzione import e la wildcard asterisco (‘*’):
    
    `import com.prova.mypackage.*`
    
    che rende visibili tutte le classi nel package.
    

Utilizzare i packages, un esempio
---------------------------------

Supponiamo di avere una serie di classi che descrivono i diversi ruoli in un’azienda (come riportate di seguito) che implementano tutte la solita interfaccia Persona.

```
//nel file Persona.java
public interface Persona {
	...
}
//nel file Dipendente.java
public abstract class Dipendente {
    ...
}
//nel file Amministrazione.java
public class Amministrazione extends Dipendente implements Persona {
    . . .
}
//nel file Segreteria.java
public class Segreteria extends Dipendente implements Persona {
    . . .
}
//nel file Tecnici.java
public class Tecnici extends Dipendente implements Persona {
    . . .
}
```

Tutte queste classi ed interfacce possono essere raggruppate in un unico package, per moltissime ragioni, fra le quali:

*   il **team di sviluppo** che lavora al progetto può determinare in modo semplice che tutte queste classi sono legate fra di loro;
*   si riesce a determinare in maniera più semplice la funzione e **la responsabilità di ciascuna classe** presente nel package;
*   **i nomi delle classi non andranno in conflitto** con quelli di altri package scritti (eventualmente) da altri sviluppatori/team;
*   le classi che appartengono al medesimo package hanno **minori restrizioni all’accesso** delle altre classi (rispetto a quelle che si trovano al di fuori del package stesso).

quindi in testa ad ognuna di queste classi potremmo inserire la dichiarazione del package:

```
package com.prova.team;
```

che specifica che tutte le classi appartengono al solito package `com.prova.team`.