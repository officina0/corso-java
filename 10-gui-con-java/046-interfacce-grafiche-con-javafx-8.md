# 046 Interfacce Grafiche Con Java FX 8

**JavaFX 8** è una libreria grafica inclusa in Java SE 8 che consente di sviluppare **Rich Client Application**, delle applicazioni dotate di un’interfaccia utente costruita attraverso un insieme di componenti **UI** \(_User Interface_\) predefiniti.

## Caratteristiche di JavaFX 8

Le API JavaFX sono integrate nel JDK Java e, grazie al fatto che una JVM può essere eseguita su tutte le piattaforme desktop più diffuse, consentono la compilazione e la generazione di applicazioni eseguibili su diversi sistemi operativi. Inoltre, a partire da Java 8 è stato introdotto il supporto alle architetture ARM diffusissime nell’ecosistema mobile.

Come vedremo in seguito, un aspetto estremamente interessante di JavaFX è la facilità con cui viene implementato il **pattern MVC** \(_Model-View-Controller_\). Il look and feel può essere modificato facilmente utilizzando un foglio di stile CSS, senza la necessità di dover scrivere nulla nel codice che definisce l’aspetto grafico dei componenti visualizzati se non le chiamate ad una classe specifica o agli id CSS.

JavaFX rende libera la costruzione di un’interfaccia consentendo di definire la disposizione dei componenti attraverso il codice di programmazione e un particolare file XML scritto in linguaggio **FXML**. L’uso dell’FXML permette di separare nettamente la progettazione grafica di una “Scena” dalla logica funzionale ad essa legata, ma con JavaFX possiamo andare oltre, non siamo infatti obbligati ad editare manualmente un file FXML ed è possibile far uso di particolari tool, come **Scene Builder**, che permettono di costruire visualmente ed in modo molto semplice un’interfaccia grafica tramite la generazione automatica di un file FXML.

## Predisporre l’ambiente di sviluppo per JavaFX 8

Per la nostra trattazione di JavaFX 8 faremo uso dell’IDE **Netbeans**, una scelta è motivata dal fatto che questo ambiente di sviluppo è naturalmente integrato con la libreria. Possiamo scaricare NetBeans al seguente [URL](https://netbeans.org/downloads/).

Figura 1. Netbeans download

![Netbeans download](http://www.html.it/wp-content/uploads/2017/03/netbeans.png)

Occorre inoltre disporre delle JDK 8 installate sul proprio sistema. Se non già presenti, è possibile effettuarne il download al seguente [url](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Durante l’installazione di Netbeans è necessario prestare attenzione alla selezione delle JDK 8 presenti sul proprio sistema.

Figura 2. Netbeans e JDK

![Netbeans JDK](http://www.html.it/wp-content/uploads/2017/03/netbeans_jdk.png)

Terminata l’installazione di Netbeans avviamo l’IDE e creiamo, attraverso il menù “file”, un nuovo progetto JavaFX di tipo “JavaFX Application”. Vedremo generare automaticamente una classe demo che implementa un’interfaccia dimostrativa molto semplice.

Partendo da questo progetto nei prossimi capitoli illustreremo la composizione di una interfaccia complessa utilizzando un file CSS per il suo aspetto ed implementando il layout attraverso Java. Successivamente analizzeremo la variante che fa uso di FXML e Scene Builder. Proseguendo in questa direzione implementeremo un semplice applicativo per la conversione di un angolo geometrico da gradi a radianti.

