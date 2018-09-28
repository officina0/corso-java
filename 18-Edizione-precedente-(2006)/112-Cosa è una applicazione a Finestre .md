Le applicazioni “a finestre” sono quelle che più spesso usiamo quando lavoriamo al computer. Su tratta di applicazioni eseguita in locale, che come interfaccia utente utilizzano le tecnologie delle finestre, tipica dei sistemi operativi Mac OS, Windows o XWindows (il server grafico per Linux e Unix). Quasi tutti i programmi che vengono usati attualmente sono applicazioni a finestre ad esempio lo sono: OpenOffice, Explorer, Visual Studio, Photoshop, etc.

Vediamo quindi come **creare una Finestra usando il package AWT**. Al solito creiamo una applicazione con il solito main, come abbiamo già fatto in precedenza. La classe creata però, questa volta, estenderà la classe `java.awt.Frame`, la quale rappresenta la finestra completa, comprendente il titolo, i bottoncini di “riduci a icona”, “massimizza” e “chiudi”.

La classe `Frame` ha diversi metodi e costruttori, che verdremo tra poco, per ora creiamo la nostra prima finestra con titolo “Prima Finestra” e che non contiene niente, la salviamo nel file `Finestra.java`.

```
import java.awt.*;
public class Finestra extends Frame
{
  public Finestra()
  {
    super("Prima Finestra");
    setLocation(100,100);
    setSize(200,100);
    
    setVisible(true);
    // in passato, al posto di setVisible() si utilizzava 
		// show();
		// che ora è un metodo deprecato
  }
  
  public static void main(String[] arg)
  {
    new Finestra();
    System.out.println("Ho creato la finestra");
  }
}

```

All’inizio, come per ogni applicazione viene invocato il `main`, il quale crea un nuovo oggetto di tipo `Finestra` (avremmo potuto anche inserire il `main` e la finestra in due file separati).

Nel costruttore dell’oggetto `Finestra`, viene invocato il costruttore della superclasse, `Frame`, con la stringa `"Prima Finestra"` (il titolo), viene invocato il metodo `setLocation` che dichiara la posizione dell’angolo destro in alto della finestra sul desktop, in questo caso `(100, 100)` (Sono le ascisse e le ordinate rispettivamente, la `x` si misura dal lato sinistro dello schermo e cresce verso destra, la `y` si misura dal lato in alto dello schermo, e cresce andando verso il basso).

Poi si chiama il metodo `setSize`, che permette di specificare larghezza e altezza della finestra, la nostra è larga `200` e alta `100`, e alla fine viene invocato il metodo `setVisible(true)`, che fa apparire la finestra sullo schermo. `setVisible` sostituisce il metodo deprecato `show()` e accetta come parametro un valore booleano: se lo impostiamo a `true` la finestra viene visualizzata, in caso contrario viene nascosta.

