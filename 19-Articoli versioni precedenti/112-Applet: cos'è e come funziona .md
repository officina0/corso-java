Un’applet non è altro che una applicazione Java che gira su web. L’applet presenta qualche differenza con le applicazioni, infatti essi non hanno nessun main, sono delle classi, chiamate come il file che le contiene, che estendono la classe Applet del package java.applet.  
Anche per gli applet esiste la versione JApplet di swing, che viene usata per inserire componenti Swing invece che componenti AWT.  
Un applet ha bisogno di un file html che la richiama, ad esempio sia PrimoApplet.java l’applet che vogliamo eseguire, lo compiliamo e il compilatore ci genera PrimoApplet.class, per eseguirlo abbiamo bisogno di un file html che al suo interno contenga il TAG:

<applet code=”PrimoApplet.class” ></applet>

  

Supponiamo che questo file si chiami pippo.html, a questo punto abbiamo due modi di eseguire l’applet, il primo in fase di debug è usando il programma appletviewer di JDK, e scriveremo in questo caso dal prompt del dos:

appletviewer pippo.html

il secondo, è usando un web browser, come Explorer o Netscape richiamando il file pippo.html.  
Come si vede, si lancia sempre il file html, e non come accadeva per le applicazioni il file .class, è il file html che richiama l’applicazione .java.  
Per maggiori dettagli sulle pagine html vi consiglio di consultare sul sito HTML.IT le sezioni riguardanti l’argomento, noi faremo delle pagine scarne che serviranno solo a caricare i nostri applet, ad esempio per l’applet PrimoApplet.class di prima, il file pippo.html sarà qualcosa del tipo:

<html>  
<head>  
<title>Applet PrimoApplet</title>  
</head>  
<body>  

Il seguente è

l’applet PrimoApplet.class

  

<applet code=”PrimoApplet.class” width=100 height=100>Il tuo browser è vecchio, cambialo!</APPLET>  
</body>  
</html>

  

Abbiamo visto come mandare in esecuzione l’applet, adesso creiamolo. Innanzitutto dobbiamo creare una classe chiamata PrimoApplet, che estende la classe java.applet.Applet, e dobbiamo definire alcuni metodi che il sistema (appletviewer o il browser) invocherà automaticamente. Uno di questi metodi si chiama paint(Graphics O), e Graphics è un oggetto che rappresenta lo schermo dell’applet, noi lo ridefiniremo in modo da fare uscire sullo schermo quello che preferiamo.  
Useremo il metodo drawString della classe Graphics per stampare una stringa su schermo.  
Il programma PrimoApplet.java è il seguente

import java.applet.*;  
import java.awt.*; public class PrimoApplet extends Applet  
{ public void paint (Graphics g)  
{  
g.drawString(“Ciao, io sono il primo applet.”,0,50);  
} }

Il package applet contiene tre interfacce ed una classe:  
Interfacce

AppletContext

,

questa interfaccia corrisponde all’ambiente dell’applet, ovvero al documento che lo contiene e agli altri applets contenuti nello stesso documento.  
AppletStub, riguarda l’ambiente di esecuzione dell’applet, sia esso il browser che l’appletviewer. AudioClip, l’interfaccia è una semplice astrazione per suonare degli audio.

Classe  
Applet

  

Analizziamo la classe Applet più in dettaglio.  
Il costruttore è unico, senza argomenti.  
Applet() Vi sono alcuni metodi chiamati dal browser o dall’appletviewer automaticamente, essi sono: void init(), questo metodo viene invocato appena l’applet viene completamente caricato nel sistema.  
Esso tipicamente viene utilizzato per inizializzare l’applet.

void start(), chiamato quando il sistema manda in esecuzione l’applet, lo informa di questo avvenimento.  
void stop(), chiamato quando il sistema stoppa l’esecuzione dell’applet, lo informa dell’evento, chiamato quando si preme il bottone STOP dell’appletviewer, si cambia pagina nel browser.  
void destroy(),

chiamato quando l’applet viene distrutto, ovvero quando si cambia esce dal browser o dall’appletviewer

Quindi il ciclo di vita di un’applet è il seguente:

*   Viene caricato, e quindi viene chiamato init();
*   Viene mandato in esecuzione, viene chiamato start(), esso invoca il metodo paint() della superclasse Container;
*   Viene stoppato premendo lo stop del browser oppure quando la finestra che lo contiene non è più in primo piano, e viene chiamato stop(), mentre quando ripassa in primo piano ne viene richimato lo start();
*   Viene infine distrutto quando si esce dal browser che lo ha mandato in esecuzione, innanzitutto viene invocato lo stop() e poi il destroy();

Nell’esempio successivo vengono visualizzate le precedenti fasi, per vedere i risultati con un browser, visto che l’output sono delle System.out.print(), selezionare in strumenti (tool), show java consolle (o simili), con l’appletviewer invece l’output viene scritto nella finestra dal quale viene invocato l’html che invoca l’applet.  
Lo editiamo nel file Stadi.java:

