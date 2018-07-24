Prima di cominciare questo che sarà uno dei capitoli più interessanti del corso, introduciamo il package awt e cosa è un componente GUI.  
Vedremo anche dei componenti che sono contenitori per altri componenti.  
Il package Abstract Window Toolkit, mette a disposizione del programmatore una serie di classi e Interfacce per la gestione di interfacce grafiche.  
Il package java.awt ha anche dei sottopackage, essi sono:

java.awt.color  
java.awt.datatransfer  
java.awt.dnd  
java.awt.event  
java.awt.font  
java.awt.geom  
java.awt.im  
java.awt.image  
java.awt.image.renderable  
java.awt.print  

  

Di cui vedremo alcune funzionalità. Sulle classi awt sono state costruite le classi del package swing, che è attualmente il package più utilizzato per costruire delle interfacce grafiche, e che vedremo tra non molto.  
Vediamo cosa contiene awt:

Interfacce
----------

ActiveEvent, interfaccia per eventi che sanno come sono stati attivati.  
Adjustable, interfaccia per oggetti che hanno un valore numerico compreso in un range di valori.  
Composite, interfaccia che serve per comporre le primitive grafiche di awt.  
CompositeContext, ambiente ottimizzato per le operazioni di composizione di primitive.  
ItemSelectable, interfaccia per oggetti che contengono un insieme di item, di cui possono essere  
selezionati anche zero o più di uno(Di solito si può selezionare almeno e al massimo un item, questa interfaccia elimina questa limitazione).  
LayoutManager, interfaccia per le classi che conterranno dei gestori di layout.  
LayoutManager2, come quello di sopra.  
MenuContainer, superclasse di tutti i menu.  
Paint, questa interfaccia definisce i colori da usare per le operazioni grafiche usando Graphics2D.  
PaintContext, ambiente ottimizzato per le operazioni di composizione di primitive grafiche usando  
Graphics2D.  
PrintGraphics, definisce il contesto per stampare.  
Shape, definizioni per costruire una forma geometrica.  
Stroke, ulteriori decorazioni per le forme.  
transparency, definisce i più comuni tipi di trasparenza.

Classi
------

AlphaComposite, classe che implementa le composizioni del fattore alfa dei colori trattando il blending e la trasparenza di grafica e immagini.  
AWTEvent, root degli eventi di AWT.  
AWTEventMulticaster, dispatcher per eventi AWT, efficiente e multiprogrammabile, effettua il multicast di eventi a vari componenti.  
AWTPermission, per i permessi AWT.  
BasicStroke, attributi per le linee esterne delle primitive grafiche.  
borderLayout, è il primo gestore di layout, questi gestori, che vedremo tra pochissimo, servono a disporre gli elementi (i componenti) nei contenitori. Questo divide il contenitore in 5 regioni: nord, sud, est, ovest e centro.  
Button, classe dei bottoni.  
Canvas, area in cui una applicazione può disegnare.  
CardLayout, altro gestore di layout.  
Checkbox, CheckboxGroup, CheckboxMenuItem esse gestiscono i bottoni di tipo booleano (premuto non premuto)  
Choice, presenta un menu a tendina per delle scelte.  
Color, colori in RGB o in altri arbitrari spazi di colori.  
Component, Classe da cui derivano tutti i componenti GUI (bottoni, label,…)  
ComponentOrientation, orientamento dei componenti  
Container, contenitore di componenti.  
Cursor, cursori.  
Dialog, finestre di dialogo, sono quelle che segnalano errori e non solo.  
Dimension, casse che gestisce le dimensioni dei componenti, altezza e larghezza.  
Event, classe che gestisce gli eventi secondo il veccio modello di gestone degli eventi (Java 1.0), è rimasto per fare girare ancora le vecchie applicazioni e applet Java, ma è dichiarato deprecated. In effetti questo modello è stato cambiato perché in situazioni complesse, ovvero con molti componenti messi uno sull’altro, non si riusciva a capire quale componente stesse ricevendo l’evento.  
EventQueue, coda di eventi.  
FileDialog, dialog speciale che gestisce l’input e l’output dei file. E’ molto comoda.  
FlowLayout, altro gestore di layout.  
font, fonts per il testo.  
fontMetrics, attributi per le font.  
Frame, classe già vista che implementa le finestre, con titolo e bordo.  
GradientPaint, riempie le figure con colori lineari con gradiente.  
Graphics, contesto grafico in cui si può disegnare.  
Graphics2D, nuovo contesto grafico, inserito con Java 2, che da altre sofisticate possibilità al disegno.  
GraphicsConfiguration, descrive le caratteristiche della desitnazione della grafica che può essere il monitor o la stampante.  
GraphicsConfigTemplate, usata per ottenere una GraphicsConfiguration valida.  
GraphicsEnvironment, descrive gli ambienti grafici e i font utilizzabili in ogni specifica piattaforma da Java.  
GraphicsDevice, descrive il GraphicDevice utilizzabile in un particolare ambiente.  
GridBagConstraints, GridBagLayout, GridLayout, gestori di layout.  
Image, superclasse per tutte le classi che rappresentano una immagine grafica.  
Insets, rappresenta il bordo di un contenitore.  
Label, etichetta.  
List, lista  
Mediatracker, classe di utilità per gestire oggetti multimediali.  
Menu, menu di tipo pull-down (a discesa).  
MenuBar, barra dei menu.  
MenuComponent, superclasse di tutti i componenti dei menu,.  
MenuItem, Voce di menu.  
MenuShortcut, serve a rappresentare l’acceleratore per una voce di menu.  
Panel, contenitore.  
Point, punto nelle coordinate x,y.  
Polygon, poligono.  
PopupMenu, menu di tipo a scomparsa.  
PrintJob, serve ad inizializzare e ad eseguire le stampe.  
Rectangle, rettangolo.  
RenderingHints, contiene aiuti per il rendering usati da Graphics2D.  
RenderingHints.Key, definisce i tasti usati per controllare i vari aspetti del rendering.  
Scrollbar, barra di scorrimento.  
ScrollPane, pannello con due barre di scorrimento.  
SystemColor, rappresenta i colori simbolici di un oggetto GUI in un sistema.  
TextComponent, superclasse di tutti i componenti che contengono testo.  
TextArea, area di testo.  
TextField, singola linea di testo.  
TexturePaint, come riempire una figura con una texture.  
Toolkit, superclasse dell’ Abstract Window Toolkit.  
Window, finestra (senza bordo e barra di menu).