![Sistema di coordinate](http://www.html.it/guide/img/guida_java/17.gif)

Le coordinate dello schermo non sono coerenti con le usuali coordinate cartesiane: la `y` cresce verso il basso e diminuisce verso l’alto. Questo è comune a tutti i linguaggi di programmazione e tutte le librerie per la grafica Raster, ed è dovuto a motivi storico-tecnici legati alla modalità di disegno dei pixel sullo schermo a livello hardware e alla necessità di non dover rappresentare numeri negativi.

Compiliamo ed eseguiamo il programma: la nostra prima finestra prenderà vita. Se eliminiamo a turno i metodi `setLocation`, `setSize` e `setVisible`, possiamo esaminare gli effetti che hanno questi metodi sulla finestra.

Nella finestra che abbiamo creato vediamo alcuni **eventi gestiti automaticamente** dal sistema, questi sono il ridimensionamento della finestra, la pressione dei bottoni “riduci ad icona” e “massimizza (minimizza)”, i quali sono gestiti direttamente dalla classe `Frame`.

Non è gestito invece l’evento “chiudi” della finestra (il bottone con la `x`), se esso viene premuto non succede niente, per terminare l’esecuzione del programma bisogna andare sun prompt del dos dal quale è stata lanciata l’applicazione e premere `CtrL+C` (l’exit per tutti i programmi DOS).

Da notare che possiamo associare ad una applicazione più di una sola finestra: Java è un linguaggio che supporta la multiprogrammazione, ogni finestra è un programma a se stante che gira in parallelo agli altri, quindi possiamo creare per la stessa applicazione più finestre Frame, come nell’esempio che segue.

```
import java.awt.*;

public class Finestre extends Frame
{
  public Finestra(String Nome, int x, int y)
  {
    super("Prima Finestra");
    setLocation(x, y);
    setSize(200, 100);
    setVisible(true);
  }
  
  public static void main(String[] args)
  {
    System.out.println("Creo 4 finestre sovrapposte");
    for (int i=1;i<5;i++) new Finestra("Finestra "+i,10+(i\*10), 10+(i\*10));
    
    System.out.println("Creo 4 finestre a scacchiera");
    for (int i=5;i<9;i++) new Finestra("Finestra "+i,(i-5)*200, 100+(i-5));
    
    System.out.println("Ho creato le otto finestre");
	}
}

```

Le stesse cose si potevano fare estendendo la classe JFrame del package `javax.swing`.

```
import javax.swing.*;

public class FinestraSwing extends JFrame
{
  public FinestraSwing()
  {
    super("Prima Finestra");
    setLocation(100,100);
    setSize(200,100);
    setVisible(true);
  }
  
  public static void main(String[] arg)
  {
    new FinestraSwing();
    System.out.println("Ho creato la finestra");
  }
}

```

Questo programma è come `Finestra.java`. solo che estende `JFrame` di `swing`, le uniche differenze sono il contenuto della finestra, questa volta grigio, prima bianco e il bottone "chiudi", che questa volta chiude la finestra (solo la finestra però, non l'applicazione intera)

In effetti l'utilizzo di `swing` e di `awt` è molto simile, solo che `swing` è più completo, esso mette a disposizione molte classi in più, da la possibilità di cambiare il look delle finestre a runtime, e altro.

Purtroppo non tutti i web browser le supportano, noi quindi utilizzeremo le `awt`, e poi discuteremo delle swing, fermo restando che chi vuole realizzare fare un applet per ora usare le awt, quando l'XML diverrà uno standard di fatto, e quindi si dovranno cambiare i browser, si potrà utilizzare tranquillamente swing anche per gli applet.

Vediamo quindi cosa contiene **la classe Frame**, innanzitutto i costruttori sono due:

`Frame()` , che crea un Frame senza nessun titolo, inizialmente invisibile.  
`Frame(String T)`, che crea un Frame con titolo T, anch'esso inizialmente invisibile.

Gli attributi della classe sono:
--------------------------------

```
static int ICONIFIED
static int NORMAL

```

per indicare lo stato della finestra, e

```
static int CROSSHAIR_CURSOR
static int DEFAULT_CURSOR
static int E\_REsize\_CURSOR
static int HAND_CURSOR
static int MOVE_CURSOR
static int N\_REsize\_CURSOR
static int NE\_REsize\_CURSOR
static int NW\_REsize\_CURSOR
static int S\_REsize\_CURSOR
static int SE\_REsize\_CURSOR
static int SW\_REsize\_CURSOR
static int TEXT_CURSOR
static int W\_REsize\_CURSOR
static int WAIT_CURSOR 

```

tutti dichiarati deprecated, per i cursori, rimpiazzati dalla classe Cursor. Eredita gli allineamenti dei componenti da Component.

**I metodi** sono:

Metodo

Descrizione

`void addNotify()`

collega il frame ad una risorsa schermo, e lo rende visualizzabile

`int getCursorType()`

dichiarato Deprecated. È della versione 1.1 delle JDK

`static Frame[] getFrames()`

da un array contenente tutti i Frames creati dall'applicazione

`Image getIconImage()`

da l'icona del Frame, essa è un oggetto di tipo Icon, che vedremo in seguito

`MenuBar getMenuBar()`

restituisce la barra dei menu del frame, è un oggetto di tipo MenuBar, che vedremo tra non molto

`int getState()`

da lo stato del frame

`String getTitle()`

da il titolo del frame

`boolean isResizable()`

da true se il frame può essere allargato e ristretto con il mouse

`protected String paramString()`

da la stringa contenente i parametri del Frame

`void remove(MenuComponent m)`

toglie la MenuBar specificata dal frame

`void removeNotify()`

toglie la connessione tra il frame e la risorsa che rappresenta lo schermo, rendendolo invisualizzabile

`void setCursor(int cursorType)`

impostava il cursore in JDK1.1, è dichiarato deprecated

`void setIconImage(Image image)`

imposta l'icona del Frame

`void setMenuBar(MenuBar mb)`

assegna una MenuBar al frame

`void setresizable(boolean resizable)`

imposta il Frame ridimensionabile oppure no, per default lo è

`void setState(int state)`

imposta lo stato del frame

`void setTitle(String title)`

imposta il titolo del frame

`Frame` è una classe che estende `java.awt.Window`, e quindi ne eredita metodi e attributi.  
In effetti `Frame` è una `Window` con in più un bordo e una `MenuBar`. I metodi ereditati da window sono:

Metodi ereditati da Window

addWindowListener, applyResourceBundle, applyResourceBundle, dispose, getFocusOwner,  
getInputContext, getLocale, getOwnedWindows, getOwner, getToolkit, getWarningString, isShowing, pack, postEvent, processEvent, processWindowEvent, removeWindowListener, show, toBack, toFront

`Window` estende `java.awt.Container`, esso è un contenitore di oggetti AWT, è un componente che può contenere altri componenti, quindi per transitività `Frame` ne eredita metodi e attributi, i metodi sono:

Metodi e attributi ereditati da Container

add, add, add, add, add, addContainerListener, addImpl, countComponents, deliverEvent, doLayout, findComponentAt, findComponentAt, getalignmentX, getalignmentY, getComponent, getComponentAt, getComponentAt, getComponentCount, getComponents, getInsets, getLayout, getMaximumsize, getMinimumsize, getPreferredsize, insets, invalidate, isAncestorOf, layout, list, list, locate, minimumsize, paint, paintComponents, preferredsize, print, printComponents, processContainerEvent, remove, remove, removeAll, removeContainerListener, setfont, setLayout,  
update, validate, validatetree

`Container` estende `java.awt.Component`, un `Component` è un oggetto che ha una rappresentazione grafica, ad esempio un bottone sarà una estensione di questa classe, la quale estende a sua volta `java.lang.Object`.

Attributi ereditati da Component

BOTTOM\_alignMENT, CENTER\_alignMENT, LEFT\_alignMENT, RIGHT\_alignMENT, TOP_alignMENT

Attributi ereditati da Component

action, add, addComponentListener, addFocusListener, addInputMethodListener, addKeyListener,  
addMouseListener, addMouseMotionListener, addPropertyChangeListener, addPropertyChangeListener, bounds, checkImage, checkImage, coalesceEvents, contains, contains, createImage, createImage, disable, disableEvents, dispatchEvent, enable, enable, enableEvents, enableInputMethods, firePropertyChange, getBackground, getBounds, getBounds, getColorModel, getComponentOrientation, getCursor, getdropTarget, getfont, getfontMetrics, getForeground, getGraphics, getHeight, getInputMethodRequests, getLocation, getLocation, getLocationOnScreen, getName, getParent, getPeer, getsize, getsize, gettreeLock, getwidth, getX, getY, gotFocus, handleEvent, hasFocus, hide, imageUpdate, inside, isDisplayable, isDoubleBuffered, isEnabled, isFocustraversable, isLightweight, isOpaque, isValid, isVisible, keyDown, keyUp, list, list, list, location, lostFocus, mouseDown, mouseDrag, mouseEnter, mouseExit, mouseMove, mouseUp, move, nextFocus, paintAll, prepareImage, prepareImage, printAll, processComponentEvent, processFocusEvent, processInputMethodEvent, processKeyEvent, processMouseEvent, processMouseMotionEvent, removeComponentListener, removeFocusListener, removeInputMethodListener, removeKeyListener, removeMouseListener, removeMouseMotionListener, removePropertyChangeListener, removePropertyChangeListener, repaint, repaint, repaint, repaint, requestFocus, reshape, resize, resize, setBackground, setBounds, setBounds, setComponentOrientation, setCursor, setdropTarget, setEnabled, setForeground, setLocale, setLocation, setLocation, setName, setsize, setsize, setVisible, show, size, toString, transferFocus

Mentre da Object vengono ereditati i soliti metodi:

Attributi ereditati da Object

clone, equals, finalize, getClass, hashCode, notify, notifyAll, wait, wait, wait

Alcuni di questi li vedremo, per gli altri vi consiglio di consultare la Documentazione del JDK, che li descrive tutti in dettaglio.

Diagramma delle estensioni di Frame

![diagramma delle estensioni di Frame](http://html.it/guide/img/guida_java/18.gif)

A questo punto si capisce come è importante l'estensione delle classi in Java, infatti in Frame possono essere invocati tutti i metodi di sopra.

Mentre quello di è:

Estensioni di JFrame di swing

![Estensioni di JFrame di swing](http://html.it/guide/img/guida_java/19.gif)

Quindi Jframe eredita tutti i metodi di Frame essendo derivato da essa, in più ne definisce altri.