# 121 Introduzione A Swing

Oltre al package java.awt, Java mette a disposizione del programmatore il package javax.swing per creare delle interfacce grafiche. In questo capitolo vedremo perché la Sun Microsystem ha creato quest’altro package e quali sono le differenze con awt. Per presentare il package parlerò del programma che sto facendo per la mia Tesi in Informatica all’Università di Pisa. Per informazioni riguardo ad Orespics potete contattare me, oppure le Prof.sse Laganà e Ricci presso il dipartimento di Informatica di Pisa \(Corso Italia, 40\). Il nome del programma è Orespics Programming Language, ed in pratica è un ambiente integrato in cui è possibile programmare agenti paralleli che comunicano tra di loro scambiandosi dei messaggi. L’ambiente permette di definire ed eseguire questi agenti, esso si propone come strumento didattico di supporto all’apprendimento delle modalità di programmazione parallela, programmazione che al programmatore abituato a programmare in modo sequenziale può risultare molto difficoltosa. Proprio perché lo strumento è uno strumento didattico, ha una interfaccia utente User Friendly. L’interfaccia è stata scritta usando il package Swing di Java.  
Swing è stato interamente scrittto in Java, usando il package awt, e mette a disposizione all’utente tante classi presenti anche in awt, ma notevolmente migliorate e potenziate, mette inoltre tante altre classi non presenti in awt.

Vediamo quindi quali sono le classi e le interfacce contenute nel pacchetto:

## Interfacce

Action  
BoundedRangeModel  
ButtonModel  
CellEditor  
ComboBoxEditor  
ComboBoxModel  
DesktopManager  
Icon  
JComboBox.KeySelectionManager  
ListCellRenderer  
ListModel  
ListSelectionModel  
MenuElement  
MutableComboBoxModel  
Renderer  
RootPaneContainer  
Scrollable  
ScrollPaneConstants  
SingleSelectionModel  
SwingConstants  
UIDefaults.ActiveValue  
UIDefaults.LazyValue  
WindowConstants

## Classi

AbstractAction  
AbstractButton  
AbstractCellEditor  
AbstractListModel  
ActionMap  
borderFactory  
Box  
Box.Filler  
BoxLayout  
ButtonGroup  
CellRendererPane  
ComponentInputMap  
DebugGraphics  
DefaultBoundedRangeModel  
DefaultButtonModel  
DefaultCellEditor  
DefaultComboBoxModel  
DefaultdesktopManager  
DefaultFocusManager  
DefaultListCellRenderer  
DefaultListCellRenderer.UIResource  
DefaultListModel  
DefaultListSelectionModel  
DefaultSingleSelectionModel  
FocusManager  
GrayFilter  
ImageIcon  
InputMap  
InputVerifier  
JApplet  
JButton  
JCheckBox  
JCheckBoxMenuItem  
JColorChooser  
JComboBox  
JComponent  
JDesktopPane  
JDialog  
JEditorPane  
JFileChooser  
JFrame  
JInternalFrame  
JInternalFrame.JDesktopIcon  
JLabel  
JLayeredPane  
JList  
JMenu  
JMenuBar  
JMenuItem  
JOptionPane  
JPanel  
JPasswordField  
JPopupMenu  
JPopupMenu.Separator  
JProgressBar  
JRadioButton  
JRadioButtonMenuItem  
JRootPane  
JScrollBar  
JScrollPane  
JSeparator  
JSlider  
JSplitPane  
JTabbedPane  
Jtable  
JTextArea  
JTextField  
JTextPane  
JToggleButton  
JToggleButton.ToggleButtonModel  
JToolBar  
JToolBar.Separator  
JToolTip  
Jtree  
Jtree.DynamicUtiltreeNode  
Jtree.EmptySelectionModel  
JViewport  
JWindow  
KeyStroke  
LookAndFeel  
MenuSelectionManager  
OverlayLayout  
ProgressMonitor  
ProgressMonitorInputStream  
RepaintManager  
ScrollPaneLayout  
ScrollPaneLayout.UIResource  
sizeRequirements  
sizeSequence  
SwingUtilities  
Timer  
ToolTipManager  
UIDefaults  
UIDefaults.LazyInputMap  
UIDefaults.ProxyLazyValue  
UIManager  
UIManager.LookAndFeelInfo  
ViewportLayout

## Eccezioni

UnsupportedLookAndFeelException

