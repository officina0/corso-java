Rettangoli
----------

Graphics permette di disegnare dei rettangoli specificandone due spigoli opposti usando il metodo:

drawRect(Iniziox,Inizioy,Finex,Finey);

questo metodo è equivalente a:

drawLine(Iniziox,Inizioy,Finex,Inizioy);  
drawLine(Iniziox,Inizioy,Iniziox,Finey);  
drawLine(Finex,Inizioy,Finex,Finey);  
drawLine(Iniziox,Finey,Finex,Finey);

è possibile anche costruire rettangoli che danno una specie di effetto rialzato o incavato usando il metodo draw3DRect(int x,int y,int larghezza,int altezza,boolean rialzato) e rettangoli con gli spigoli arrotondati con il metodo drawRoundRect(int x, int y, int larghezza, int altezza, int larghezzaArco, int altezzaArco).  
E’ inoltre possibile colorare all’interno tutte le figure chiuse usando i metodi fill, ad esempio per colorare un RoundRect bisogna usare fillRoundRect, che ha glistessi parametri della corrispondente draw.

Cerchi ed ellissi
-----------------

Si può disegnare una ellisse usando la drawOval (int x, int y, int larghezza,int altezza) e colorarla usando la fillOval(int x, int y, int larghezza,int altezza).  
Se altezza e altezza sono uguali si disegna un cerchio.

Poligoni
--------

Usando la drawPolygon si disegna un generico poligono. Esistono due tipi di drawPolygon:

drawPoligon( int[] PUNTIx, int PUNTIy, int NUMEROPUNTI);

e

drawPolygon (Polygon p)

La seconda usa un oggetto della classe Polygon, che definisce un poligono.  
Esistono le corrispondenti fillPolygon.

Testo
-----

Per disegnare una stringa si può usare il metodo drawString (String TESTO, int x, int y);  
Un testo può essere disegnato con diverse font, per cambiare le font Graphics mette a disposizione il metodo setfont(font font), dove font è l’oggetto che definisce il tipo di carattere.  
font è la classe che definisce i caratteri, è possibile instanziarla creando uno specifico font, usando font(String name, int style, int size).  
font è una classe abbastanza complicata, però è facile trovare ed usare i fonts del sistema mentre si esegue un programma java:  
Nelle vecchie versioni di java baste scrivere:

String [] NOMI=Toolkit.getdefaultToolkit().getfontList();

Mentre nelle nuove versioni di Java (da JDK 1.2 )

String [] NOMI=GraphicsEnvironment.  
getLocalGraphicsEnvironment().  
getAvailablefontFamilyNames();

è poi facile instanziare oggetti font conoscendo i nomi.

Archi
-----

Si possono disegnare degli archi usando drawArc(int x, int y, int larghezza, int altezza, int angoloIniziale, int angoloArco)

L’uso di tutti questi metodi è illustrato nell’esempio seguente.  
Il file che lo manda in esecuzione è:

<html>  
<head>  
<title>  
Applet grafDemo (grafDemo.class)  
</title>  
</head>  
<body>  
<APPLET code=”grafDemo.class” width=500 height=400>  
</APPLET>  
</body>  
</html>