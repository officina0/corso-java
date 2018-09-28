Spesso nelle interfacce dei nostri Applet ci capita di dover fare effettuare all’utente una o più scelte tra varie possibilità. Per fare questo dobbiamo elencare le possibilità e poi leggere la scelta.  
Per fare questo possiamo usare le liste, che rapresentano una serie di oggetti e danno la possibilità di sceglierne alcuni.  
Pensiamo ad esempio ad un’ applet che gestisce le prenotazioni di visite guidate ad alcune località, e che faccia scegliere tra tutte quelle che si vogliono visitare.  
Oltre alle solite form per il Nome, Cognome ecc.. avremo la lista delle località, vediamo come possiamo implementarla.  
Per costruire la lista useremo uno dei tre costruttori:

List() , crea una nuova lista  
List(int rows), crea una nuova lista con rows linee  
List(int rows, boolean multipleMode), crea una nuova lista

con rows linee, e le dice che a seconda  
del valore booleano multipleMode si potranno scegliere uno o più elementi della lista.

Nel nostro caso useremo la terza forma di costruttore.

List lista=new List(0,true);

Alcuni metodi che possiamo usare sulle liste sono i seguenti:

void add(String item) e void add(String item, int index), per appendere alla fine della lista, oppure  
in una data posizione un nuovo componente.

void addActionListener(ActionListener l), per associare un ActionListener alla lista.  
void addItemListener(ItemListener l), per associare un ItemListener alla lista.  
String getItem(int index), per avere l’elemento in posizione indicata.  
int getItemCount() , per avere il numero di elementi.  
String[] getItems(), per avere tutti gli elementi.  
EventListener[] getListeners(Class listenerType) , per avere gli ascoltatori associati alla lista del  
tipo voluto.  
Dimension getMinimumsize(), da la dimensione minima della lista.  
Dimension getPreferredsize(), da la dimensione preferita per la lista.  
int getrows() , dice il numero di linee attualmente visibili nella lista.  
Int getSelectedIndex(), da l’indice dell’elemento selezionato.  
int[] getSelectedIndexes(), da gli indici degli elementi selezionati.  
String getSelectedItem(), da l’oggetto selezionato.  
String[]getSelectedItems(), da gli oggetti selezionati.  
Object[] getSelectedObjects(), da la lista degli elementi selezionati in un vettore di oggetti.  
boolean isIndexSelected(int index), dice se l’indice è selezionato.  
boolean isMultipleMode(), dice se nella lista si possono selezionare più elementi.  
void makeVisible(int index), visualizza l’item in posizione indicata.  
void remove(int position), void remove(String item), eliminano gli elementi indicati dall’indice o  
dall’etichetta.  
void removeAll(), elimina tutti gli elementi della lista.  
void removeActionListener(ActionListener l) e void removeItemListener(ItemListener l), eliminano  
gli ascoltatori di eventi definiti per la lista.  
void replaceItem(String newValue, int index), modifica l’elemento specificato.  
void select(int index), seleziona l’elemento indicato nella lista.  
void setMultipleMode(boolean b), dice alla lista se è possibile selezionare solo un’elemento o più di  
uno.

Vediamo l’esempio sull’uso delle liste.

