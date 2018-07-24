# 082 Introduzione A Java Annotations

Possiamo definire un’annotation come un appunto che mettiamo per specificare qualcosa relativo al codice che stiamo scrivendo, un attributo particolare, un metodo o una classe che hanno delle peculiarità. Attraverso questo meccanismo siamo capaci di dare espressività al codice, di renderlo più leggibile agli occhi di altri sviluppatori ma soprattutto agli occhi del compilatore.

Le annotations, infatti, sono delle annotazioni per il compilatore \(o per chi si occupa del deploy dell’applicazione\) che, attraverso di esse avrà la possibilità di effettuare determinate operazioni.

Questo paradigma, oltre a rendere più espressivo il codice sorgente, permette una migliore manutenzione dello stesso. Utilizzando le opportune annotations, infatti, riusciremo ad evitare possibili errori di compilazione, oppure, meglio ancora, potremo delegare a strumenti esterni la configurazione dell’applicazione che stiamo scrivendo \(nell’ultima parte dell’articolo vedremo le annotations in ambiente enterprise\).

La stessa Sun definisce gli usi delle annotazioni come di seguito:

* Per informare il compilatore
* Per processare a tempo di compilazione \(o di deploy\)
* Per processare a run time

Le ultime due caratteristiche sono tipiche degli ambienti enterprise, che le possono utilizzare mediante [reflection](http://java.html.it/articoli/leggi/2414/reflection-in-java/).

## Annotazioni previste da JDK 1.5

Un’annotazione si presenta nella seguente forma:

Listato 4.1. Esempio di un’annotazione

@Autore\(  
name = “Pasquale Congiustì”,  
company = “HTML.it”  
\)  
class ClasseAnnotata\(\) {  
…  
}

Ogni annotazione si presenta con il simbolo `@` seguito dal nome dell’annotazione. Eventualmente può essere valorizzata con dei valori, tra parentesi tonde come coppia nome-valore. Essa precede la classe, il metodo o l’attributo che vogliamo annotare.

In questo esempio abbiamo annotato la classe con l’annotation Autore ed i due attributi name e company.

Nella prossima lezione vediamo le Annotations di default, fornite a partire dalla distribuzione J2SE 1.5. Si tratta di tre semplici annotazioni utili al compilatore.

Attenzione: utilizzare le annotazioni significherà che il codice non può essere compilato \(e spesso anche eseguito\) con versioni Java precedenti.

