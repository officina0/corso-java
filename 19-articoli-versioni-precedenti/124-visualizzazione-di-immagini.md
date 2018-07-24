# 124 Visualizzazione Di Immagini

Iniziamo a vedere i metodi drawImage della classe Graphics, con i quali è possibile visualizzare delle immagini salvate con formato gif o jpg.  
Nella classe Graphics ci sono vari metodi drawImage, essi sono:

abstract boolean drawImage\(Image img, int x, int y, Color bgcolor, ImageObserver observer\) abstract boolean drawImage\(Image img, int x, int y, ImageObserver observer\)  
abstract boolean drawImage\(Image img, int x, int y, int width, int height, Color bgcolor, ImageObserver observer\)  
abstract boolean drawImage\(Image img, int x, int y, int width, int height, ImageObserver observer\)  
abstract boolean drawImage\(Image img, int dx1, int dy1, int dx2, int dy2, int sx1, int sy1, int sx2, int sy2, Color bgcolor, ImageObserver observer\)  
abstract boolean drawImage\(Image img, int dx1, int dy1, int dx2, int dy2, int sx1, int sy1, int sx2, int sy2, ImageObserver observer\)

Tutti i metodi richiedono i due parametri Image e ImageObserver.  
Image rappresenta l’immagine da visualizzare, essa è una classe astratta che fornisce un accesso indipendente dal formato a immagini grafiche, oggetti di questa classe vengono creati usando metodi di altre classi che creano delle immagini, a seconda del conteso in cui si vogliono usare le immagini.  
Ad esempio, volendo disegnare una immagine in un componente Gui, si può usare il metodo createImage\(\) della classe Component.  
Volendo invece disegnare una immagine in un’applet, è possibile usare il metodo getImage\(\) della classe Applet.  
La classe Toolkit ha sia createImage\(\) che getImage\(\).  
L’altro parametro richiesto da tutte le drawImage\(\) è un oggetto che implementa l’interfaccia ImageObserver, e rappresenta in pratica l’oggetto grafico su cui verrà visualizzata l’immagine.

Component implementa l’interfaccia ImageObserver, di conseguenza la implementano tutte le sue estensioni, tra cui:

* Tutti i GUI
* Le Applets
* Tutte le finestre \(tra cui i Frames\)

tra gli altiri parametri ci sono:

* la posizione x in cui verrà visualizzata l’imagine nel componente
* la posizione y in cui verrà visualizzata l’imagine nel componente il colore dello sfondo

Proviamo a fare un’esempio di visualizzazione di una immagine. Creiamo innanzitutto una immagine, ad esempio la seguente