import java.awt.*;  
import java.awt.event.*;  
public class liste extends Frame  
{  
List lista=new List(0,true);  
Label text=new Label(“Bellezze che possono essere visitate nella località scelta”);

public liste()  
{  
super(“Scelta itinerario”);  
lista.add(“Benevento”);  
lista.add(“Foiano di Val Fortore”);  
lista.add(“Baselice”);  
lista.add(“San Bartolomeo in Galdo”);  
lista.add(“San Marco dei Cavoti”);  
lista.add(“Montefalcone in Val Fortore”);  
lista.add(“Pesco Sannita”);  
lista.add(“Colle Sannita”);  
lista.add(“Castelvetere in Val Fortore”);  
lista.add(“Castelfranco in Miscano”);  
lista.add(“Ginestra degli Schiavoni”);  
lista.add(“San Giorgio la Molara”);  
lista.add(“Molinara”);  
lista.add(“Pietrelcina”);  
lista.add(“Fragneto Monforte”);  
lista.add(“Circello”);  
lista.add(“Campolattaro”);  
add(lista,borderLayout.CENTER);  
add(text,borderLayout.SOUTH);  
addWindowListener(new listeWindowListener());  
lista.addItemListener(new ascoltaLista());  
setsize(350,100);  
setresizable(false);  
show();  
}  
public static void main(String [] arg)  
{  
new liste();  
}  
class listeWindowListener implements WindowListener  
{  
public void windowActivated(WindowEvent e)  
{}  
public void windowClosed(WindowEvent e)  
{}  
public void windowClosing(WindowEvent e)  
{  
String[] s=lista.getSelectedItems();  
int i=0;  
System.out.println(“Itinerario selezionato”);  
try  
{  
while (true)  
{  
System.out.println(s[i++]);  
}  
}  
catch (ArrayIndexOutOfBoundsException er)  
{System.out.println(“Buon divertimento…”);}  
System.exit(0);  
}  
public void windowDeactivated(WindowEvent e)  
{}  
public void windowDeiconified(WindowEvent e)  
{}  
public void windowIconified(WindowEvent e)  
{}  
public void windowOpened(WindowEvent e)  
{}  
}  
class ascoltaLista implements ItemListener  
{  
public void itemStateChanged(ItemEvent e)  
{  
int indice=((Integer) e.getItem()).intValue();  
if (indice==0) text.setText(“Rocca dei Rettori, arco di traiano, anfiteatro Romano, città spettacolo”);  
if (indice==1) text.setText(“località San Giovanni, Campanile, via Roma, lago, festa S.Giovanni, festa dell’emigrante”);  
if (indice==2) text.setText(“oasi di San Leonardo”);  
if (indice==3) text.setText(“centro storico”);  
if (indice==4) text.setText(“centro storico”);  
if (indice==5) text.setText(“centro storico”);  
if (indice==6) text.setText(“centro storico”);  
if (indice==7) text.setText(“centro storico”);  
if (indice==8) text.setText(“centro storico”);  
if (indice==9) text.setText(“Bosco”);  
if (indice==10) text.setText(“centro storico”);  
if (indice==11) text.setText(“Lago di San Giorgio”);  
if (indice==12) text.setText(“centro storico”);  
if (indice==13) text.setText(“Piana Romana, centro storico, case di Padre Pio”);  
if (indice==14) text.setText(“Raduno internazionale di mongolfiere, palazzo Ducale”);  
if (indice==15) text.setText(“centro storico”);  
if (indice==16) text.setText(“Diga di Campolattaro”);  
}  
}  
}

Le liste sono quindi un buon GUI per fare effettuare all’utente una scelta fra varie possibilità.  
Vi è un altro GUI che serve a fare questo, che in pratica è una lista a scomparsa, nel senso che è visibile solo l’oggetto selezionato, e che si vedono gli altri solo quando si deve fare la scelta, tipo menù a scomparsa, mentre nelle liste sono sempre visibili.  
Il GUI di cui sto parlando è il Choice (scelta), il cui funzionamento è simile a quello delle liste.  
Il costruttore è uno, senza parametri, mentre i metodi sono:

void add(String item), aggiunge una possibilità in fondo alle scelte possibili.  
void addItem(String item), come la add  
void addItemListener(ItemListener l), associa un ascoltatore di eventi per la Choice.  
String getItem(int index), restituisce la scelta possibile che si trova all’indice i.  
int getItemCount(), da’ il numero di possibilità per la Choice.  
EventListener[] getListeners(Class listenerType), da tutti gli ascoltatori associati alla Choice.  
int getSelectedIndex(), restituisce l’indice della scelta selezionata nella Choice.  
String getSelectedItem(), restituisce la scelta selezionata nella Choice.  
Object[] getSelectedObjects(), da tutti gli item selezionati.  
void insert(String item, int index), inserisce una scelta nella posizione indicata della Choice.

void remove(int position), void remove(String item), void removeAll() , per rimuovere le scelte  
possibili dalla lista di scelte.

