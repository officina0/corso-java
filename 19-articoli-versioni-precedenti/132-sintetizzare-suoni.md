# 132 Sintetizzare Suoni

Come ho gia detto è anche possibile sintetizzare suoni, questo è quello che fa il prossimo programma, da editare nel file sintetizzatore.java

import javax.swing._;  
import java.awt._;  
import java.awt.event._;  
import javax.swing.border._;  
import java.io._;  
import java.util._;  
import javax.sound.sampled._;  
import javax.sound.midi._;  
public class sintetizzatore extends JFrame  
{  
int TEMPO=50;  
int CANALE=0;  
int UNS=-1;  
int StrUMENTO=0;  
Sintetizzatore SINTETIZZATORE=new Sintetizzatore\(\);  
// GUI  
JLabel tastiera1=new JLabel\(new ImageIcon\(“tastiera1.gif”\)\);  
JLabel tastiera2=new JLabel\(new ImageIcon\(“tastiera2.gif”\)\);  
JButton OkB=new JButton\(“Ok”\);

JComboBox Strumenti=new JComboBox\(\);  
JComboBox Canali=new JComboBox\(\);  
JLabel uns=new JLabel\(“Ultima nota suonata”\);  
JTextField msg=new JTextField\(  
“Nessuna nota”  
\);  
JSlider SL=new JSlider\(\);  
public sintetizzatore\(\)  
{  
setTitle\(“Sintetizzatore sonoro by Pietro Castellucci”\);  
setupAspetto\(\);  
setupValori\(\);  
setupEventi\(\);  
SINTETIZZATORE.setStrumento\(  
Strumenti.getSelectedIndex\(\),CANALE\);  
setresizable\(false\);  
setdefaultCloseOperation\(DO\_NOTHING\_ON\_CLOSE\);  
pack\(\);  
Toolkit t = Toolkit.getdefaultToolkit\(\);  
Dimension d=t.getScreensize\(\);  
Dimension win=getsize\(\);  
win=getsize\(\);  
setLocation\(d.width/2-\(win.width/2\)-1,d.height/2-\(win.height/2\)-1\);  
show\(\);  
}

void setupAspetto\(\)  
{  
tastiera1.setCursor\(new Cursor\(Cursor.HAND\_CURSOR\)\);  
tastiera2.setCursor\(new Cursor\(Cursor.HAND\_CURSOR\)\);  
OkB.setCursor\(new Cursor\(Cursor.HAND\_CURSOR\)\);  
tastiera1.setToolTipText\(  
“Cliccami per suonare”  
\);  
tastiera2.setToolTipText\(  
“Cliccami per suonare”  
\);  
OkB.setToolTipText\(  
“Esco dal programma”  
\);

JPanel P=new JPanel\(new borderLayout\(\)\);  
JPanel Pannello=new JPanel\(new GridLayout\(2,1\)\);  
Pannello.add\(tastiera1\);  
Pannello.add\(tastiera2\);  
P.add\(Pannello,borderLayout.CENTER\);  
JPanel SP=new JPanel\(new FlowLayout\(\)\);  
Strumenti.setborder\(  
borderFactory.createTitledborder\(  
“Strumenti”  
\)  
\);  
Canali.setborder\(  
borderFactory.createTitledborder\(  
“Canali”  
\)  
\);  
SP.add\(Strumenti\);  
// SP.add\(Canali\);  
SP.add\(uns\);  
msg.setEditable\(false\);  
msg.setBackground\(P.getBackground\(\)\);  
SP.add\(msg\);  
SP.add\(OkB\);  
P.add\(SP,borderLayout.SOUTH\);  
SL.setborder\(borderFactory.createTitledborder\(  
“Pressione “  
\)\);  
SL.setOrientation\(JSlider.VERTICAL\);  
SL.setMajorTickSpacing\(25\);  
SL.setMaximum\(100\);  
SL.setMinimum\(0\);  
SL.setMinorTickSpacing\(1\);  
SL.setPaintLabels\(true\);  
SL.setPaintTicks\(true\);  
SL.setPainttrack\(true\);  
SL.setSnapToTicks\(true\);  
SL.setValue\(TEMPO\);  
P.add\(SL,borderLayout.EAST\);

setContentPane\(P\);  
}

void setupValori\(\)  
{  
try  
{  
int i=0;  
while \(true\)  
{  
String nome= SINTETIZZATORE.Strumenti\[i++\].getName\(\);  
Strumenti.addItem\(nome\);  
}  
}  
catch \(ArrayIndexOutOfBoundsException e\)  
{};

try  
{  
int i=0;  
while \(true\)  
{  
String nome=”Canale”+” “+\(i+1\);  
SINTETIZZATORE.CANALI\[i++\].allNotesOff\(\);  
Canali.addItem\(nome\);  
}  
}  
catch \(ArrayIndexOutOfBoundsException e\)  
{};  
if \(Strumenti.getItemCount\(\)&gt;0\)  
Strumenti.setSelectedIndex\(StrUMENTO\);  
}

void setupEventi\(\)  
{  
tastiera1.addMouseListener\(new Tastiera1Listener\(\)\);  
tastiera2.addMouseListener\(new Tastiera2Listener\(\)\);

Strumenti.addItemListener\(new ItemListener\(\)  
{  
public void itemStateChanged\(ItemEvent e\)  
{  
SINTETIZZATORE.setStrumento\(  
Strumenti.getSelectedIndex\(\),CANALE\);  
}  
}  
\);

Canali.addItemListener\(new ItemListener\(\)  
{  
public void itemStateChanged\(ItemEvent e\)  
{  
CANALE=Canali.getSelectedIndex\(\);  
SINTETIZZATORE.cambiaCanale\(CANALE\);  
}  
}\);

OkB.addActionListener\(new ActionListener\(\)  
{  
public void actionPerformed\(ActionEvent e\)  
{  
System.out.println\(“Grazie per avere suonato con me!”\);  
System.exit\(0\);  
}  
}  
\);

}  
// Eventi

public class Tastiera1Listener implements MouseListener  
{  
public void mouseClicked\(MouseEvent e\)  
{}  
public void mouseEntered\(MouseEvent e\)  
{}  
public void mouseExited\(MouseEvent e\)  
{}  
public void mousePressed\(MouseEvent e\)  
{  
// System.out.println\(“Tastiera 1 Punto: \(x=”+e.getX\(\)+”,y=”+e.getY\(\)+”\)”\);  
UNS=getNota\(e.getX\(\)\);  
// System.out.println\(“Nota=”+nota\);  
TEMPO=SL.getValue\(\);  
SINTETIZZATORE.suona\(UNS,TEMPO,CANALE\);  
msg.setText\(“”+\(UNS+1\)\);  
}  
public void mouseReleased\(MouseEvent e\)  
{}  
}

public class Tastiera2Listener implements MouseListener  
{  
public void mouseClicked\(MouseEvent e\)  
{}  
public void mouseEntered\(MouseEvent e\)  
{}  
public void mouseExited\(MouseEvent e\)  
{}  
public void mousePressed\(MouseEvent e\)  
{  
// System.out.println\(“Tastiera 2 Punto: \(x=”+e.getX\(\)+”,y=”+e.getY\(\)+”\)”\);  
UNS=64+getNota\(e.getX\(\)\);  
// System.out.println\(“Nota=”+nota\);  
TEMPO=SL.getValue\(\);  
SINTETIZZATORE.suona\(UNS,TEMPO,CANALE\);  
msg.setText\(“”+\(UNS+1\)\);  
}  
public void mouseReleased\(MouseEvent e\)  
{}  
}

// Utility fun  
int getNota \(int pos\)  
{  
int nota;  
nota=\(pos/12\);  
return nota;  
}

public static void main\(String\[\] arg\)  
{  
new sintetizzatore\(\);  
}

//**\***  
// SOUND MANAGER  
//**\***  
//_**\***_  
// Sintetizzatore:  
//_**\***_  
public class Sintetizzatore  
{  
private Synthesizer SYNT;  
private Sequencer sequencer;  
private Sequence seq;  
private Soundbank BANK;  
public Instrument\[\] Strumenti;  
public MidiChannel\[\] CANALI;  
public Sintetizzatore\(\)  
{  
setupSintetizzatore\(\);  
}  
void setupSintetizzatore\(\)  
{  
try  
{  
SYNT=MidiSystem.getSynthesizer\(\);  
sequencer=MidiSystem.getSequencer\(\);  
seq= new Sequence\(Sequence.PPQ, 10\);  
SYNT.open\(\);  
BANK = SYNT.getdefaultSoundbank\(\);  
if \(BANK != null\)  
Strumenti = SYNT.getdefaultSoundbank\(\).getInstruments\(\);  
else  
Strumenti = SYNT.getAvailableInstruments\(\);  
CANALI=SYNT.getChannels\(\);  
}  
catch\(MidiUnavailableException ecc\){tuttonull\(\);}  
catch\(InvalidMidiDataException ecc2\){tuttonull\(\);}  
;  
}  
void tuttonull\(\)  
{  
SYNT=null;  
sequencer=null;  
seq=null;  
BANK=null;  
Strumenti=null;  
}  
public void setStrumento\(int str,int can\)  
{  
SA=str;  
int prog=Strumenti\[str\].getPatch\(\).getProgram\(\);  
CANALI\[can\].programChange\(prog\);  
}  
private int SA=0;  
public void cambiaCanale\(int can\)  
{  
int prog=Strumenti\[SA\].getPatch\(\).getProgram\(\);  
CANALI\[can\].programChange\(prog\);  
}  
public void suona\(int nota,int tempo, int canale\)  
{  
CANALI\[canale\].allNotesOff\(\);  
CANALI\[canale\].noteOn\(nota,tempo\);  
}  
public void suona\(int nota,int tempo\)  
{  
suona\(nota,tempo,0\);  
}  
public void zitto\(\)  
{  
CANALI\[0\].allNotesOff\(\);  
}

} // End of Sintetizzatore  
}