![immagine](http://html.it/guide/img/guida_java/29a.jpg)

e la salviamo in formato gif usando un qualunque editore di immagini, chiamandola ad esempio pietro.jpg.  
A questo punto creiamo la seguente applet:

import java.awt._; // Per la classe Graphics  
import java.applet._; // Per la classe Applet  
import java.net._; // Per leURL  
/_  
Come abbiamo gia detto i files per gli applet non sono altro che degli URL  
\*/  
public class VisualizzaImmagine extends Applet  
{  
Image ImmaginePietro;  
public void init\(\)  
{  
setBackground\(Color.white\);  
ImmaginePietro=getImage\(getdocumentBase\(\),”pietro.jpg”\);  
}  
public void paint\(Graphics g\)  
{  
g.drawImage\(ImmaginePietro,0,0,this\);  
getAppletContext\(\).showStatus\(“Visualizzo l’immagine pietro.jpg, Applet creata da Pietro Castellucci”\);  
}  
}

la mettiamo in un file chiamato VisualizzaImmagine.java e la invochiamo usando il documento html seguente \(Immagine.html\):

 Applet VisualizzaImmagine, visualizza l’immagine pietro.jpg  
 Quella di sopra è una applet, ma quella di sotto no!  
 ![](https://github.com/officina0/corso-java/tree/b61a019c01cccd475f720e51f1283eb6155b3c66/19-Articoli%20versioni%20precedenti/”pietro.jpg”)

Se eseguiamo l’applet ci accorgiamo che non vi è alcuna differenza tra l’immagine caricata nell’applet e quella caricata usando il tag IMG src dell’html, a parte il messaggio che appare quando vi si passa sopra con il mouse, potrebbe sembrare anche un lavoro inutile per le vostre pagine html.

Questo non è assolutamente vero, infatti l’imagine caricata nell’applet appartiene ad un programma che può fare anche cose complesse, ad esempio è possibile aggiungere altri componenti GUI all’applet, oppure fare delle elaborazioni sull’immagine stessa, come mostra il seguente esempio \(VisualizzaImmagine2.java\):

import java.awt._; // Per la classe Graphics  
import java.applet._; // Per la classe Applet  
import java.net._; // Per leURL  
import java.awt.event._; // Per gli eventi  
/  
_Come abbiamo gia detto i files per gli applet non sono altro che degli URL_    
/  
public class VisualizzaImmagine2 extends Applet  
{  
Image ImmaginePietro;  
CheckboxGroup gruppo=new CheckboxGroup\(\);  
Checkbox b1=new Checkbox\(“Ferma”,gruppo,true\);  
Checkbox b2=new Checkbox\(“Animata”,gruppo,false\);  
Panel p=new Panel\(new GridLayout\(1,3\)\);  
public void init\(\)  
{  
setBackground\(Color.white\);  
setLayout\(new borderLayout\(\)\);  
ImmaginePietro=getImage\(getdocumentBase\(\),”pietro.jpg”\);  
p.add\(new Label\(\)\);  
p.add\(b1\);  
p.add\(b2\);  
b1.addItemListener\(new IL\(\)\);  
b2.addItemListener\(new IL\(\)\);  
add\(p,borderLayout.SOUTH\);  
}  
boolean MOVIMENTO=false;

int numero=0;

int inc=1;  
public void paint\(Graphics g\)  
{  
if \(!MOVIMENTO\)  
{  
g.drawImage\(ImmaginePietro,0,0,this\);  
getAppletContext\(\).showStatus\(“Visualizzo l’immagine pietro.jpg, Immagine ferma, Applet creata da Pietro Castellucci”\);

numero=0;  
}  
else  
{  
g.setPaintMode\(\);  
g.drawImage\(Elabora\(ImmaginePietro,numero\),0,0,this\);

for \(int k=0;k&lt;=99;k++\) getAppletContext\(\).showStatus\(“Visualizzo l’immagine pietro.jpg, Frame “+numero+”, Applet creata da Pietro Castellucci”\);

getAppletContext\(\).showStatus\(“Visualizzo l’immagine pietro.jpg, Frame “+numero+”, Applet creata da Pietro Castellucci”\);  
if \(numero&gt;=10\) inc=-1;  
if \(numero&lt;=0\) inc=1;  
numero+=inc;  
}  
repaint\(\);  
}

Image Elabora \(Image img, int frm\)  
{

return img.getScaledInstance\(324-\(frm\*20\), 85-\(frm\*4\),img.SCALE\_FAST\);

}  
public class IL implements ItemListener  
{  
public void itemStateChanged\(ItemEvent e\)  
{

if \(b1.getState\(\)\) MOVIMENTO=false;  
else MOVIMENTO=true;  
repaint\(\);  
}

}  
}

Caricata dal file html seguente:

 Applet VisualizzaImmagine, visualizza l’immagine pietro.jpg  
 Quella di sopra è una applet, ma quella di sotto no!  
 ![](https://github.com/officina0/corso-java/tree/b61a019c01cccd475f720e51f1283eb6155b3c66/19-Articoli%20versioni%20precedenti/”pietro.jpg”)

La nuova classe Graphics2D mette a disposizione altri metodi drawImage, tra cui alcuni che hanno un parametro di tipo Affinetransform che come si capisce dal nome è una trasformazione affine dell’immagine, ovvero una scalatura, una rotazione o una traslazione, o una combinazione di queste.

Per avere una versione senza sfarfallio basta aggiungere la funzione:

public void update\(Graphics g\)  
{  
paint\(g\);  
}

