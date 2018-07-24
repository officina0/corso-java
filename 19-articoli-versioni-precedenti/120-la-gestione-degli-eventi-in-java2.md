# 120 La Gestione Degli Eventi In Java 2

Abbiamo visto quesi tutti i componenti GUI di awt, per costruire una interfaccia basta combinare queste e ascoltarne gli eventi.  
Per ogni GUI vista abbiamo visto come gestire alcuni eventi, vediamo adesso quali altri eventi possono essere ascoltati.  
Innanzitutto abbiamo ascoltato degli eventi di tipo ActionEvent, ovvero eventi che consistono nel compiere un’azione sulle gui da parte dell’utente, tipo premere un bottone.  
Per fare questo, ricordo che abbiamo implementato l’interfaccia ActionListener, e ne abbiamo ridefinito l’unico evento actionPerformed \(ActionEvent e\), che viene invocato quando viene esequita una azione sul componente GUI \(o sui componenti\) a cui è associato.  
Per associare al GUI l’ascoltatore di eventi che abbiamo definito abbiamousato il metodo addActionListener \(ActionListener ascoltatore\) del GUI.  
Pe usarli bisogna includere nel programma:

import java.awt.event.ActionListener;  
import java.awt.event.ActionEvent;

oppure

impore java.awt.event.\*;

Ascoltatori di eventi di tipo ActionListener possono essere associati agli oggetti GUI di awt:

Button

List

\(quando si seleziona o deseleziona un elemento\)

MenuItem  
TextField

Abbiamo visto anche come implementare un ItemListener, per ascoltare eventi provenienti da GUI che hanno uno stato di on/off, quando cambiano il proprio stato.  
Per associarlo ad un GUI abbiamo usato addItemListener\(ItemListener ascoltatore\) del GUI, quando l’evento è generato dal GUI, viene invocato il metodo itemStateChanged\(ItemEvent e\) dell’ascoltatore associato.

Da includere:

import java.awt.event.\*;

oppure

import java.awt.event.ItemListener;  
import java.awt.event.ItemEvent;

I GUI awt per cui può essere ascoltato l’evento sono:

## Checkbox

CheckboxMenuItem

\(non lo abbiamo visto, esso è come un MenuItem, solo che funziona come un Checkbox, ovvero può essere cliccato o meno, se associato ad un CheckboxGroup, diviene ancora una volta un radiobutton, solo che su un menu\).

Choice

List

Abbiamo anche visto gli ascoltatori degli eventi delle finestre, ovvero di tipo WindowListener.  
Essi vengono associati alle finestre usando il metodo di queste addWindowListener\(WindowListener WL\), e per definirle bisogna implementare l’interfaccia WindowListener e ridefinirne i metodi:

void windowActivated\(WindowEvent e\), invocato quando la finestra diviene attiva, ovvero viene in primo piano sul desktop.  
void windowClosed\(WindowEvent e\), invocato quando la finestra si è chiusa.  
void windowClosing\(WindowEvent e\), invocato quando si preme il bottone chiudiFinestra \(X\).  
void windowDeactivated\(WindowEvent e\), invocato quando la finestra passa in secondo piano.  
void windowDeiconified\(WindowEvent e\), invocato quando la finestra passa da icona ad attiva.  
void windowIconified\(WindowEvent e\), quando viene ridotta ad icona.  
void windowOpened\(WindowEvent e\), quando viene aperta.

Per sentire gli eventi della finestra bisogna includere:

import java.awt.event.WindowListener;  
import java.awt.event.WindowEvent;

oppure

import java.awt.event.\*;

Gli eventi delle finestre avvengono sempre più o meno a coppie, ad esempio quando si preme la X in alto a destra della finestre, prima viene invocato windowClosing e poi windowClosed, oppure quando una finestra passa da icona ad attiva prima sente windowDeiconified e poi WindowActivated, eccetera.  
E’ possibile sentire eventi di tipo finestra per gli elementi:

Window  
Frame  
Dialog

Sulle Dialog apro una piccola parentesi:  
Sono dei Frame senza icona , riduci a icona e massimizza, sonomolto utili per fare appunto dei dialoghi veloci con l’utente, sono ad esempio delle dialog le finestre che appaiono nelle applicazioni e dicono Sei sicuro di volere uscire?  
Una Dialog può essere modale o non modale, la prima è una Dialog che blocca completamente l’applicazione, la quale non sente più gli eventi, la seconda invace va in parallelo a questa.  
Le dialog di awt non possono contenere dei menu, mentre quelle di swing \(javax.swing.JDialog\) si.