Avrete notato la corposità del programma, non fatevi spaventare, perché la maggior parte del codice serve per la grafica e per sentire gli eventi, la parte interessante per sintetizzare suoni l’ho racchiusa nella sottoclasse Sintetizzatore, ovvero in sintetizzatore.Sintetizzatore ovvero la seguente:

//_**\***_  
// Sintetizzatore:  
//_**\***_  
public class Sintetizzatore  
{  
private Synthesizer SYNT;  
private Sequencer sequencer;  
private Sequence seq;  
private Soundbank BANK;  
public Instrument\[\] Strumenti;  
public MidiChannel\[\] CANALI;  
public Sintetizzatore\(\)  
{  
setupSintetizzatore\(\);  
}  
void setupSintetizzatore\(\)  
{  
try  
{  
SYNT=MidiSystem.getSynthesizer\(\);  
sequencer=MidiSystem.getSequencer\(\);  
seq= new Sequence\(Sequence.PPQ, 10\);  
SYNT.open\(\);  
BANK = SYNT.getdefaultSoundbank\(\);  
if \(BANK != null\)  
Strumenti = SYNT.getdefaultSoundbank\(\).getInstruments\(\);  
else  
Strumenti = SYNT.getAvailableInstruments\(\);  
CANALI=SYNT.getChannels\(\);  
}  
catch\(MidiUnavailableException ecc\){tuttonull\(\);}  
catch\(InvalidMidiDataException ecc2\){tuttonull\(\);}  
;  
}  
void tuttonull\(\)  
{  
SYNT=null;  
sequencer=null;  
seq=null;  
BANK=null;  
Strumenti=null;  
}  
public void setStrumento\(int str,int can\)  
{  
SA=str;  
int prog=Strumenti\[str\].getPatch\(\).getProgram\(\);  
CANALI\[can\].programChange\(prog\);  
}  
private int SA=0;  
public void cambiaCanale\(int can\)  
{  
int prog=Strumenti\[SA\].getPatch\(\).getProgram\(\);  
CANALI\[can\].programChange\(prog\);  
}  
public void suona\(int nota,int tempo, int canale\)  
{  
CANALI\[canale\].allNotesOff\(\);  
CANALI\[canale\].noteOn\(nota,tempo\);  
}  
public void suona\(int nota,int tempo\)  
{  
suona\(nota,tempo,0\);  
}  
public void zitto\(\)  
{  
CANALI\[0\].allNotesOff\(\);  
}  
} // End of Sintetizzatore

L’aspetto del programma è il seguente:

![interfaccia](http://html.it/guide/img/guida_java/38.gif)

