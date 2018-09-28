Ci rimane da vedere un componente fondamentale per un interfaccia, ovvero il Testo.  
Le abstract window Toolkit di Java mettono a disposizione due classi per il testo:  
TextArea e TextField, ambedue estendono la classe TextComponent.

![TextArea e TextField](http://html.it/guide/img/guida_java/25.gif)

In TextComponent troviamo una serie di metodi utili per la gestione del testo, sia in TextArea che in TextField, tra cui:

Color getBackground() e setBackground(Color c), per prendere e settare il colore di Background del testo.  
String getSelectedText(), prende il testo selezionato.  
int getSelectionEnd(), da la posizione della fine selezione del testo.  
Int getSelectionStart(), da la posizione dell’inizio selezione del testo.  
String getText(), da il testo del TextComponent  
boolean isEditable(), dice se il testo del componente è editabile.  
void select(int selectionStart, int selectionEnd), seleziona il testo da selectionStart a selectionEnd.  
void selectAll(), seleziona tutto il testo.  
void setEditable(boolean b), setta se il testo è editabile o meno.  
void setSelectionEnd(int selectionEnd), setta la fine della selezione del testo.  
void setSelectionStart(int selectionStart), setta l’inizio della selezione.  
void setText(String t), setta il contenuto del componente di testo con il valore t.

Inoltre in TextComponent si possono usare tutti i metodi di Component, ovvero:

action, add, addComponentListener, addFocusListener, addHierarchyBoundsListener, addHierarchyListener, addInputMethodListener, addKeyListener, addMouseListener, addMouseMotionListener, addNotify, addPropertyChangeListener, addPropertyChangeListener, bounds, checkImage, checkImage, coalesceEvents, contains, contains, createImage, createImage, deliverEvent, disable, disableEvents, dispatchEvent, doLayout, enable, enable, enableEvents, enableInputMethods, firePropertyChange, getalignmentX, getalignmentY, getBounds, getBounds, getColorModel, getComponentAt, getComponentAt, getComponentOrientation, getCursor, getdropTarget, getfont, getfontMetrics, getForeground, getGraphics, getGraphicsConfiguration, getHeight, getInputContext, getInputMethodRequests, getLocale, getLocation, getLocation, getLocationOnScreen, getMaximumsize, getMinimumsize, getName, getParent, getPeer, getPreferredsize, getsize, getsize, getToolkit, gettreeLock,  
getwidth, getX, getY, gotFocus, handleEvent, hasFocus, hide, imageUpdate, inside, invalidate, isDisplayable, isDoubleBuffered, isEnabled, isFocustraversable, isLightweight, isOpaque, isShowing, isValid, isVisible, keyDown, keyUp, layout, list, list, list, list, list, locate, location, lostFocus, minimumsize, mouseDown, mouseDrag, mouseEnter,  
mouseExit, mouseMove, mouseUp, move, nextFocus, paint, paintAll, postEvent, preferredsize, prepareImage, prepareImage, print, printAll, processComponentEvent, processFocusEvent, processHierarchyBoundsEvent,  
processHierarchyEvent, processInputMethodEvent, processKeyEvent, processMouseEvent, processMouseMotionEvent, remove, removeComponentListener, removeFocusListener, removeHierarchyBoundsListener, removeHierarchyListener, removeInputMethodListener, removeKeyListener, removeMouseListener, removeMouseMotionListener, removePropertyChangeListener, removePropertyChangeListener, repaint, repaint, repaint, repaint, requestFocus, reshape, resize, resize, setBounds, setBounds, setComponentOrientation, setCursor, setdropTarget, setEnabled, setfont, setForeground, setLocale, setLocation, setLocation, setName, setsize,  
setsize, setVisible, show, show, size, toString, transferFocus, update, validate

Vediamo quindi i TextField. Un TextField è in pratica una singola linea di testo editabile, con un aspetto grafico, è uno di quei campi che siamo abituati a vedere nelle Form che si trovano su internet per immettere nomi, cognomi, ecc.

Ho quattro possibili costruttori:

TextField(), crea una TextField vuota.  
TextField(int c), crea una TextField vuota di c colonne, ovvero una linea di c caratteri.  
TextField(String t), crea una TextField inizializzata con t.  
TextField(String t, int c), crea una TextField inizializzata con t, di c caratteri.

tra i vari metodi, quelli più interessanti per i nostri scopi, sono:

boolean echoCharIsSet(), dice se la TextField fa l’echo dei caratteri inseriti.  
int getColumns(), da il numero di colonne della TextField.  
char getEchoChar(), da il carattere usato per l’echo.  
void setColumns(int columns), setta il numero di colonne della TextField.  
void setEchoChar(char c), setta il carattere di echo per la TextField.  
void setText(String t), per settare il testo contenuto.

Per le dimensioni:

Dimension getMinimumsize()  
Dimension getMinimumsize(int c)  
Dimension getPreferredsize()  
Dimension getPreferredsize(int c)

La TextArea invece è un vero e proprio testo, non solo una riga, ma tante, comprende anche le due scrollbar per navigare sul testo se questo è più grande della finestra dove è visualizzato.

I costruttori sono:

TextArea(), costruisce una nuova TextArea.  
TextArea(int r, int c) , costruisce una nuova TextArea, di r righe e c colonne.  
TextArea(String text), costruisce una nuova TextArea inizializzata con il testo passato.  
TextArea(String text, int rows, int columns) , costruisce una nuova TextArea inizializzata con il  
testo passato, di r righe e c colonne.  
TextArea(String text, int rows, int columns, int scrollbars), come il precedente, solo che permette di  
specificare la politica di visualizazione delle scrollbar.

Vediamone alcuni metodi:

void append(String str), appende il testo passato alla TextArea. (nelle vecchie versioni di Java è  
appendText(String str)).  
int getColumns(), int getrows() per avere informazioni sul numero di colonne o righe.  
void insert(String str, int pos), inserisce il testo passato nella posizione passata. (nelle vecchie  
versioni di Java è  
insertText(String str)).  
void replaceRange(String str, int start, int end), rimpiazza il testo da start a end con la stringa passata.

void setColumns(int columns) e void setrows(int rows) per settare il numero di colonne o righe  
della TextArea.

Per le dimensioni dell’oggetto GUI:

Dimension getMinimumsize()  
Dimension getMinimumsize(int rows, int columns)  
Dimension getPreferredsize()  
Dimension getPreferredsize(int rows, int columns)

Abbiamo visto tutti i componenti di testo, a questo punto possiamo fare un’esempio che le usa ambedue.

Creiamo un’applet, quindi il launcher è:

<html>  
<head>  
<title>  
Esempio di uso dei componenti di testo.  
</title>  
</head>  
<body>  
<APPLET code=”Testo.class” width=500 height=200>  
<param name=testo value=”Testo iniziale per la TextArea.Inserire altri caratteri.”>  
</APPLET>  
</body>  
</html>

mentre il codice, da editare in Testo.java, è il seguente:

import java.awt.event.KeyListener;  
import java.awt.event.KeyEvent;  
import java.awt.TextField;  
import java.awt.TextArea;  
import java.awt.borderLayout;  
import java.applet.*;  
public class Testo extends Applet  
{  
TextField TF=new TextField(20);  
TextArea TA=new TextArea();  
public void init()  
{  
add(TA,borderLayout.CENTER);  
add(TF,borderLayout.SOUTH);  
String t=getParameter(“testo”);  
if (t!=null) TA.setText(t);  
TF.setEditable(false);  
TA.addKeyListener(new MyKeyListener());  
}  
public class MyKeyListener implements KeyListener  
{  
public void keyPressed(KeyEvent e)  
{  
}  
public void keyReleased(KeyEvent e)  
{  
int valore=e.getKeyCode();  
if (valore!=10) TF.setText(“Inserito carattere “+e.getKeyChar());  
else TF.setText(“Inserito carattere INVIO”);  
}  
public void keyTyped(KeyEvent e)  
{  
}  
}

}