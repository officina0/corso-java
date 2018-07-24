# 131-Il pacchetto javax.suond.midi

Vediamo come leggere e suonare un file midi, per fare questo ci occorre il pacchetto javax.sound.midi, il quale contiene:

## Interfacce

* ControllerEventListener
* MetaEventListener
* MidiChannel
* MidiDevice
* Receiver
* Sequencer
* Soundbank
* Synthesizer
* transmitter

## Classi

* Instrument
* MetaMessage
* MidiDevice.Info
* MidiEvent
* MidiFileFormat
* MidiMessage
* MidiSystem
* Patch
* Sequence
* Sequencer.SyncMode
* ShortMessage
* SoundbankResource
* SysexMessage
* track
* VoiceStatus

## Eccezioni

* InvalidMidiDataException
* MidiUnavailableException

La tecnica per suonare un file midi è simile a quella usata per i file wav, solo che si parte da MidiSystem.

Nel seguente esempio viene suonato il file sorpresa.mid, che è un file midi che io apprezzo molto, e credo che apprezzaranno tutti i miei coetanei.  
Editatelo nel file midifiles.java

import javax.swing._;  
import javax.sound.midi._;  
import java.io.\*;  
public class midifiles extends JFrame  
{  
public midifiles\(\)  
{  
try  
{  
File f2=new File\(“sorpresa.mid”\);  
MidiFileFormat mff2=MidiSystem.getMidiFileFormat\(f2\);  
Sequence S=MidiSystem.getSequence\(f2\);  
Sequencer seq=MidiSystem.getSequencer\(\);  
seq.open\(\);  
seq.setSequence\(S\);  
seq.start\(\);  
System.out.println\(“Sorpresa per tutti i miei coetanei”\);  
System.out.println\(“Premere CtrL-C per interrompere”\);

}  
catch\(MidiUnavailableException ecc\){}  
catch\(InvalidMidiDataException ecc2\){}  
catch\(IOException ecc3\){}  
;  
}

public static void main\(String\[\] ar\)  
{  
new midifiles\(\);  
}  
}

