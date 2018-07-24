Iniziamo la panoramica sui GUI, per usare nella stessa finestra più componenti abbiamo bisogno di uno o più contenitori, questi li vedremo in seguito, però in alcuni casi di questo paragrafo dovrò usarli senza averli spiegati, perché mi necessiteranno più di un componente GUI nella finestra.  
Sono sicuro che non avrete problemi nel comprendere lo stesso i programmi, perché userò in modo semplice sia i Layout che i Container, però mi scuso lo stesso se questo vi arrecherà anche un minimo disagio.  
In particolare userò i Panel e userò delle costanti della classe borderLayout, la quale divide il Container, in questo caso il Panel, in cinque zone, NORTH, SOUTH, WEST, EAST e CENTER.  
In questa lezione esamineremo due componenti, i più semplici, le Label e i Bottoni. Le prime non hanno nessun evento associato mentre i secondi, a seconda del tipo di bottone hanno due tipi di eventi associati.

Etichette
---------

Iniziamo la nostra panoramica sui componenti vedendo il primo semplice elemento, l’etichetta, che tra l’altro abbiamo già usato. L’etichetta è un contenitore per un testo, essa fa apparire sullo schermo una singola linea di testo non editabile dall’utente, che però può essere cambiata dall’applicazione.  
L’etichetta, chiamata in Java Label, si trova nel package java.awt, quindi per essere usata il nostro programma dovrà cominciare con una import java.awt.Label (per inglobare solo la classe Label del package), oppure import java.awt.* (per inglobare tutte le classi del package). All’interno del programma che la vuole usare bisognerà quindi dichiarare un oggetto di tipo Label nel seguente modo:

Label etichetta = new Label();

oppure, se non è stato importato il package java.awt

:

java.awt.Label etichetta = new Label();

La Label ha tre costruttori:

Label() , che costruisce un’etichetta vuota.  
Label(String text), che costruisce un’etichetta con testo text, giustificata a sinistra.  
Label(String text, int alignment) , che costruisce un’etichetta con testo text, giustificata secondo alignment. I valori possibili di alignment sono: Label.LEFT, Label.RIGHT, e Label.CENTER.

Alcuni metodi dell’oggetto sono:

int getalignment(), da la giustificazione attuale dell’oggetto.  
String getText() , da il testo contenuto nella label.  
void setalignment(int alignment), cambia la giustificazione della Label con alignment.  
void setText(String text), setta il testo della Label.

Poi c’è il metoto AccessibleContext getAccessibleContext(), che vedremo quando vedremo i contenitori, infatti la Label è in effetti un contenitore di testo.  
Label proviene dalla seguente gerarchia:

![gerarchia label](http://html.it/guide/img/guida_java/21.gif)

Quindi eredita i soliti metodi di Object e quelli di java.awt.Component. Tutti i componenti GUI sono ereditati da Component, quindi alla fine della panoramica dei GUI parleremo di Component, la quale contiene metodi molto utili per la gestione di questi componenti, come ad esempio metodi per settare e ottenere informazioni su dimensioni, colori dello sfondo, colori del testo, bordi, ecc… del componente.  
A parte i metodi di Component, siamo pronti ad usare le Label nei nostri programmi. Per ora non faccio nessun esempio dell’uso delle Label, le vedremo ne prossimo paragrafo quando parleremo dei bottoni.  
Il package swing mette a disposizione un’naloga classe denominata JLabel, essa è una estensione di Label, e mette a disposizione una vasta gamma di funzionalità supplementari, ad esempio è possibile creare JLabel che hanno sia un testo che un’immagine.

Bottoni
-------

Un altro componente molto usato, e di semplice realizzazione è il bottone, esso è leggermente più complicato della Label, in quanto può ricevere degli eventi (Click sul bottone).  
I bottoni sono di tre tipi, i bottoni normali, le Checkbox e i radiobutton, iniziamo a vedere i primi.  
La classe che definisce i bottoni è Button di java.awt, essa mette a disposiozione tutti i metodi per gestire l’aspetto del bottone. I costruttori della classe sono due:

Button(), costruisce un bottone senza etichetta;  
Button(String txt), costruisce un bottone con etichetta txt.

Alcuni metodi della classe sono:

void addActionListener(ActionListener l), aggiunge il bottone ad un oggetto chiamato  
ActionListener, il quale è un ascoltatore di eventi. Quando il bottone verrà cliccato, l’evento verrà gestito dall’ActionListener.  
AccessibleContext getAccessibleContext(), come per le Label, i bottoni sono anch’essi dei contenitori.  
String getActionCommand(), ogni bottone ha associato un comando, questo ne restituisce quello attuale.  
String getLabel(), restituisce l’etichetta associata al bottone.  
EventListener\[\] getListeners(Class listenerType), da un array contenente tutti gli ascoltatori di aventi definiti per questo bottone.  
addXXXListener(), aggiunge un ascoltatore di eventi, l’ascoltatore di eventi di tipo XXX. In Java esistono vari tipi di eventi, li vedremo tra poco, uno di questi è l’evento di tipo action, in questo caso il metodo è addActionListener( )  
void removeActionListener(ActionListener l), elimina l’ascoltatore di eventi l precedentemente associato al bottone.  
void setActionCommand(String command), setta il comando associato al bottone, inizialmente il comando è la label.  
void setLabel(String label), setta la Label (etichetta) del bottone.

Siamo pronti per definire il nostro primo bottone, dobbiamo solo riuscire a capire come catturarne l’evento scaturito dalla pressione dello stesso. Come già detto in precedenza io parlerò dellagestione degli eventi secondo Java 2, perché il vecchio modello di gestione degli eventi di Java 1 è obsoleto e non funzionale.  
Secondo Java 2, definito un bottone, visto che l’evento principale del bottone è il click, che è un evento appartenente alla famiglia degli eventi Action, bisogna associare un ActionListener allo stesso bottone, usando il metodo addActionListaner(ActionListener l); Quello che dobbiamo fare noi è creare un ActinListener specializzato per il nostro bottone (o per i nostri bottoni).  
ActionListener è definito nel package java.awt.event (da includere nel programma) esso è una interfaccia, quindi deve essere implementata, all’interno della classe in cui è definita.  
Ad esempio, sto facendo un Frame chiamato Pippo, contenente un bottone, di cui voglio ascoltare il click, lo voglio ascoltare con un ActionListener specializzato da me chiamato AscoltaPippo, dovrò scrivere qualcosa del tipo:

import java.awt.*;  
import java.awt.event.*;  
public class Pippo extends Frame  
{  
Button bottone=new Button(“CIAO”);  
… metodi di Pippo, tra cui il costruttore, che conterrà un bottone.addActionListener(new AscoltaPippo());  
  
public class AscoltaPippo implements ActionListener()  
{  
// Gestione dell’evento  
} // Fine classe AscoltaPippo interna a Pippo  
}//Fine classe Pippo

In AscoltaPippo dovrò specializzare tutti i metodi dell’interfaccia che sto estendendo, ActionListener ha un solo metodo:

void actionPerformed(ActionEvent e) quindi in AscoltaPippo dovrò definire il metodo public void actionPerformed(ActionEvent e), è questo il metodo che verrà invocato quando cliccherò il bottone.  
L’ActionEvent che mi arriva nel metodo come parametro conterrà tutte le informazioni riguardanti l’evento, tra cui il comando. AscoltaPippo può essere associato anche a più bottoni, la sua actionPerformed verrà invocata al click di ognuno di questi bottoni. Chiariamo subito il concetto, che forse è un pochino più difficile delle altre cose che abbiamo fatto con un piccolo esempio, vi assicuro però che una volta assimilato il concetto, la gestione degli eventi di Java è molto semplice.  
Creiamo un Frame che ha un bottone e una Label, ogni volta che il bottone viene cliccato la label da un messaggio.

import java.awt.*;  
import java.awt.event.*;  
public class Bottone extends Frame  
{  
// Costruttore classe Bottone  
Button cliccami=new Button(“Cliccami”);  
Label cliccato=new Label(“Non mi hai cliccato nemmeno una volta”);  
public Bottone()  
{  
cliccami.addActionListener(new Ascoltatore());  
// setup comando  
cliccami.setActionCommand(“CLICK”);  
// Aggiungo il bottone e la label al Frame.  
// Non badate alle seguenti istruzioni,  
// la add serve ad aggiungere un componente ad  
// un contenitore, e il secondo parametro della  
// add, ovvero borderLayout, è un gestore di Layout,  
// che serve a stabilire il modo in cui gli oggetti  
// GUI vengono posti nel contenitore.  
add(cliccami,borderLayout.NORTH);  
add(cliccato,borderLayout.SOUTH);  
// metodi di Frame  
pack();  
show();  
}  
// main  
public static void main (String \[\] arg)  
{  
new Bottone();  
}  
// Ascoltatore di eventi Action  
int Volte=2;  
public class Ascoltatore implements ActionListener  
{  
public void actionPerformed (ActionEvent e)  
{  
String Comando=e.getActionCommand();  
if (Comando.compareTo(“CLICK”)==0)  
{  
cliccato.setText(“Mi hai cliccato”);  
cliccami.setLabel(“Ricliccami”);  
cliccami.setActionCommand(“RECLICK”);  
};  
if (Comando.compareTo(“RECLICK”)==0)  
cliccato.setText(“Mi hai cliccato “+(Volte++)+” volte.”);  
}  
}// Fine Ascoltatore  
}// Fine Bottone  

  

Il risultato è la seguente finestra:

![Risultato](http://html.it/guide/img/guida_java/21a.gif) ![Risultato](http://html.it/guide/img/guida_java/21b.gif) ![Risultato](http://html.it/guide/img/guida_java/21c.gif)

Come si vede in Ascoltatore, vengono gestiti due comandi differenti associati allo stesso bottone, ed il bottone quando viene cliccato la prima volta cambia il comando associato all’evento click.  
Provate a modificarlo cambiando il comando per stabilire con certezza le prime dieci volte che viene cliccato e stampando nella label “Hai cliccato nove volte il bottone” (la nona volta).

Ricapitolando, in una interfaccia grafica innanzitutto si programma l’aspetto grafico, ovvero si inseriscono i bottoni, i menu ecc, poi si creano e aggiungono gli ascoltatori di eventi, i quali a seconda dell’evento che si è verificato chiamano uno o l’altro metodo di gestione del programma (aggiorna l’interfaccia, calcola le funzioni del programma di cui è interfaccia grafica).  
Questo è un principio generale della programmazione di Interfacce Grafiche, ovvero della programmazione ad eventi, ogni programma che usate funziona così, ed ogni programma che scriverete funzionerà così.  
Il tipo di programmazione è leggermente differente da quella a cui un programmatore su piattaforma non grafiche (tipo Dos, Linux senza X-Win) sono abituati, e all’inizio può risultare non molto semplice, ma ci si deve solo abituare, che poi è lo stesso.

Il secondo tipo di bottoni che vediamo sono i Checkbox, ovvero quei bottoni di tipo booleano, che trattengono lo stato. Si usano per fare delle scelte, tra di loro non esclusive o esclusive (in questo caso sono dei radiobutton), a esempio se si devono scegliere alcuni tra gli hobby preferiti. Questi bottoni una volta cliccati rimangono tali, e bisogna ricliccarli per farli tornare nella situazione di partenza.  
Per creare una Checkbox bisogna creare un oggetto appartenente alla classe Checkbox di java.awt, i costruttori possibili sono i seguenti:

Checkbox(), crea una Checkbox senza etichetta.  
Checkbox(String label), crea una Checkbox con etichetta label.  
Checkbox(String label, boolean state), crea una Checkbox con etichetta label, e cliccata o meno a seconda del valore booleano state.

Invece per le radiobutton:

Checkbox(String label, boolean state, CheckboxGroup group), crea una Checkbox con etichetta label inserita in una specifica  
CheckboxGroup, che è un contenitore di Checkbox.  
Checkbox(String label, CheckboxGroup group, boolean state) crea una Checkbox con etichetta label inserita in una specifica  
CheckboxGroup, e settata a seconda del valore di state.

In pratica tra tutti i bottoni inseriti nel CheckboxGroup, solo uno può essere cliccato, gli altri devono essere non cliccati. Le Checkbox del primo tipo come ho detto vengono usate per effettuere delle scelte non esclusive, come ad esempio la scelta degli interessi tra una lista di interessi possibili, mentre le Checkbox del secondo tipo, ovvero i radiobuttons servono per fare delle scelte esclusive, ad esempio la scelta del reddito di una persona tra tante possibili fasce.

tra i metodi della classe Checkbox alcuni più interessanti sono:

void addItemListener(ItemListener l) , associa un ItemListener alla Checkbox, l’ItemListener gestisce gli eventi dei Checkbox e non solo, sono un secondo tipo di eventi che impareremo a gestire, comunque somigliano agli ActionListener, solo che sono associati ad oggetti che hanno uno stato.  
CheckboxGroup getCheckboxGroup(), da l’eventuale gruppo in cui è inserita la Checkbox.  
Le solite String getLabel() e EventListener\[\] getListeners(Class listenerType) boolean getState(), questo metodo dice se la Checkbox è cliccataoppure no.  
void removeItemListener(ItemListener l), toglie l’attuale ascoltatore di eventi Item.  
void setCheckboxGroup(CheckboxGroup g), setta un gruppo per la Checkbox.  
void setLabel(String label), setta l’etichetta  
void setState(boolean state), setta lo stato.

Vediamo anche cosa contiene un CheckboxGroup. Il costruttore è uno senza parametri, e i metodi non dichiarati Deprecated sono tre:

Checkbox getSelectedCheckbox(), da la Checkbox selezionata nel gruppo.  
void setSelectedCheckbox(Checkbox box), setta la Checkbox del gruppo indicata.  
String toString(), da una stringa rappresentante l’oggetto.

Proviamo quindi a fare un esempietto che usa sia Checkbox che RadioButton.

import java.awt.*;  
import java.awt.event.*;  
public class Check extends Frame  
{  
// Costruttore classe Bottone  
Label r=new Label(“Reddito annuo”);  
Label i=new Label(“Interessi”);  
Button chiudi=new Button(“Chiudi Applicazione”);  
CheckboxGroup reddito=new CheckboxGroup();  
Checkbox r1=new Checkbox(“Da 0 ai 10”);  
Checkbox r2=new Checkbox(“Dagli 11 ai 30”);  
Checkbox r3=new Checkbox(“Dai 31 ai 70”);  
Checkbox r4=new Checkbox(“Dai 71 ai 100”);  
Checkbox r5=new Checkbox(“Dai 101 a 200”);  
Checkbox r6=new Checkbox(“Dai 201 a 500”);  
Checkbox r7=new Checkbox(“Sono ricco sfondato”);  
Checkbox i1=new Checkbox(“Sport”);  
Checkbox i2=new Checkbox(“Informatica”);  
Checkbox i3=new Checkbox(“Lettura”);  
Checkbox i4=new Checkbox(“Cinema”);  
Checkbox i5=new Checkbox(“Animali”);  
Checkbox i6=new Checkbox(“Fumetti”);  
Checkbox i7=new Checkbox(“Lotterie”);  
Checkbox i8=new Checkbox(“Donne nude”);  
Checkbox i9=new Checkbox(“Uomini nudi”);  
public Check()  
{  
chiudi.addActionListener(new Ascoltatore());  
Panel p1=new Panel();  
Panel p2=new Panel();  
p1.add(r);  
p1.add(r1);  
p1.add(r2);  
p1.add(r3);  
p1.add(r4);  
p1.add(r5);  
p1.add(r6);  
p1.add(r7);  
r1.setCheckboxGroup(reddito);  
r2.setCheckboxGroup(reddito);  
r3.setCheckboxGroup(reddito);  
r4.setCheckboxGroup(reddito);  
r5.setCheckboxGroup(reddito);  
r6.setCheckboxGroup(reddito);  
r7.setCheckboxGroup(reddito);  
add(p1,borderLayout.NORTH);  
p2.add(i);  
p2.add(i1);  
p2.add(i2);  
p2.add(i3);  
p2.add(i4);  
p2.add(i5);  
p2.add(i6);  
p2.add(i7);  
p2.add(i8);  
p2.add(i9);  
add(p2,borderLayout.CENTER);  
add(chiudi,borderLayout.SOUTH);  
// metodi di Frame  
pack();  
show();  
}  
// main  
public static void main (String \[\] arg)  
{  
new Check2();  
}  
// Ascoltatore di eventi Action  
public class Ascoltatore implements ActionListener  
{  
public void actionPerformed (ActionEvent e)  
{  
System.out.println(“Iscrizione al sito XXXXXXX.it”);  
System.out.print(“Guadagno annuo “);  
if (r1.getState()) System.out.println(r1.getLabel());  
else if (r2.getState()) System.out.println(r2.getLabel());  
else if (r3.getState()) System.out.println(r3.getLabel());  
else if (r4.getState()) System.out.println(r4.getLabel());  
else if (r5.getState()) System.out.println(r5.getLabel());  
else if (r6.getState()) System.out.println(r6.getLabel());  
else if (r7.getState()) System.out.println(r7.getLabel()+” (Beato te!)”);  
else System.out.println(“NON DICHIARATO”);  
System.out.println(“Interessi dichiarati:”);  
if (i1.getState()) System.out.println(“t”+i1.getLabel());  
if (i2.getState()) System.out.println(“t”+i2.getLabel());  
if (i3.getState()) System.out.println(“t”+i3.getLabel());  
if (i4.getState()) System.out.println(“t”+i4.getLabel());  
if (i5.getState()) System.out.println(“t”+i5.getLabel());  
if (i6.getState()) System.out.println(“t”+i6.getLabel());  
if (i7.getState()) System.out.println(“t”+i7.getLabel());  
if (i8.getState()) System.out.println(“t”+i8.getLabel());  
if (i9.getState()) System.out.println(“t”+i9.getLabel());  
System.exit(0);  
}  
}// Fine Ascoltatore  
}// Fine Bottone  
  
Fig 4: Finestra del programma  

  

Ho dovuto di nuovo usere i layout e anche i Panel, me ne scuso perché ancora non ne parliamo, ma è l’unico modo per inserire più GUI nella stessa finestra, per esempio provate a togliere i Panel e quindi ad aggiungere tutto direttamente alla finestra, senza usare i Layout, vedrete cosa succede.  
Nell’applicazione c’è un bottone che serve a chiudere la finestra, questo perché ancora non siamo in grado di gestire gli eventi della finestra, come il close, che se lo provate vedrete che non va. tra qualche tempo saremo in grado di gestire anche quelli.

Nell’applicazione non ho gestito gli eventi delle Checkbox, questo perché non ne ho avuto bisogno, esistono situazioni in cui si devono gestire, e sono quelle in cui oltre allo stato del bottone cambia qualcosa, ad esempio lo stato di qualche GUI, come nell’esempio seguente.

import java.awt.*;  
import java.awt.event.*;  
public class Check2 extends Frame  
{  
// Costruttore classe Bottone  
Button chiudi=new Button(“Chiudi Applicazione”);  
Checkbox i1=new Checkbox(“primi 3”);  
Checkbox i2=new Checkbox(“secondi 4”);  
Checkbox ii1=new Checkbox(“uno”);  
Checkbox ii2=new Checkbox(“due”);  
Checkbox ii3=new Checkbox(“tre”);  
Checkbox ii4=new Checkbox(“quattro”);  
Checkbox ii5=new Checkbox(“cinque”);  
Checkbox ii6=new Checkbox(“sei”);  
Checkbox ii7=new Checkbox(“sette”);  
public Check2()  
{  
chiudi.addActionListener(new Ascoltatore());  
Panel p1=new Panel();  
Panel p2=new Panel();  
p1.add(ii1);  
p1.add(ii2);  
p1.add(ii3);  
p1.add(ii4);  
p1.add(ii5);  
p1.add(ii6);  
p1.add(ii7);  
ii1.addItemListener(new AItem1());  
ii2.addItemListener(new AItem1());  
ii3.addItemListener(new AItem1());  
ii4.addItemListener(new AItem1());  
ii5.addItemListener(new AItem1());  
ii6.addItemListener(new AItem1());  
ii7.addItemListener(new AItem1());  
i1.addItemListener(new AItem2());  
i2.addItemListener(new AItem2());  
add(p1,borderLayout.NORTH);  
p2.add(i1);  
p2.add(i2);  
add(p2,borderLayout.CENTER);  
add(chiudi,borderLayout.SOUTH);  
// metodi di Frame  
pack();  
show();  
}  
// main  
public static void main (String \[\] arg)  
{  
new Check();  
}  
// Ascoltatore di eventi Action  
public class Ascoltatore implements ActionListener  
{  
public void actionPerformed (ActionEvent e)  
{System.exit(0);}  
}// Fine Ascoltatore  
// Ascoltatori di eventi Item  
public class AItem1 implements ItemListener  
{  
public void itemStateChanged(ItemEvent e)  
{  
i1.setState(ii1.getState()&&  
ii2.getState()&&  
ii3.getState());  
i2.setState(ii4.getState()&&  
ii5.getState()&&  
ii6.getState()&&  
ii7.getState());  
}  
}//Fine AItem1  
public class AItem2 implements ItemListener  
{  
public void itemStateChanged(ItemEvent e)  
{  
ii1.setState(i1.getState());  
ii2.setState(i1.getState());  
ii3.setState(i1.getState());  
ii4.setState(i2.getState());  
ii5.setState(i2.getState());  
ii6.setState(i2.getState());  
ii7.setState(i2.getState());  
}  
}//Fine AItem2  
}// Fine Bottone