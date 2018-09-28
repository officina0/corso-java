Una delle principali mancanze, delle vecchie versioni di Java, era quella relativa a un paradigma per l’elaborazione di una sorgente XML. Nel momento in cui intendevate effettuare una lettura o qualsiasi operazione su un file XML dovevate utilizzare uno strumento proprietario (API di terze parti) con il rischio di legarvi a vita ad una specifica implementazione.

Una delle modifiche della versione 1.4 di Java è stata quella di introdurre, tra le sue API di base, la **JAXP** (Java API for XML Processing). Il principale obiettivo di questa modifica è quello di astrarre l’implementazione concreta, lasciando quindi allo sviluppatore un’interfaccia generica da utilizzare, e poi un motore di parsing da poter cambiare in maniera dinamica.

Prima di vedere l’implementazione concreta, citiamo i due **modelli di parsing** che vengono proposti in JAXP: **SAX** (Simple API for XML) e **DOM** (Document Object Model).

Non entriamo nel dettaglio delle due tipologie, richiederebbe un capitolo a parte, ma per comprendere quello che segue è bene sapere che il primo è un modello di accesso semplice, basato sugli eventi incontrati nella lettura di un file XML (apertura e chiusura dei tag, attributi, ecc), il secondo è basato su una scansione del documento e di una rappresentazione dell’albero degli oggetti.

Il primo non mantiene in memoria una rappresentazione del documento, quindi può essere utilizzato per grossi file XML o per accessi veloci. Il secondo, invece, mantiene una struttura dei nodi, quindi può essere utilizzato per documenti di dimensione ridotta o per modificare la struttura dell’albero, aggiungendo e rimuovendo dinamicamente i nodi.

Visto che sono due possibili modelli di utilizzo, JAXP lascia la possibilità di scegliere. La libreria infatti è così composta:

1.  javax.xml.parsers
2.  org.w3c.dom
3.  org.xml.sax
4.  javax.xml.transform

Il primo è un package generico che definisce le classi factory per poter accedere alle implementazioni concrete di SAX e DOM, e quindi poter processare i file XML.

Il secondo definisce le classi e l’interfaccia programmatica da utilizzare nel caso di un processing DOM delle risorse XML. Qui è contenuta la definizione di Node, Attribute e in generale del modello che rappresenta l’albero dei nodi e come poterlo accedere e modificare.

Il terzo è più semplice in quanto espone il modello programmatico SAX per processare le risorse XML, in questo caso guidato dagli eventi (quindi dalla classe Handler e relativi metodi `startTag()`, `endTag()`, ecc).

L’ultimo package introduce la possibilità di trasformare il codice XML attraverso il meccanismo della [trasformazione XSL](http://java.html.it/articoli/leggi/2710/trasformare-xml-in-jsp/ "Articolo sulle trasformazioni XSL"), dando accesso a una serie di interfacce di utilità per raggiungere lo scopo.

Come si vede dal codide, non c’è la necessità di importare librerie di terze parti ed è immediato manipolare il documento modificandone la struttura.

Listato 16.1. Esempio di accesso DOM a un documento

package it.html.xml;

public class MainXML {  
  public static void main(String[] args) throws Exception {  
    //Recupero concretamente l’engine di parsing DOM (di default)  
    DocumentBuilderFactory dbfactory = DocumentBuilderFactory.newInstance();  
    DocumentBuilder domparser = dbfactory.newDocumentBuilder();  
      
    //Effettuiamo il parsing di un file  
    Document doc = domparser.parse(new File(“data.xml”));  
    //Recuperiamo il primo nodo  
    Node x = doc.getFirstChild();  
    //Recupero il primo nodo TEAM del documento  
    Node y = doc.getElementsByTagName(“TEAM”).item(0);  
    //Aggiungo il nodo y a x  
    x.appendChild(y);  
      
    //Creiamo un nuovo documento  
    Document doc2 = domparser.newDocument();  
    //e appendiamo la rappresentazione del primo doc  
    doc2.appendChild(x);  
      
    //infine salviamo il file  
    XMLSerializer serializer = new XMLSerializer();  
    serializer.setOutputCharStream(new java.io.FileWriter(“data2.xml”));  
    serializer.serialize(doc2);  
  }  
}

L’uniformità garantita dall’interfaccia JAXP ci permette di vedere un documento DOM sempre allo stesso modo. Ciò significa che questo codice potrà essere riutilizzato anche quando cambia l’implementazione del motore di parsing sottostante. Infatti, le prime due righe di codice sono quelle che concretamente recuperano l’implementazione che poi ci garantisce un corretto utilizzo della risorsa.

Abbiamo mostrato come funziona il DOM, ma altrettanta semplicità avrete utilizzando l’interfaccia SAX o la trasformazione XSL.