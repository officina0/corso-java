I pacchetti aggiunti al linguaggio sono 4:

1.  javax.sound.midi
2.  javax.sound.midi.spi
3.  javax.sound.sampled
4.  javax.sound.sampled.spi

Le parti che finiscono per spi servono a leggere e scrivere files sonori, convertirli o leggere files di strumenti nel caso di midi. Noi non vedremo queste parti, mentre vedremo javax.sound.sampled e javax.sound.midi.  
Il pacchetto javax.swing.sampled permette di registrare suonare e modificare dati sonori.  

Il contenuto del pacchetto è:

Interfacce
----------

*   Clip
*   DataLine
*   Line
*   LineListener
*   Mixer
*   Port
*   SourceDataLine
*   TargetdataLine

Classi
------

*   AudioFileFormat
*   AudioFileFormat.Type
*   AudioFormat
*   AudioFormat.Encoding
*   AudioInputStream
*   AudioPermission
*   AudioSystem
*   BooleanControl
*   BooleanControl.Type
*   CompoundControl
*   CompoundControl.Type
*   Control
*   Control.Type
*   DataLine.Info
*   EnumControl
*   EnumControl.Type
*   FloatControl
*   FloatControl.Type
*   Line.Info
*   LineEvent
*   LineEvent.Type
*   Mixer.Info
*   Port.Info
*   ReverbType

Eccezioni
---------

*   LineUnavailableException
*   UnsupportedAudioFileException

Vediamo come usare questo pacchetto solo per suonare un file sonoro, ad esempio il file chiamato italian.wav.

Inanzitutto dobbiamo reperire il file, e questo lo facciamo come al solito instanziando un oggetto della classe File (di java.io)

File sf=new File(“italian.wav”);

A questo punto iniziamo una try{} catch, nella quale includeremo tutto quello che ha a che fare con il suono, cattureremo le eccezioni:

catch(UnsupportedAudioFileException ee){}  
catch(IOException ea){}  
catch(LineUnavailableException LUE){};

Per usare il motore sonoro di Java dobbiamo partire dalla classe AudioSystem, da questa otterremo due oggetti di tipo AudioFileFormat e AudioInputStream

AudioFileFormat aff=AudioSystem.getAudioFileFormat(sf);  
AudioInputStream ais=AudioSystem.getAudioInputStream(sf);

che rappresentano il contenuto del file italian.wav

Dobbiamo creare un oggetto AudioFormat da AudioFileFormat per poterlo suonare

AudioFormat af=aff.getFormat();

Il nostro scopo è quello di creare un oggetto che implementi l’interfaccia Clip, il quale contiene i metodi play, per fare questo scriveremo:

DataLine.Info info = new DataLine.Info(  
Clip.class,  
ais.getFormat(),  
((int) ais.getFrameLength() *  
af.getFramesize()));  
Clip ol = (Clip) AudioSystem.getLine(info);

Da ol, che è di tipo Clip possiamo suonare, e questo si fa aprendo la linea e invocando i metodi per suonare:

ol.open(ais); <-(APRE l’input stream, ovvero il file italian.wav)

e quindi

ol.loop(NUMERO);

dove NUMERO indica il numero di ripetizioni del dile, nel nostro caso è Clip. LOOP_CONTINUOUSLY per indicare che bisogna ripetere all’infinito.

L’esempio completo, editato in suono.java è il seguente:

import javax.swing.*;  
import javax.sound.sampled.*;  
import java.io.*;  
public class suono extends JFrame  
{  
public suono()  
{  
File sf=new File(“italian.wav”);  
AudioFileFormat aff;  
AudioInputStream ais;  
try  
{  
aff=AudioSystem.getAudioFileFormat(sf);  
ais=AudioSystem.getAudioInputStream(sf);  
AudioFormat af=aff.getFormat();  
DataLine.Info info = new DataLine.Info(  
Clip.class,  
ais.getFormat(),  
((int) ais.getFrameLength() *  
af.getFramesize()));  
Clip ol = (Clip) AudioSystem.getLine(info);  
ol.open(ais);  
ol.loop(Clip.LOOP_CONTINUOUSLY);  
System.out.println(“Riproduzione iniziata, premere CtrL-C per interrompere”);  
}  
catch(UnsupportedAudioFileException ee){}  
catch(IOException ea){}  
catch(LineUnavailableException LUE){};  
}  
public static void main(String\[\] ar)  
{  
new suono();  
}  
}

Sembra un po macchinoso, ma seguendo questa procedura è possiblie leggere e suonare ogni file wav. Per effettuare altre operazioni il pacchetto è abbastanza complesso, e oggi non esistono manuali che ne spiegano il funzionamento (tranne la documentazione del JDK, che come avrete avuto modo di vedere non spiega come usare le cose, ma le descrive soltanto).