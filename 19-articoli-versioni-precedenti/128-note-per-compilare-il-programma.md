# 128 Note Per Compilare Il Programma

Visto che il programma è stato creato per un browser \(che hanno le vecchie versioni di Java\) ho dovuto usare String \[\] NOMI=Toolkit.getdefaultToolkit\(\).getfontList\(\); per avere i caratteri, se si compila con JDK 1.2 o 1.3 si deve scrivere:

Ø javac grafDemo.java -deprecation

Il quale darà un warning per dire che si usa un metodo deprecato.  
Se invece userete l’appletviewer per visualizzare l’applet potete cambiare

String \[\] NOMI=Toolkit.getdefaultToolkit\(\).getfontList\(\);

con

String \[\] NOMI=GraphicsEnvironment.  
getLocalGraphicsEnvironment\(\).  
getAvailablefontFamilyNames\(\);

e apprezzare la grande quantità di font presenti.

Nell’esempio ho usato per i colori tre scrollbar, che tra i GUI non avevamo visto.  
Per creare l’oggetto scrollbar ho usato il costruttore:

Scrollbar R=new Scrollbar\(Scrollbar.VERTICAL, 0, 1, 0, 255\);

che crea una scrollbar verticale con valori che vanno da 0 a 255 con valore iniziale 0.  
Poi ho settato il valore della scrollbar con R.setValue\(255\); ed ho detto che l’incremento per ogni click deve essere di 10 con R.setUnitIncrement\(10\);

Per ascoltare i cambiamenti uso un oggetto che implementa la classe AdjustmentListener che ascolta eventi di tipo AdjustmentEvent. Per associare l’ascoltatore all’oggetto scrollbar si usa il metodo addAdjustmentListener.

![metodo addAdjustmentListener](http://html.it/guide/img/guida_java/34.gif)