Vediamo quali eventi possono essere ascoltati da oggetti di tipo TextComponent, ovvero oggetti di tipo TextField e TextArea. Possiamo ascoltare eventi di tipo TextEvent, implementando l’interfaccia TextListener, e ridefinendone il metodo textValueChanged\(TextEvent e\), invocato ogni volta che il testo viene modificato.  
L’interfaccia e l’evento si trovano in java.awt.event

Per oggetti di tipo container, ovvero per Panel, ScrollPane, Window \(e quindi Dialog e Frame\), posso ascoltare gli eventi sul contenitore, usando il metodo addContainerListener\(ContainerListener l\). Bisogna implementare l’interfaccia ContainerListener e ridefinire i metodi:

void componentAdded\(ContainerEvent e\)  
void componentremoved\(ContainerEvent e\)

che vengono automaticamente invocati quando vengono aggiunti o tolti elementi al contenitore.  
Bisogna includere java.awt.event.ContainerListener e java.awt.event.ContainerEvent.

A questo punto vediamo quali eventi si possono ascoltare su oggetti di tipo Component, ovvero su TUTTI i componenti awt, compresi i contenitori e le finestre.

In Component troviamo i metodi:

addComponentListener\(ComponentListener l\)  
addFocusListener\(FocusListener l\)  
addHierarchyBoundsListener\(HierarchyBoundsListener l\)  
addHierarchyListener\(HierarchyListener l\)  
addInputMethodListener\(InputMethodListener l\)  
addKeyListener\(KeyListener l\)  
addMouseListener\(MouseListener l\)  
addMouseMotionListener\(MouseMotionListener l\)  
addPropertyChangeListener\(PropertyChangeListener listener\)  
addPropertyChangeListener\(String propertyName, PropertyChangeListener listener\)

Il ComponentListener ascolta eventi sul componente, si devono ridefinire i metodi:

void componentHidden\(ComponentEvent e\)  
void componentMoved\(ComponentEvent e\)  
void componentresized\(ComponentEvent e\)  
void componentShown\(ComponentEvent e\)

che vengono invocati quando il componente viene nascosto, mosso, cambiato di dimensioni e visualizzato.

FocusListener ascolta eventi di focus della tastiera sul componente, ovvero se si seleziona o meno il componente con la tastiera.  
Bisogna ridefinirne i metodi:

focusGained\(FocusEvent e\)

, chiamato quando il componente è selezionato.

focusLost\(FocusEvent e\)

, chiamato dal componente quando perde la selezione della tastiera.

Si pensi ad esempio alla navigazione tra varie form di una interfaccia premendo il tasto TAB.

HierarchyBoundsListener, sente eventi di spostamento degli antenati dal componente, ad esempio un bottone su una finestra, se implementa questa interfaccia sente se la finestra si muove.  
Bisogna ridefinire i metodi:

void ancestorMoved\(HierarchyEvent e\)

, chiamata quando l’antenato viene mosso.

void ancestorResized\(HierarchyEvent e\)

, chiamata quando l’antenato viene ambiato di dimensione.

HierarchyListener ascolta i cambiamenti degli antenati del componente, bisogna ridefinire il metodo hierarchyChanged\(HierarchyEvent e\), sarà HierarchyEvent a contenere il cambiamento fatto.

KeyListener lo abbiamo gia visto, ascolta eventi provenienti dalla tastiera, dobbiamo ridefinire i metodi

void keyPressed\(KeyEvent e\)

, invocato quando un tasto viene premuto.

void keyReleased\(KeyEvent e\)

, invocato quando un tasto viene lasciato.

void keyTyped\(KeyEvent e\)

, invocato quando un tasto si tiene premuto.  
In pratica quando si preme un tasto vengono sentiti gli eventi nell’ordine: keyPressed, keyTyped e keyReleased.

MouseListener ascolta gli eventi provenienti dal mouse, bisogna ridefinire i metodi:

void mouseClicked\(MouseEvent e\), invocato quando viene cliccato un bottone del muose sul componente.  
void mouseEntered\(MouseEvent e\), invocato quando il mouse entra nel componente.  
void mouseExited\(MouseEvent e\), invocato quando il mouse esce dal componente.  
void mousePressed\(MouseEvent e\), invocato quando il mouse si tiene premuto sul componente.  
void mouseReleased\(MouseEvent e\), invocato quando il mouse viene rilasciato.

