# 123 Le Funzioni Paint Repaint E Update La Classe Graphics E Graphics 2 D

Java ci permette di disegnare su un’applet semplicemente ridefinendone il metodo `paint`, questo metodo viene automaticamente invocato dal sistema quando la finestra contenente l’applet va in primo piano, quindi ogni comando grafico all’applet sarà dato nel metodo `paint` dell’applet:

void paint \(Graphics g\) { // Disegno }

Quando l’applet ha disegnato, e si vogliono visualizzare eventuali modifiche è possibile invocare il metodo `repaint()` oppure il metodo `update(Graphics g)` che cancella l’area in cui l’applet ha disegnato e ne reinvoca la paint.

_Nota: Il metodo_ `paint(Graphics g)` _non può essere invocato direttamente._

Non solo l’applet ha il metodo `paint` che può essere ridefinito, ma anche gli oggetti di tipo `Component`, che formano la GUI. Anche per modificare l’aspetto grafico dei componenti basta ridefinirne il metodo `paint`.

Tra i componenti c’è la precedentemente citata “tela” su cui è possibile disegnare, essa è un oggetto della classe `Canvas`. Chi è abituato a programmare con altri linguaggi si aspetterà che nella `paint` ci sia un inizializzazione del device su cui si va a disegnare e poi una serie di istruzioni tipo `drawLine`, `drawBox` etc, ma qui non è del tutto vero, ovvero nei `Component` e negli `applet` non c’è nessun metodo grafico.

A questo punto si spiega anche il parametro `g` di tipo `Graphics` della `paint`: è questo l’oggetto che contiene i metodi grafici usabili da Java, in effetti esso rappresenta in qualche modo l’area in cui verranno disegnate le forme primitive.

Graphics è una classe che non può essere instanziata, ma `nella` paint viene ricevuta come paramtero, ed è solo qui che può essere usata, essa contiene i metodi:

abstract void clearRect\(int x, int y, int width, int height\)

abstract void clipRect\(int x, int y, int width, int height\)

abstract void copyArea\(int x, int y, int width, int height, int dx, int dy\)

abstract Graphics create\(\)

Graphics create\(int x, int y, int width, int height\)

abstract void dispose\(\)

void draw3DRect\(int x, int y, int width, int height,boolean raised\)

abstract void drawArc\(int x, int y, int width, int height, int startAngle, int arcAngle\)

void drawBytes\(byte\[\] data, int offset, int length, int x, int y\)

void drawChars\(char\[\] data, int offset, int length, int x, int y\)

abstract boolean drawImage\(...\) // ne esistono diverse versioni

abstract void drawLine\(int x1, int y1, int x2, int y2\)

abstract void drawOval\(int x, int y, int width, int height\)

abstract void drawPolygon\(int\[\] xPoints, int\[\] yPoints, int nPoints\)

void drawPolygon\(Polygon p\)

abstract void drawPolyline\(int\[\] xPoints, int\[\] yPoints, int nPoints\)

void drawRect\(int x, int y, int width, int height\)

abstract void drawRoundRect\(int x, int y, int width, int height, int arcwidth, int arcHeight\)

abstract void drawString\(AttributedCharacterIterator iterator, int x, int y\)

abstract void drawString\(String str, int x, int y\)

void fill3DRect\(int x, int y, int width, int height, boolean raised\)

abstract void fillArc\(int x, int y, int width, int height, int startAngle, int arcAngle\)

abstract void fillOval\(int x, int y, int width, int height\)

abstract void fillPolygon\(int\[\] xPoints, int\[\] yPoints, int nPoints\)

void fillPolygon\(Polygon p\)

abstract void fillRect\(int x, int y, int width, int height\)

abstract void fillRoundRect\(int x, int y, int width, int height, int arcwidth, int arcHeight\)

void finalize\(\)

abstract Shape getClip\(\)

abstract Rectangle getClipBounds\(\)

Rectangle getClipBounds\(Rectangle r\)

abstract Color getColor\(\)

abstract font getfont\(\)

fontMetrics getfontMetrics\(\)

abstract fontMetrics getfontMetrics\(font f\)

boolean hitClip\(int x, int y, int width, int height\)

abstract void setClip\(int x, int y, int width, int height\)

abstract void setClip\(Shape clip\)

abstract void setColor\(Color c\)

abstract void setfont\(font font\)

abstract void setPaintMode\(\)

abstract void setXORMode\(Color c1\)

String toString\(\)

abstract void translate\(int x, int y\)

Avremo modo tra non molto di vedere in dettaglio tutti questi metodi.

In Java 2 la classe `Graphics` è stata potenziata aggiungendo la classe `Graphics2D`, che la estende aggiungendo notevoli potenzialità alla grafica in Java. Per avere un’assaggio delle potenzialità di Java potete vedere la Demo rilasciata insieme al JDK, se ad esempio avete scaricato il JDK 1.3 e lo avete installato in `c:jdk1.3`, la demo si trova in:

c:jdk1.3demojfcJava2D

e per eseguirla bisogna digitare dal prompt:

java -jar Java2Demo.jar

Demo Java

![Demo Java](http://html.it/guide/img/guida_java/29.jpg)