void removeItemListener(ItemListener l), elimina l’ascoltatore di eventi di tipo ItemListener  
associato alla Choice.

void select(int pos), void select(String str), per selezionare una scelta.

Facciamo un programmino che usi le Choice, però visto che finora abbiamo creato delle applicazioni a finestre, creiamo un’applet, visto che e GUI, come già detto, possono essere usate in ambedue i tipi di programmi.  
Parlando di applet, visto che non lo abbiamo fatto finora, creiamo un applet che legge i parametri passatigli dal file html che lo invoca, come fanno in pratica la maggior parte degli applet presenti sul sito html.it.  
Nell’applet, a titolo di esempio ho messo un piccolo controllo  
sul campo autore, questo solo per farvi vedere come si può fare. La cosa può risultare utile se si crea un’applet e la si distribuisce, in modo da lasciare obbligatorio il parametro autore e il suo valore, per evitare che altri possano spacciarsi per il ceratore dell’applet, e quindi per affermare i diritti di cerazione dell’applet.

Il controllo non è sofisticato, un hacker lo eviterebbe facilmente, però un hacker non ha interesse di crackare un programma free, solo per eliminare i diritti d’autore, mentre per un utente normale è impossibile eseguire il programma senza specificare il nome dell’autore. Quel nome può anche essere ad esempio un numero di serie, se si vuole vendere un programma.

Il file html, si chiamerà Choice.html, ed il suo contenuto sarà:

<html>  
<head>  
<title>  
Esempio di uso della java.awt.Choice e del passaggio di parametri ad un applet  
</title>  
</head>  
<body>  
<APPLET code=”Scelte.class” width=350 height=100>  
<param name=loc1 value=”Benevento”>  
<param name=loc2 value=”Foiano di Val Fortore”>  
<param name=loc3 value=”Baselice”>  
<param name=loc4 value=”San Bartolomeo in Galdo”>  
<param name=loc5 value=”San Marco dei Cavoti”>  
<param name=loc6 value=”Montefalcone in Val Fortore”>  
<param name=loc7 value=”Pesco Sannita”>  
<param name=loc8 value=”Colle Sannita”>  
<param name=loc9 value=”Castelvetere in Val Fortore”>  
<param name=loc10 value=”Castelfranco in Miscano”>  
<param name=loc11 value=”Ginestra degli Schiavoni”>  
<param name=loc12 value=”San Giorgio la Molara”>  
<param name=loc13 value=”Molinara”>  
<param name=loc14 value=”Fragneto Monforte”>  
<param name=loc15 value=”Circello”>  
<param name=loc16 value=”Campolattaro”>  
<param name=autore value=”Pietro Castellucci”>

</APPLET>  
</body>  
</html>

Mentre il codice dell’applet, editato nel file chiamato Scelte.java, è:

import java.awt.*;  
import java.awt.event.*;  
import java.applet.*;