import java.applet.*;  
public class Stadi extends Applet  
{  
public Stadi()  
{  
System.out.println(“Invocato il costruttore di Stadi”);  
}  
public void init()  
{  
super.init();  
System.out.println(“Eseguito public void init()”);  
}  
public void start()  
{  
super.start();  
System.out.println(“Eseguito public void start()”);  
}  
public void stop()  
{  
super.stop();  
System.out.println(“Eseguito public void stop()”);  
}  
public void destroy()  
{  
super.destroy();  
System.out.println(“Eseguito public void destroy()”);  
}  
}  

  

Lo compiliamo con: javac Stadi.java  
Per caricarlo creeremo il file Stadi.html che conterrà:

<html>  
<head>  
<title>Stadi.html carica Stadi.class</title>  
</head>  
<body>  
Il seguente è l’applet Stadi, che fa vedere gli stadi attraversati dall’applet.  
<BR>  
<applet code=”Stadi.class” width=200 height=100>Il tuo browser è vecchio, cambialo!</APPLET>  
</body>  
</html>

Per eseguirlo batteremo : appleviewer Stadi.html, oppure faremo apri sul file Stadi.html con il nostro browser preferito. Gli altri metodi della classe Applet sono:

AppletContext getAppletContext(), da l’AppletContext associato all’applet, ovvero il documento che lo ha mandato in esecuzione e gli altri applet invocati da questo.  
String getAppletInfo(), da informazioni sull’applet, deve essere sovrascritta, la normale da null.  
AudioClip getAudioClip(URL url), da l’oggetto di tipo AudioClip associato all’URL inserito. Ricordo che l’URL è una risorsa del web.  
AudioClip getAudioClip(URL url, String name), da l’oggetto di tipo AudioClip associato all’URL e al nome.  
URL getCodeBase(), da l’url associato all’applet.  
URL getdocumentBase(), da l’url del documento html che ha invocato l’applet.  
Image getImage(URL url), da l’ oggetto di tipo Image associata all’url inserito, essa può essere stampata sullo schermo.  
Image getImage(URL url, String name), da l’oggetto di tipo Image associato all’url e al nome.  
Locale getLocale(), da l’oggetto di tipo Locale associato all’applet, esso si cura dell’internazionalizzazione, si trova nel package java.util.  
String getParameter(String name), da il valore del parametro chiamato name preso dalla pagina html che invoca l’applet. L’applet infatti può essere invocato con dei valori di ingresso, come l’args del main, questo metodo li preleva.

Ad esempio se invoco l’applet Clock.class così:

<applet code=”Clock” width=50 height=50>  
<param name=Color value=”blue”>  
</applet>

Se nel codice dell’applet scriverò getParameter(“Color”) il risultato che otterrò sarà “blue”.

String\[\]\[\] getParameterInfo(), da un array che contiene informazioni sui parametri dell’applet. boolean isActive(), dice se l’applet è attivo.  
static AudioClip newAudioClip(URL url), prende un AudioClip da un dato url.  
void play(URL url), suona l’audio clip preso dall’url assoluto.  
void play(URL url, String name), suona il Clip dato dall’url e dal nome specificato.  
void resize(Dimension d) o void resize(int width, int height), richiede all’applet di modificare le proprie dimensioni. Dimension è un oggetto awt che è una dimensione, ovvero un altezza e una larghezza.  
void setStub(AppletStub stub), setta l’AppletStub dell’applet con il nuovo stub.  
void showStatus(String msg), Richiede all’applet che la stringa venga stampata nella finestra di stato dell’applet.

Un applet è una estensione di Panel, il quale è un semplice contenitore, da questo eredita il metodo: addNotify

Panel estende Container da cui eredita, e fa ereditare ad Applet i metodi: add, add, add, add, add, addContainerListener, addImpl, countComponents, deliverEvent, doLayout, findComponentAt, findComponentAt, getalignmentX, getalignmentY, getComponent, getComponentAt, getComponentAt, getComponentCount, getComponents, getInsets, getLayout, getMaximumsize, getMinimumsize, getPreferredsize, insets, invalidate, isAncestorOf, layout, list, list, locate, minimumsize, paint, paintComponents, paramString, preferredsize, print, printComponents, processContainerEvent, processEvent, remove, remove, removeAll, removeContainerListener, removeNotify, setfont, setLayout, update, validate, validatetree

A sua volta Container estende Component, e quindi vi sono gli attributi

BOTTOM\_alignMENT, CENTER\_alignMENT, LEFT\_alignMENT, RIGHT\_alignMENT, TOP_alignMENT

e i metodi:

