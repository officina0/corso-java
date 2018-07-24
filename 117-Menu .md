Un menu in una applicazione non è altro che una MenuBar in cui vi sono vari menu.  
Si pensi ad un programma qualsiasi con le voci di menu File Edit ed Help, queste tre voci In Java sono degli oggetti della classe Menu, e vanno aggiunte ad un oggetto della classe MenuBar il quale va attaccato alla finestra. Ogni menu ha varie voci, ad esempio il menu File avrà le voci: Apri, Chiudi, Salva e Esci, questi in Java sono degli oggetti della classe MenuItem (o anche Menu se conterranno altri sottomenu).

![Voci menu](http://html.it/guide/img/guida_java/23a.gif)

  

Quindi se ad una applicazione vogliamo aggiungere un menù dobbiamo in qualche ordine fare le seguenti cose:

*   Creare gli oggetti MenuItem
*   Creare gli oggetti menu ed attaccarvi i MenuItem
*   Creare una MenuBar e attaccare i Menu

E poi bisogna al solito scrivere dei gestori per gli eventi provenienti dai menu ed associarli ai menu.  
Vediamo in pratica come si costruisce un menu, iniziamo dai MenuItem:  
Gli eventi dei MenuItem sono quelli che devono essere gestiti da noi, a differenza degli eventi dei menu che li gestisce il sistema, infatti mentre i secondi servono a fare apparire e scomparire le voci di menu, i primi sono i click sul comando corrispondente all’Item.  
Quindi per questi dovremo scrivere degli ActionListener, come per i bottoni, infatti essi non sono altro che dei bottoni speciali. I costruttori sono tre:

MenuItem() , che costruisce un MenuItem senza etichetta.  
MenuItem(String label), che costruisce un MenuItem con etichetta label.  
MenuItem(String label, MenuShortcut s), che costruisce un MenuItem con etichetta label e accelleratore (tasto di scelta rapida) definito in MenuShortcut s.

Alcuni metodi sono:

addActionListener(ActionListener l), associa un ActionListener al MenuItem per sentirne gli eventi di tipo ActionEvent (il click).  
void deleteShortcut(), cancella il tasto di scelta rapida per il menuitem.  
String getActionCommand(), da l’azione associata al MenuItem, l’azione è quella passata alll’actionListener del bottone per identificare il bottone stesso, infatti più item possono avere lo stesso gestore di eventi, il quale potrà distinguere il bottone cliccato in base al comandoo che gli arriva.  
String getLabel(), restituisce l’etichetta del MenuItem  
EventListener\[\]getListeners(Class listenerType) , restituisce tutti gli ascoltatori di eventi associati al MenuItem, deltipo listenerType.  
MenuShortcut getShortcut(), restituisce la definizione dell’acceleratore per il MenuItem.  
boolean isEnabled(), dice se il menu è abilitato o meno, se è disabilitato verrà visualizzato ingrigito.  
void removeActionListener(ActionListener l), elimina l’ascoltatore associato.  
void setActionCommand(String command), setta il comando associato al MenuItem, se non è specificato, il comando è l’etichetta del MenuItem.  
void setEnabled(boolean b), abilita e disabilita il MenuItem.  
void setLabel(String label), setta l’etichetta per il MenuItem.  
void setShortcut(MenuShortcut s), definisce l’acceleratore per il menu.

Quindi per creare gli Item del menu File descritto prima dovremo scrivere:

MenuItem apri=new MenuItem(“Apri”);  
MenuItem chiudi=new MenuItem(“Chiudi”);  
MenuItem salva=new MenuItem(“Salva”);  
MenuItem esci=new MenuItem(“Esci”);

Creiamo quindi l’oggetto Menu per attaccare i MenuItem.  
Due costruttori dell’oggetto sono:

Menu(), costruisce un Menu senza etichetta  
Menu(String label), costruisce un menu con etichetta label.

tra i metodi ci sono:

MenuItem add(MenuItem mi), che aggiunge un MenuItem al Menu.  
void add(String label), cheaggiunge un’etichetta al Menu (tipo separatore).  
void addSeparator(), aggiunge un vero e proprio separatore al menu, ovvero una linea.

Aggiungendo oggetti ad un menù in questo modo, essi verranno posizionati nello stesso ordine in cui vengono aggiunti.  
Per specificare invece la posizione in cui si vogliono inserire bisogna usare i metodi:

void insert(MenuItem menuitem, int index)  
void insert(String label, int index)  
void insertSeparator(int index)

I metodi MenuItem getItem(int index) e int getItemCount() servono per prendere un item data una posizione e per sapere il numero di MenuItem inseriti in un menu.

Per rimuovere gli Item dal menu invece si usano i metodi:

void remove(int index)  
void remove(MenuComponent item) e  
void removeAll()  
Quindi per creare il nostro menù File scriveremo:  
Menu file= new Menu(“File”);  
file.add(apri);  
file.add(salva);  
file.add(chiudi);  
file.addSeparator();  
file.add(esci);  

A questo punto supponendo di avere creato anche Edit ed Help, dobbiamo aggiungerli ad una MenuBar, la quale si crea usando il costruttore MenuBar().  
In questa ci sono i metodi remove e removeAll come per i menu, e c’è la add:

Menu add(Menu m);

Quindi la nostra MenuBar sarà creata nel seguente modo:

MenuBar barra=new MenuBar();  
barra.add(file);  
barra.add(edit);  
barra. setHelpMenu(help);

Quest’ultimo metodo è un metodo speciale per aggiungere un menu di aiuto.  
Infine non rimane che associare la barra di menu alla nostra finestra, questo è semplicissimo, perché la nostra finestra sarà una estensione di Frame, e in Frame c’è il metodo:

setMenuBar(MenuBar mb);

Nel nostro caso sarà setMenuBar (barra);  
ATTENZIONE: con awt non è possibile inserire menu in Dialog e in Applet (quelle che si vedono in giro sono dei finti menu, ovvero sono programmati disegnando dei menu, oppure non sono solo awt), mentre con swing si.  
Di seguito c’è il listato di una applicazione che definisce un menu e ne sente gli eventi:

import java.awt.*;  
import java.awt.event.*;  
public class Finestramenu extends Frame  
{  
MenuItem apri=new MenuItem(“Apri”);  
MenuItem chiudi=new MenuItem(“Chiudi”);  
MenuItem salva=new MenuItem(“Salva”);  
MenuItem esci=new MenuItem(“Esci”);  
MenuItem taglia=new MenuItem(“Taglia”);  
MenuItem copia=new MenuItem(“Copia”);  
MenuItem incolla=new MenuItem(“Incolla”);  
MenuItem cancella=new MenuItem(“Cancella”);  
MenuItem cerca=new MenuItem(“Cerca”);  
MenuItem rimpiazza=new MenuItem(“Rimpiazza”);  
MenuItem aiuto=new MenuItem(“Indice”);  
Menu file= new Menu(“File”);  
Menu edit= new Menu(“Edit”);  
Menu help= new Menu(“Help”);  
MenuBar barra = new MenuBar();  
Label risultato=new Label(“Nessuna voce di menu cliccata”);  
public Finestramenu()  
{  
setupEventi();  
file.add(apri);  
file.add(salva);  
file.add(chiudi);  
file.addSeparator();  
file.add(“Menu Uscita”);  
file.addSeparator();  
file.add(esci);  
edit.add(taglia);  
edit.add(copia);  
edit.add(incolla);  
edit.add(cancella);  
edit.addSeparator();  
edit.add(cerca);  
edit.add(rimpiazza);  
help.add(aiuto);  
barra.add(file);  
barra.add(edit);  
barra.setHelpMenu(help);  
setMenuBar(barra);  
add(risultato);  
pack();  
show();  
addWindowListener(new FinestramenuWindowListener());  
}  
void setupEventi()  
{  
apri.addActionListener(new AscoltatoreMenu());  
salva.addActionListener(new AscoltatoreMenu());  
chiudi.addActionListener(new AscoltatoreMenu());  
esci.addActionListener(new AscoltatoreMenu());  
taglia.addActionListener(new AscoltatoreMenu());  
copia.addActionListener(new AscoltatoreMenu());  
incolla.addActionListener(new AscoltatoreMenu());  
cancella.addActionListener(new AscoltatoreMenu());  
cerca.addActionListener(new AscoltatoreMenu());  
rimpiazza.addActionListener(new AscoltatoreMenu());  
aiuto.addActionListener(new AscoltatoreMenu());  
}  
public static void main(String\[\] arg)  
{  
new Finestramenu();  
}  
class AscoltatoreMenu implements ActionListener  
{  
public void actionPerformed (ActionEvent e)  
{  
risultato.setText(” Cliccato “+e.getActionCommand());  
if (e.getActionCommand().compareTo(“Esci”)==0) System.exit(0);  
}  
}  
class FinestramenuWindowListener implements WindowListener  
{  
public void windowActivated(WindowEvent e)  
{  
System.out.println(“Sentito un Window Activated”);  
}  
public void windowClosed(WindowEvent e)  
{  
System.out.println(“Sentito un Window Closed”);  
}  
public void windowClosing(WindowEvent e)  
{  
System.out.println(“Sentito un Window Closing”);  
System.exit(0);  
}  
public void windowDeactivated(WindowEvent e)  
{  
System.out.println(“Sentito un Window Deactivaded”);  
}  
public void windowDeiconified(WindowEvent e)  
{  
System.out.println(“Sentito un Window Deiconified”);  
}  
public void windowIconified(WindowEvent e)  
{  
System.out.println(“Sentito un Window Iconified”);  
}  
public void windowOpened(WindowEvent e)  
{  
System.out.println(“Sentito un Window Opened”);  
}  
}  
}  

Come si vede, nell’esempio c’è un altro ascoltatore di eventi. Questo sente eventi di tipo WindowEvent, e serve per ascoltare eventi della finestra.  
Per associarlo alla finestra si usa il metodo addWindowListener(WindowListener l); dove WindowListener è l’ascoltatore di eventi. Per definire un ascoltatore di eventi bisogna quindi implementare l’interfaccia WindowListener e ridefinirne tutti i metodi:

class MIOASCOLTATORE implements WindowListener

I metodi da ridefinire sono:

public void windowActivated(WindowEvent e)  
public void windowClosed(WindowEvent e)  
public void windowClosing(WindowEvent e)  
public void windowDeactivated(WindowEvent e)  
public void windowDeiconified(WindowEvent e)  
public void windowIconified(WindowEvent e)  
public void windowOpened(WindowEvent e)

Bisogna ridefinirli tutti.  
Se si manda in esecuzione l’esempio si vede, grazie alle System.out.println messe nei metodi ridefiniti, a quale tipo di evento sulla finestra sono associati i vari metodi, come ad esempio windowClosed è invocato quando si preme la X della finestra.

Per esercizio provare a scrivere dei WindowListener per ascoltare l’evento chiudi Finestra per tutti i Frame definiti negli esempi di questo capitolo (li ho lasciati volutamente senza).

I menu visti non sono gli unici menu implementabili in Java, un altro tipo di menù è il menu di tipo Popup, ovvero quel menu che si associa ad un componente o ad una posizione. Un esempio di menu popup è il menu che viene fuori sul desktop del sistema operativo Windows quando si preme il bottone destro del mouse (Figura sotto).

![menu popup](http://html.it/guide/img/guida_java/23b.gif)

  

Per creare un menu popup bisogna creare un oggetto appartenente alla classe PopupMenu, usando uno dei cosrtuttori: PopupMenu(), PopupMenu(String label), che creano rispettivamente un menu senza e con etichetta.

PopupMenu è una estensione di Menu, quindi ne eredita i metodi e anche quelli per aggiungere ed eliminare dei MenuItem. tra i suoi metodi invece ce n’è uno per visualizzare il menu in una data posizione dello schermo rispetto ad un dato componente: show(Component origin, int x, int y).