Notiamo in numero di classi molto superiore di quelle di awt, inoltre notiamo che ci sono tante classi che hanno lo stesso nome di alcune classi di awt, tranne che per una J iniziale, come ad esempio JFrame, JDialog, eccetera, queste classi hanno lo stesso funzionamento delle analoghe classi di awt, tranne che per il fatto che contengono tanti metodi utili in più.  
Presentiamo alcune delle cose in più in swing rispetto ad awt.  
In swing ci sono le Toolbar, ovvero delle piccole barre su cui ci vanno dei bottoni, esse possono essere spostate all’interno della finestra che le contiene.  
I bottoni di swing, come quasi tutti gli altri JComponent hanno la possibilità olre che di avere un’etichetta anche una immagine, immagine che può cambiare a seconda dello stato del bottone.  
Inoltre in swing possono essere usati i Tooltip, ovvero quei suggerimenti che escono automaticamente su un componente quando il mouse vi ci si sofferma per un po di tempo.  
Vediamo l’effetto di questi componenti swing in Orespics.

![effetto dei componenti swing in Orespics](http://html.it/guide/img/guida_java/27a.gif)

Nella Figura possiamo notare la Toolbar \(in cui ho nascosto i bordi\) di altre 8 Toolbar, una per ogni categoria di bottoni. Si vedono i bottoni che contengono delle immagini, alcune attive e altre inattive.

![Figura toolbar 1](http://html.it/guide/img/guida_java/27b.gif)

Nella Figura si può vedere una Tooltip su uno dei bottoni.

![Figura toolbar 2](http://html.it/guide/img/guida_java/27c.gif)

Nella Figura possiamo notare che anche nei menu possono essere inserite delle immagini, rendendoli molto gradevoli all’utente.

![](http://html.it/guide/img/guida_java/27d.gif)

Le toolbar possono essere facilmente nascoste e visualizzate sullo schermo.

![Figura toolbar 3](http://html.it/guide/img/guida_java/27e.gif)

Nella Figura notiamo il cambiamento dell’immagine del bottone con il pinguino, che diventa più grande, con swing, come già detto è possibile cambiare l’icona a seconda dello stato del bottone.

![Figura 4 ](http://html.it/guide/img/guida_java/27f.gif)

Altri due componenti molto belli presenti in Swing e non in awt sono i TabbedPane e gli Alberi.  
I primi sono i controlli a schede tipici delle interfacce di Windows, molto gradevoli, e i secondi fanno vedere delle strutture ad albero, tipo qelle che si vedono navigando sugli hard disk usando esplora di Windows.

Con swing si può anche cambiare il cosiddetto Look and Feel della finestra, ovvero il suo aspetto, rendendola simile ad una interfaccia tipica di Java, di Windows, di Macintosh \(se si è su una macchina Mac\) e di Linux \(X-Windows – Motif\).

Con Swing cambia completamente la gestione del testo, infatti si possono creare testi colorati, con vari stili di carattere.

Tante altre sono le possibilità di Swing, come ad esempio delle Dialog per navigare sui file con delle preview e delle dialog per la scelta dei colori, già implementate dal pacchetto.

Molto interessante è secondo me sapere che la dialog di scelta dei colori della Figura 16 viene creata e usata con una singola istruzione, ovvero basta invocare il metodo:

static JDialog  
createDialog\(Component c, String title,  
boolean modal, JColorChooser chooserPane,  
ActionListener okListener,  
ActionListener cancelListener\)

della classe JColorChooser.

Comunque credo di avere fatto capire quali sono le potenzialità del pacchetto, pacchetto che però ancora non è stato incluso nelle versioni di Java dei browser, e che quindi non è ancora possibile creare delle applet swing senza caricare plugins nei sopracitati browser.

Nei testi di swing è possibile inserire anche immagini, e inoltre si possono leggere e decodificare direttamente file di tipo html, rtf \(Un cugino del .doc\).

In Swing c’è la classe Jdialog, analoga alla Applet di java.applet, essa ne è una estensione, e presenta, oltre ai metodi della class Applet \(di cui è una estensione\), altri metodi molto interessanti, tra cui il metodo void setJMenuBar\(JMenuBar menuBar\).

Ovviamente siccome ci sono tanti GUI in più in swing, c’è la necessità di gestirne gli eventi, esiste quindi il sottopacchetto javax.swing.event che estende gli eventi di java.awt.event per gestire gli eventi dei componenti di swing.