action, add, addComponentListener, addFocusListener, addInputMethodListener, addKeyListener, addMouseListener, addMouseMotionListener, addPropertyChangeListener, addPropertyChangeListener, bounds, checkImage, checkImage, coalesceEvents, contains, contains, createImage, createImage, disable, disableEvents, dispatchEvent, enable, enable, enableEvents, enableInputMethods, firePropertyChange, getBackground, getBounds, getBounds, getColorModel, getComponentOrientation, getCursor, getdropTarget, getfont, getfontMetrics, getForeground, getGraphics, getHeight, getInputMethodRequests, getLocation, getLocation, getLocationOnScreen, getName, getParent, getPeer, getsize, getsize, gettreeLock, getwidth, getX, getY, gotFocus, handleEvent, hasFocus, hide, imageUpdate, inside, isDisplayable, isDoubleBuffered, isEnabled, isFocustraversable, isLightweight, isOpaque, isValid, isVisible, keyDown, keyUp, list, list, list, location, lostFocus, mouseDown, mouseDrag, mouseEnter, mouseExit, mouseMove, mouseUp, move, nextFocus, paintAll, prepareImage, prepareImage, printAll, processComponentEvent, processFocusEvent, processInputMethodEvent, processKeyEvent, processMouseEvent, processMouseMotionEvent, removeComponentListener, removeFocusListener, removeInputMethodListener, removeKeyListener, removeMouseListener, removeMouseMotionListener, removePropertyChangeListener, removePropertyChangeListener, repaint, repaint, repaint, repaint, requestFocus, reshape, resize, resize, setBackground, setBounds, setBounds, setComponentOrientation, setCursor, setdropTarget, setEnabled, setForeground, setLocale, setLocation, setLocation, setName, setsize, setsize, setVisible, show, size, toString, transferFocus

E quindi quelli di Object:  
clone, equals, finalize, getClass, hashCode, notify, notifyAll, wait, wait, wait

La gerarchia è:

![gerarchia object](http://html.it/guide/img/guida_java/19a.gif)

Facciamo un altro piccolo applet chiamati Info.java

import java.applet.*;  
import java.awt.*;  
import java.awt.image.*;  
public class Info extends Applet implements ImageObserver  
{  
  
public Info()  
{  
}  
public void init()  
{  
super.init();  
setBackground(Color.yellow);  
resize(400,200);  
}  
  
public void start()  
{  
super.start();  
}  
public void stop()  
{  
super.stop();  
}  
  
public void destroy()  
{  
super.destroy();  
}  
  
public void paint (Graphics g)  
{  
g.setColor(Color.darkGray);  
String p=getAppletInfo();  
if (p!=null) g.drawString(p,10,10);  
g.drawString(“CODE:”+getCodeBase().toString(),10,20);  
g.drawString(“DOC:”+getdocumentBase().toString(),10,30);  
Image io=getImage(getCodeBase(),”me.JPG”);  
// Per visualizzare questa imagine ho bisogno che Info implementi l’interfaccia ImageObserver.  
// g.drawImage(Image,x,y,ImageObserver);  
g.drawImage(io,10,40,this);  
g.drawString(“Questo sono io”,80,80);  
String nome=getParameter(“parametro3”);  
String cognome=getParameter(“parametro2”);  
String eta=getParameter(“parametro1”);  
g.drawString(“Esegue il programma”,10,150);  
g.drawString(nome,10,160);  
g.drawString(cognome,10,170);  
g.drawString(“di “+eta+” anni”,10,180);  
}  
public String getAppletInfo()  
{  
return “Applet di Pietro Castellucci”;  
}  
public String\[\]\[\] getParameterInfo()  
{  
String\[\]\[\] r={  
{“parametro1″,”intero”,”Tua età”},  
{“parametro2″,”Stringa”, “Tuo Cognome”},  
{“parametro3″,”Stringa”,”Tuo Nome”}  
};  
return r;  
}  
}  

Per caricare l’applet creeremo un file chiamato Info.html, che conterrà:

<html>  
<head>  
<title>Info.html carica Info.class</title>  
</head>  
<body>  
Il seguente è l’applet Info.  
<BR>  
<applet code=”Info.class” width=400 height=200>  
<param name=parametro1 value=”ETA’ DI CHI ESEGUE IL PROGRAMMA”>  
<param name=parametro2 value=” COGNOME DI CHI ESEGUE IL PROGRAMMA “>  
<param name=parametro3 value=” NOME DI CHI ESEGUE IL PROGRAMMA “>  
Il tuo browser è vecchio, cambialo!  
</APPLET>  
</body>  
</html>

Mi raccomando di specificare i parametri parametro1, parametro2 e parametro3, perché nell’applet non si fa nessun controllo sulla loro definizione, quindi l’applet fa una eccezione non catturata, inoltre nella directory dove mettete l’applet, ci deve essere un file chiamato me.JPG, che è un’immagine 67×88 pixel.