Eccezioni
---------

AWTException  
IllegalComponentStateException

Errori
------

AWTError

  

Vista questa panoramica sul package cerchiamo di capire a cosa servono queste classi.  
tra tutte le classi, distinguiamo alcune classi nel package per quello che fanno.  
Innanzitutto abbiamo visto delle classi Frame, Dialog e Windows, le quali rappresentano delle finestre, queste finestre conterranno dei menu (le prime due) e dei componenti GUI. Per contenerli Devono avere dei contenitori, di questa specie abbiamo visto Container e Panel. Ogni Contenitore può contenere un Layout Manager, che gestisce il posizionamento di più componenti nello stesso contenitore, e può contenere anche altri contenitori, in modo da creare strutture anche complesse.  
Infine abbiamo visto i componenti GUI veri e propri che vanno aggiunti ai contenitori, ad esempio Label. Nell’esempio che segue attacchiamo una label al contenitore di un Frame.

import java.awt.*;  
public class Etichetta extends Frame  
{  
Label et=new Label();  
public Etichetta(String etichetta)  
{  
et.setText(etichetta);  
setTitle(“Etichetta “+etichetta);  
add(et);  
pack();  
show();  
}  
public static void main (String\[\] a)  
{  
try  
{new Etichetta(a\[0\]);}  
catch (ArrayIndexOutOfBoundsException e)  
{System.out.println(“ERRORE: battere java Etichetta StrINGA”);};  
}  
}  

  

Come abbiamo già detto, i GUI vanno sia su Frame che su Applet, quindi si può pensare alla versione applet del programma precedente.

import java.applet.*;  
import java.awt.*;  
public class ApplEtichetta extends Applet  
{  
Label et=new Label();  
public void init ()  
{  
et.setText(“Ciao”);  
add(et);  
}  
}  

  

Che sarà messo in un file chiamato ApplEtichetta.java e sarà chiamato da un file html contenente:

<html>  
<head>  
<title>  
</title>  
</head>  
<body>  
<APPLET code=”ApplEtichetta.class” width=200 height=100>  
</APPLET>  
</body>  
</html>

Nel package swing è tutto molto simile, anche se la gestione dei contenitori è leggermente differente.