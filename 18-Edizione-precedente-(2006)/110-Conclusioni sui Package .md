In questo capitolo abbiamo visto alcuni package contenenti le API del linguaggio Java, ma ce ne sono degli altri:

java.applet , che vedremo nel prossimo capitolo, il quale serve per creare dei programmi che girano  
sui web browsers, chiamati applet.  
java.awt, questo package e i suoi sottopackages implementano le classi per implementare i controlli  
GUI, per implementare interfacce grafiche, oltre agli strumenti per il disegno, manipolazione di immagini, stampa e altre funzionalità. Inizieremo a vederlo nel prossimo capitolo  
java.beans, package che permette di definire componenti Java e di utilizzarli in altri programmi.  
java.rmi, package per l’invocazione di metodi remoti, ovvero di metodi di oggetti che si trovano ovunque sulla rete, per costruire delle applicazioni distribuite.  
java.security, ci sono classi che implementano le funzioni di sicurezza, come ad esempio classi utilizzate per crttografare documenti prima di mandarli in rete.  
java.sql, interfaccia tra il linguaggio Java e il linguaggio per basi di dati SQL.  
java.text, classi molto utili per l’interazionalizzazione.  
javax.accessibility, classi che supportano tecnologie a sostegno degli utenti disabili.  
javax.swing, è un estensione di java.awt, per costruire applets e applicazioni grafiche è un portento.  
org.omg.CORBA, permette di interfacciare il linguaggio Java con il linguaggio CORBA.

Ancora una volta vi invito per saperne di più a controllare la documantazione del JDK, disponibile On line, sia per essere scaricata che per essere consultata presso il sito della Sun Microsystem www.sun.com.

Questi sono i package standard del linguaggio Java, a questi si aggiungono le estensioni standard del linguaggio. Le estensioni standard sono dei package che nelle prossime versioni di java diventeranno package standard, e che per ora sono versioni beta. Swing è stata una estensione fino all’uscita di Java2, ora di fatto è più utilizzata delle vecchie awt.

API Servlet
-----------

è destinata alla programmazione di applicazioni server in Java. Questa API è costituita dai packages javax.servlet e javax.servlet.http.

Java 3D
-------

gestisce il disegno tridimensionale, è come la versione Java di OpenGL (JavaGL), la  
famosissima libreria della SGI (alcuni la conoscono come Glide, ovvero la libreria OpenGL  
per le 3Dfx) e DirectX della Microsoft.  
Si può scaricare al sito: [http://java.sun.com/products/java-media/3D/index.html](http://java.sun.com/products/java-media/3D/index.html "Link esterno")

Java Media Framework
--------------------

gestisce all’interno dei programmi Java vari formati audio, video  
multimediali, i file supportati sono i seguenti:  
.mov, .avi, .viv, .au, .aiff, .wav, .midi, .rmf, .gsm, .mpg, .mp2, .rtp.  
Se non tutta, almenu una parte diverrà standard con Java 1.3, la quale è in uscita (fine Aprile).  
Si scarica al sito:  
[http://www.javasoft.com](http://www.javasoft.com/ "Link esterno")

Speech
------

funzioni di riconoscimento vocale, non solo per i comandi, ma è possibile anche editare  
interi file. Questo package fa anche l’output vocale.  
Si può scaricare al sito: [http://java.sun.com/products/java-media/speech/index.html](http://java.sun.com/products/java-media/speech/index.html "Link esterno")

Telephony
---------

funzioni di telefonia, fax.

JavaMail
--------

classi per gestire la posta elettronica.

Java Naming and Directory Services
----------------------------------

per accedere ai servizi di nomi e directory usando  
protocolli come LDAP.  
Questo packages è diventato standard in JDK 1.3

Java Management
---------------

per la gestione di reti locali.

JavaSpaces
----------

ulteriori classi per la creazione di applicazioni distribuite.

JavaCommerce
------------

per il commercio elettronico.

Personalmente non vedo l’ora che diventino standard le API Java 3D, Java Media Framework, JavaSpeech e Java Telephony, parchè le funzionalità promesse da queste API sono veramente eccezionali. Usarle adesso è possibile, ma a proprio rischio e pericolo, infatti esse sono ancora delle versioni beta, e quindi piene di errori, inoltre se si vogliono scrivere degli applet usando queste nuove funzionalità è possibile, ma per mandarle in esecuzione occorre l’appletviewer del JDK, perché sicuramente il Java implementato nei web browser ancora non le supporta. Si pensi che Swing è divenuto un package standard del linguaggio, ma ancora esistono dei browser che non lo supportano.