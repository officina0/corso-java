# 116 Contenitori E Gestione Dei Layout

Nella lezione precedente abbiamo visto come inserire nelle nostre applicazioni e nei nostri applets delle etichette e dei bottoni. Per fare questo abbiamo usato dei contenitori e dei layout, senza sapere come funzionassero. In questa lezione vedremo il loro funzionamento, e come sfruttarli per rendere migliori le nostre interfacce.  
Iniziamo a descrivere la classe Container del package java.awt, esso è una estensione della classe Component, dalla quale provengono tutti i componenti GUI, ed in effetti anch’esso è un componente GUI.

![classe Container](http://html.it/guide/img/guida_java/22a.gif)

Un Container è un contenitore per oggetti GUI, e quindi anche per altri container. Esso se usato insieme ad un Layout Manager permette di contenere anche più di un GUI \(e Contenitori\), permettendo di fare apparire più oggetti nelle nostre interfacce. Vediamo cosa contiene la classe.  
Container\(\) , è l’unico costruttore della classe.

Alcuni metodi:

Component add\(Component comp\), aggiunge il componente alla fine del contenitore.  
Component add\(Component comp, int index\), aggiunge il componente nella posizione indicata del contenitore.  
void add\(Component comp, Object constraints\) e void add\(Component comp, Object constraints, int index\) , come prima, solo che si specificano delle costanti per il componente inserito.  
Component add\(String name, Component comp\), aggiunge al contenitore il componente e gli da un nome.  
void addContainerListener\(ContainerListener l\), setta il ContainerListener che ascolterà gli eventi occorsi al contenitore.  
void doLayout\(\), causa la disposizione dei componenti nel contenitore.  
Component findComponentAt\(int x, int y\), restituisce il componente che si trova nella posizione specificata del contenitore \(posizione grafica\).  
Component findComponentAt\(Point p\) , restituisce il componente che si trova nella posizione specificata del contenitore \(posizione grafica\).  
float getalignmentX\(\), restituisce l’allineamento lungo l’asse X del contenitore.  
float getalignmentY\(\), restituisce l’allineamento lungo l’asse Y del contenitore.  
Component getComponent\(int n\), restituisce l’ennesimo componente contenuto nel contenitore.  
Component getComponentAt\(int x, int y\), restituisce il componente che si trova nella posizione specificata del contenitore \(posizione grafica\).  
Component getComponentAt\(Point p\), restituisce il componente che si trova nella posizione specificata del contenitore \(posizione grafica\).  
int getComponentCount\(\) , da il numero di componenti conteinuti nel contenitore.  
Component\[\] getComponents\(\), da tutti i componenti contenuti nel contenitore.  
Insets getInsets\(\), da un’oggetto Insets per il contenitore, questo oggetto rappresenta le dimensioni \(grafiche\) del contenitore.  
LayoutManager getLayout\(\), restituisce il LayuotManager associato al contenitore.  
EventListener\[\] getListeners\(Class listenerType\), restituisce tutti gli ascoltatori di eventi associati al contenitore.  
Dimension getMaximumsize\(\), restituisce la dimensione massima che può assumere il contenitore.  
Dimension getMinimumsize\(\), restituisce la dimensione minima che può assumere il contenitore.  
Dimension getPreferredsize\(\), restituisce la dimensione preferita per il contenitore.  
void list\(PrintStream out, int indent\), stampa la lista dei componenti contenuti nel contenitore, su uno stream di output.  
void list\(PrintWriter out, int indent\), Stampa su una stampante i componenti contenuti nel contenitore.  
void paint\(Graphics g\), disegna il contenitore. La paint è una funzione molto importante, vedremo come ridefinirla per disegnare sulla finestra.  
void paintComponents\(Graphics g\). disegna tutti i componenti del contenitore.  
void print\(Graphics g\), stampa il contenitore \(l’aspetto grafico\).  
void printComponents\(Graphics g\), stampa tutti i componenti contenuti nel contenitore \(il loro aspetto grafico\).  
void remove\(Component comp\), elimina il componente specificato dal contenitore.  
void remove\(int index\), rimuove il componente che si trova nella posizione specificata nel contenitore.  
void removeAll\(\), rimuove tutti i componenti contenuti nel contenitore.  
void removeContainerListener\(ContainerListener l\), elimina l’ascoltatore di eventi per containers da questo container.  
void setfont\(font f\), setta le font usate per il testo nel contenitore.  
void setLayout\(LayoutManager mgr\), associa al contenitore un gestore di layout.  
void update\(Graphics g\), ridisegna il contenitore.  
void validate\(\),

invalida il contenitore e i suoi componenti.

Inserendo componenti nel contenitore, come sel successivo esempio, ci accorgiamo che solo l’ultimo viene visualizzato, questo perché non vi abbiamo dato un gestore di Layout, nel caso di applet invece, il LayoutManager di default è il FlowLayout.

import java.awt.\*;  
public class Contenitore extends Frame  
{  
Label l1=new Label\(“Etichetta1”\);  
Label l2=new Label\(“Etichetta2”\);  
Label l3=new Label\(“Etichetta3”\);  
Label l4=new Label\(“Etichetta4”\);  
Label l5=new Label\(“Etichetta5”\);  
public Contenitore\(\)  
{  
// uso add, perchè il Frame è una estensione di Window, che a sua  
// votla estende Container.  
add\(l1\);  
add\(l2\);  
add\(l3\);  
add\(l4\);  
add\(l5\);  
doLayout\(\);  
pack\(\);  
show\(\);  
}  
public static void main\(String \[\] arg\)  
{  
new Contenitore\(\);  
}

}

Quindi vediamo come inserire un gestore di Layout nel contenitore, il metodo della classe container che lo permette è void setLayout\(LayoutManager mgr\), bisogna solo vedere come è l’oggetto di tipo LayoutManager.  
LayoutManager è una interfaccia, tra le calssi che la implementano vedremo GridLayout, FlowLayout.  
Esiste un’altra interfaccia denominata LayoutManager2, che estende questa, e che ha altre classi che la implementano, di queste vedremo: CardLayout, borderLayout, GridBagLayout, BoxLayout, OverlayLayout.  
Quindi nel metodo setLayout \(…\) possiamo usare come parametro un’oggetto di queste classi.  
A questo punto dobbiamo capire qual è la differenza tra i vari layout manager.  
Il FlowLayout permette di disporre i componenti di un contenitore da sinistra verso destra su un’unica linea.

![FlowLayout](http://html.it/guide/img/guida_java/22b.gif)

Per usare il LayoutManager FlowLayout nell’esempio precedente basta invocare il metodo setLayout con parametro new FlowLayout\(\), e poi i componenti vengono automaticamente inseriti dalla add da destra verso sinistra sulla stessa linea.

import java.awt.\*;  
public class ContEFL extends Frame  
{  
Label l1=new Label\(“Etichetta1”\);  
Label l2=new Label\(“Etichetta2”\);  
Label l3=new Label\(“Etichetta3”\);  
Label l4=new Label\(“Etichetta4”\);  
Label l5=new Label\(“Etichetta5”\);  
public ContEFL\(\)  
{  
// uso add, perchè il Frame è una estensione di Window, che a sua  
// votla estende Container.  
setLayout\(new FlowLayout\(\)\);  
add\(l1\);  
add\(l2\);  
add\(l3\);  
add\(l4\);  
add\(l5\);  
pack\(\);  
show\(\);  
}  
public static void main\(String \[\] arg\)  
{  
new ContEFL\(\);  
}  
}

La finestra risultante è:

![finestra risultante](http://html.it/guide/img/guida_java/22c.gif)

Il GridLayout permette di disporre i componenti in un contenitore come se fossero disposti su una griglia.

![GridLayout ](http://html.it/guide/img/guida_java/22d.gif)

Tutti i componenti assumeranno a stessa dimensione. La classe ha tre costruttori, uno senza parametri che crea una griglia di 1 riga, praticamente un FlowLayout, e altri due che permettono di specificare la dimensione della griglia.  
L’esempio precedente cambia così:

import java.awt.\*;  
public class ContEGL extends Frame  
{  
Label l1=new Label\(“Etichetta1”\);  
Label l2=new Label\(“Etichetta2”\);  
Label l3=new Label\(“Etichetta3”\);  
Label l4=new Label\(“Etichetta4”\);  
Label l5=new Label\(“Etichetta5”\);  
public ContEGL\(\)  
{  
// uso add, perchè il Frame è una estensione di Window, che a sua  
// votla estende Container.  
setLayout\(new GridLayout\(2,3\)\);  
add\(l1\);  
add\(l2\);  
add\(l3\);  
add\(l4\);  
add\(l5\);  
pack\(\);  
show\(\);  
}  
public static void main\(String \[\] arg\)  
{  
new ContEGL\(\);  
}  
}

Il risultato è la seguente finestra:

![risultato](http://html.it/guide/img/guida_java/22e.gif)

Il LayoutManager CardLayout permette di visualizzare componenti diversi in tempi diversi, ovvero esso visualizza solo un componente alla volta, ma il componente visualizzato può essere cambiato.  
Ecco un esempio dell’uso di CardLayout

import java.awt._;  
import java.awt.event._;  
public class ContECL extends Frame  
{  
Label l1=new Label\(“Etichetta1”\);  
Label l2=new Label\(“Etichetta2”\);  
Label l3=new Label\(“Etichetta3”\);  
Label l4=new Label\(“Etichetta4”\);  
Label l5=new Label\(“Etichetta5”\);  
Panel p=new Panel\(new GridLayout\(2,1\)\);  
CardLayout CL=new CardLayout\(14,14\);  
Panel p1=new Panel\(CL\);  
Panel NPB=new Panel\(new FlowLayout\(\)\);  
public ContECL\(\)  
{  
p.add\(NPB\);  
p.add\(p1\);  
Button N=new Button\(“Prossimo”\);  
Button P=new Button\(“Precedente”\);  
NPB.add\(P\);  
NPB.add\(N\);  
P.addActionListener\(new ActionListener\(\)  
{  
public void actionPerformed\(ActionEvent e\)  
{  
CL.previous\(p1\);  
}  
}  
\);  
N.addActionListener\(new ActionListener\(\)  
{  
public void actionPerformed\(ActionEvent e\)  
{  
CL.next\(p1\);  
}  
}  
\);  
p1.add\(“uno”,l1\);  
p1.add\(“due”,l2\);  
p1.add\(“tre”,l3\);  
p1.add\(“quattro”,l4\);  
p1.add\(“cinque”,l5\);  
add\(p\);  
pack\(\);  
show\(\);  
}  
public static void main\(String \[\] arg\)  
{  
new ContECL\(\);  
}  
}

Il risultato dell’ esempio sono le seguenti finestre:

![risultato](http://html.it/guide/img/guida_java/22f.gif) ![risultato](http://html.it/guide/img/guida_java/22g.gif) ![risultato](http://html.it/guide/img/guida_java/22h.gif) ![](http://html.it/guide/img/guida_java/22i.gif) ![risultato](http://html.it/guide/img/guida_java/22l.gif)

Che mostrano come le cinque etichette vengono mostrate nella stessa posizione in tempi differenti.  
Da notare nell’esempio che sono stati usati anche altri gestori di Layout, questo per inserire i bottoni Prossimo e Precedente.

borderLayout è uno dei gestori di Layout più usati, esso permette di inserire in un contenitore cinque componenti, uno a Nord del contenitore, uno a Sud, uno a Est , uno ad Ovest, ed uno al centro.  
La dimensione del contenitre verrà data dal componente centrale, le cinque posizioni sono indicate dalle costanti della classe:

borderLayout.NORTH  
borderLayout.SOUTH  
borderLayout.EAST  
borderLayout.WEST  
borderLayout.CENTER

Ecco un’esempio di uso di borderLayout. Nell’esempio ho cambiato i colori di sfondo delle etichette per far vedere il componente intero.

import java.awt.\*;

public class ContEBL extends Frame  
{

Label l1=new Label\(“Etichetta a Nord”,Label.CENTER\);  
Label l2=new Label\(“Etichetta a Sud”,Label.CENTER\);  
Label l3=new Label\(“Etichetta a Est”,Label.CENTER\);  
Label l4=new Label\(“Etichetta a Ovest”,Label.CENTER\);  
Label l5=new Label\(“Etichetta al Centro”,Label.CENTER\);

public ContEBL\(\)  
{  
// uso add, perchè il Frame è una estensione di Window, che a sua  
// votla estende Container.  
l1.setBackground\(Color.pink\);  
l2.setBackground\(Color.lightGray\);  
l3.setBackground\(Color.green\);  
l4.setBackground\(Color.yellow\);  
l5.setBackground\(Color.orange\);  
setLayout\(new borderLayout\(\)\);  
add\(l1,borderLayout.NORTH\);  
add\(l2,borderLayout.SOUTH\);  
add\(l3,borderLayout.EAST\);  
add\(l4,borderLayout.WEST\);  
add\(l5,borderLayout.CENTER\);  
pack\(\);  
show\(\);  
}  
public static void main\(String \[\] arg\)  
{  
new ContEBL\(\);  
}  
}

Il risultato è:

![risultato](http://html.it/guide/img/guida_java/22m.gif)

Esistono anche altri gestori di Layout, che non vedremo, che però potete studiare insieme a questi nella documentazione ufficiale delle Java Development Kit.