public class Scelte extends Applet  
{  
Label text=new Label(“Bellezze che possono essere visitate nella località scelta”);  
Choice scelte=new Choice();  
public void init()  
{  
// Controllo diritti di autore:  
String autore=getParameter(“autore”);  
if (autore==null) System.exit(0);  
if (autore.compareTo(“Pietro Castellucci”)!=0)  
{  
System.out.println(“Parametro autore non valido, inserire come valore Pietro Castellucci”);  
System.exit(0);  
}  
String p=getParameter(“loc1”);  
if (p==null) p=”Località 1 non definita”;  
scelte.add(p);

p=getParameter(“loc2”);  
if (p==null) p=”Località 2 non definita”;  
scelte.add(p);

p=getParameter(“loc3”);  
if (p==null) p=”Località 3 non definita”;  
scelte.add(p);

scelte.add(p);

p=getParameter(“loc4”);  
if (p==null) p=”Località 4 non definita”;  
scelte.add(p);

p=getParameter(“loc5”);  
if (p==null) p=”Località 5 non definita”;  
scelte.add(p);

p=getParameter(“loc6”);  
if (p==null) p=”Località 6 non definita”;  
scelte.add(p);

p=getParameter(“loc7”);  
if (p==null) p=”Località 7 non definita”;  
scelte.add(p);

p=getParameter(“loc8”);  
if (p==null) p=”Località 8 non definita”;  
scelte.add(p);

p=getParameter(“loc9”);  
if (p==null) p=”Località 9 non definita”;  
scelte.add(p);

p=getParameter(“loc10”);  
if (p==null) p=”Località 10 non definita”;  
scelte.add(p);

p=getParameter(“loc11”);  
if (p==null) p=”Località 11 non definita”;  
scelte.add(p);

p=getParameter(“loc12”);  
if (p==null) p=”Località 12 non definita”;  
scelte.add(p);

p=getParameter(“loc13”);  
if (p==null) p=”Località 13 non definita”;  
scelte.add(p);

p=getParameter(“loc14”);  
if (p==null) p=”Località 14 non definita”;  
scelte.add(p);

p=getParameter(“loc15”);  
if (p==null) p=”Località 15 non definita”;  
scelte.add(p);

p=getParameter(“loc16”);  
if (p==null) p=”Località 16 non definita”;  
scelte.add(p);

p=getParameter(“loc17”);  
if (p==null) p=”Località 17 non definita”;  
scelte.add(p);

scelte.addItemListener(new ascoltaChoice());

// Visualizzo la Choice

add(scelte, borderLayout.CENTER);  
add(text, borderLayout.SOUTH);

}

class ascoltaChoice implements ItemListener  
{

public void itemStateChanged(ItemEvent e)  
{

int indice=scelte.getSelectedIndex();

if (indice==0) text.setText(“Rocca dei Rettori, arco di traiano, anfiteatro Romano”);  
if (indice==1) text.setText(“località San Giovanni, Campanile, via Roma, lago”);  
if (indice==2) text.setText(“oasi di San Leonardo”);  
if (indice==3) text.setText(“centro storico”);  
if (indice==4) text.setText(“centro storico”);  
if (indice==5) text.setText(“”);  
if (indice==6) text.setText(“”);  
if (indice==7) text.setText(“”);  
if (indice==8) text.setText(“”);  
if (indice==9) text.setText(“Bosco”);  
if (indice==10) text.setText(“”);  
if (indice==11) text.setText(“Lago di San Giorgio”);  
if (indice==12) text.setText(“”);  
if (indice==13) text.setText(“Piana Romana, centro storico, case di Padre Pio”);  
if (indice==14) text.setText(“Raduno internazionale di mongolfiere, palazzo Ducale”);  
if (indice==15) text.setText(“”);  
if (indice==16) text.setText(“Diga di Campolattaro”);  
}  
}  
}

L’ultimo componente GUI che vediamo in questa lezione è lo ScrollPane, ovvero un pannello in cui è possibile inserire un componente grande a piacere, che deve essere visualizzato a pezzi.  
Lo ScrollPane usa due Scrollbar, una orizzontale ed una verticale, per selezionare la parte del GUI che si vuole vedere. Da notare che la Scrollbar è un oggetto GUI anch’esso, quindi può essere inserito e gestito nelle proprie applicazioni. Nello ScrollPane si può scegliere se visualizzare le Scrollbar sempre, mai o solo se è necessario.  
Uno ScrollPane tipico è quello che si usa per visualizzare un file di testo.  
Ho due tipi di costruttori per l’oggetto:

ScrollPane(int sbp), crea uno ScrollPane con politica di visualizzazione lelle Scrollbar definita da  
spb.

I valori possibili sono :

ScrollPane.SCROLLBARS_ALWAYS sempre in vista  
ScrollPane.SCROLLBARS\_AS\_NEEDED in vista solo se ce n’è bisogno  
ScrollPane.SCROLLBARS_NEVER mai in vista

ScrollPane(), crea uno ScrollPane con politica ScrollPane.SCROLLBARS\_AS\_NEEDED.

tra i metodi, oltre a quelli per gestire lo ScrollPane troviamo quelli di Container e di Component, infatti ScrollPane è una estensione di Container, quindi può essere visto come un contenitore può essere visto come un componente.

![ScrollPane](http://html.it/guide/img/guida_java/24.gif)