MouseMotionListener, altri eventi del mouse sul componente:

mouseDragged\(MouseEvent e\)

, quando il mouse viene premuto e mosso sul componente.

void mouseMoved\(MouseEvent e\)

, invocato quando il mouse si muove sul componente, con o senza bottoni premuti.

L’ultima interfaccia per eventi del mouse è MouseInputListener, che implementa tutte e due le interfacce precedenti, quindi ascolta tutti gli eventi precedenti del mouse.

Nell’esempio successivo vengono ascoltati vari eventi, viene anche usata una Dialog.

import java.awt.event._;  
import java.awt._;  
class Ascolta extends Frame  
{  
Button b1=new Button\(“Bottone1”\);  
Button b2=new Button\(“Visualizza Dialog”\);  
Button b3=new Button\(“Esci”\);  
Button b4=new Button\(“Chiudi Dialog”\);  
Choice l1=new Choice\(\);  
TextField TF1=new TextField\(15\);  
Dialog D;  
AscoltaActionListener aal;  
AscoltaItemListener ail;  
AscoltaWindowListener awl;  
AscoltaWindowListenerDialog awld;  
AscoltaMouseListener aml;  
AscoltaKeyListener akl;  
public Ascolta\(\)  
{  
setTitle\(“Frame, esempio ascoltatori eventi”\);  
setLocation\(100,100\);  
setLayout\(new GridLayout\(5,1\)\);  
add\(b1\);  
add\(b2\);  
add\(b3\);  
add\(l1\);  
add\(TF1\);  
D=new Dialog\(this,true\);  
D.setsize\(200,100\);  
D.setLocation\(200,100\);  
D.add\(b4,borderLayout.SOUTH\);  
D.setTitle\(“Dialog – modale”\);  
preparaD\(false\);  
l1.add\(“Scelta 1”\);  
l1.add\(“Scelta 2”\);  
l1.add\(“Scelta 3”\);  
// Inizializzo gli ascoltatori degli eventi.  
aal=new AscoltaActionListener\(\);  
ail=new AscoltaItemListener\(\);  
awl=new AscoltaWindowListener\(\);  
awld=new AscoltaWindowListenerDialog\(\);  
aml=new AscoltaMouseListener\(\);  
akl=new AscoltaKeyListener\(\);  
b1.addActionListener\(aal\);  
b2.addActionListener\(aal\);  
b3.addActionListener\(aal\);  
TF1.addActionListener\(aal\);  
l1.addItemListener\(ail\);  
addWindowListener\(awl\);  
addMouseListener\(aml\);  
b1.addMouseListener\(aml\);  
b2.addMouseListener\(aml\);  
b3.addMouseListener\(aml\);  
b4.addMouseListener\(aml\);  
l1.addMouseListener\(aml\);  
TF1.addMouseListener\(aml\);  
TF1.addTextListener\(new TextListener\(\)  
{  
public void textValueChanged\(TextEvent e\)  
{  
System.out.println\(“TextListener: cambiato valore della TextField”\);  
}  
}  
\);  
D.addMouseListener\(aml\);  
addKeyListener\(akl\);  
b1.addKeyListener\(akl\);  
b2.addKeyListener\(akl\);  
b3.addKeyListener\(akl\);  
b4.addKeyListener\(akl\);  
l1.addKeyListener\(akl\);  
TF1.addKeyListener\(akl\);  
D.addKeyListener\(akl\);  
pack\(\);  
show\(\);  
}  
void preparaD\(boolean m\)  
{  
D.setModal\(m\);  
}  
public static void main \(String \[\] s\)  
{  
System.out.println\(“Ascolto gli eventi.”\);  
new Ascolta\(\);  
}  
// ActionListener  
class AscoltaActionListener implements ActionListener  
{  
public void actionPerformed\(ActionEvent e\)  
{  
String c=e.getActionCommand\(\);  
if \(c.compareTo\(“Bottone1”\)==0\) System.out.println\(“ActionListener: Premuto Bottone1”\);  
else  
if \(c.compareTo\(“Esci”\)==0\)  
{  
System.out.println\(“ActionListener: Premuto Esci”\);  
System.exit\(9\); // è un numero qualsiasi, si chiama exit code.  
} else  
if \(c.compareTo\(“Visualizza Dialog”\)==0\)  
{  
System.out.println\(“ActionListener: Premuto Visualizza Dialog”\);  
D.show\(\);  
b4.addActionListener\(aal\);  
b2.removeActionListener\(aal\);  
D.addWindowListener\(awld\);  
} else  
if \(c.compareTo\(“Chiudi Dialog”\)==0\)  
{  
System.out.println\(“ActionListener: Premuto Visualizza Dialog”\);  
D.hide\(\);  
b4.removeActionListener\(aal\);  
b2.addActionListener\(aal\);  
D.removeWindowListener\(awld\);  
} else  
{  
System.out.println\(“ActionListener: Premuto Invio nella TextField, letto:”+TF1.getText\(\)\);  
TF1.setText\(“”\);  
}  
}  
}  
// ItemListener  
class AscoltaItemListener implements ItemListener  
{  
public void itemStateChanged\(ItemEvent e\)  
{  
String val=l1.getSelectedItem\(\);  
System.out.println\(“ItemListener: Selezionato “+val\);  
}  
}  
// WindowListener  
public class AscoltaWindowListener implements WindowListener  
{  
public void windowActivated\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra attivata”\);  
}  
public void windowClosed\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra chiusa”\);  
}  
public void windowClosing\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Premuto chiudi finestra”\);  
System.exit\(0\);  
}  
public void windowDeactivated\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra disattivata”\);  
}  
public void windowDeiconified\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra deiconificata”\);  
}  
public void windowIconified\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra riduci a icona”\);  
}  
public void windowOpened\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra aperta”\);  
}  
}  
public class AscoltaWindowListenerDialog implements WindowListener  
{  
public void windowActivated\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Dialog attivata”\);  
}  
public void windowClosed\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Dialog chiusa”\);  
}  
public void windowClosing\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Premuto chiudi Dialog”\);  
D.hide\(\);  
b4.removeActionListener\(aal\);  
b2.addActionListener\(aal\);  
D.removeWindowListener\(awld\);  
}  
public void windowDeactivated\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Finestra disattivata”\);  
}  
public void windowDeiconified\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Dialog deiconificata”\);  
}  
public void windowIconified\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Dialog riduci a icona”\);  
}  
public void windowOpened\(WindowEvent e\)  
{  
System.out.println\(“WindowListener: Dialog aperta”\);  
}  
}  
// Mouse Listener:  
class AscoltaMouseListener implements MouseListener  
{  
public void mouseClicked\(MouseEvent e\)  
{  
Component a=e.getComponent\(\);  
String n=a.getName\(\);  
System.out.println\(“MouseListener: Mouse cliccato sul componente “+n\);  
}  
public void mouseEntered\(MouseEvent e\)  
{  
Component a=e.getComponent\(\);  
String n=a.getName\(\);  
System.out.println\(“MouseListener: Mouse entrato nel componente “+n\);  
}  
public void mouseExited\(MouseEvent e\)  
{  
Component a=e.getComponent\(\);  
String n=a.getName\(\);  
System.out.println\(“MouseListener: Mouse uscito dal componente “+n\);  
}  
public void mousePressed\(MouseEvent e\)  
{  
Component a=e.getComponent\(\);  
String n=a.getName\(\);  
System.out.println\(“MouseListener: il bottone e’ schiacciato su coponente “+n\);  
}  
public void mouseReleased\(MouseEvent e\)  
{  
Component a=e.getComponent\(\);  
String n=a.getName\(\);  
System.out.println\(“MouseListener: Mouse rilasciato sul componente “+n\);  
}  
}  
// Tastiera  
class AscoltaKeyListener implements KeyListener  
{  
public void keyPressed\(KeyEvent e\)  
{  
System.out.println\(“KeyListener: Premuto tasto “+e.getKeyChar\(\)+  
” codice “+  
e.getKeyCode\(\)\);  
}  
public void keyReleased\(KeyEvent e\)  
{  
System.out.println\(“KeyListener: Rilasciato tasto “+e.getKeyChar\(\)+  
” codice “+  
e.getKeyCode\(\)\);  
}  
public void keyTyped\(KeyEvent e\)  
{  
System.out.println\(“KeyListener: Stai premendo il tasto “+e.getKeyChar\(\)+  
” codice “+  
e.getKeyCode\(\)\);  
}  
}  
}

