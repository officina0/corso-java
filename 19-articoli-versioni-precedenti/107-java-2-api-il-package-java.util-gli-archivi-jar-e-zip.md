# 107-Java 2 API: il package java.util, gli archivi jar e zip

In questa lezione vedremo la parte di java.util che tratta gli utilissimi file .zip e i .jar  
Iniziamo a veder java.util.zip  
I file .zip sono degli archivi che contengono dei file compressi, essi sono molto usati per scambiare dati in internet perché ne riducono anche notevolmante le dimensioni.  
Esistono vari tipi di file compressi, e vari programmi per comprimere e decomprimere i dati, si pensi agli archivi RAR, ai CAB di Windows, agli ARJ. Questo package ci da la possibilità di trattare dati compressi secondo gli standard ZIP e GZIP, che usano l’algoritmo di compressione chiamato DEFLATE, in questo package troviamo anche utility per controllare i codici checksum CRC-32 e Adler-32 di un arbitrario stream di ingresso.  
Perché usare questo package, i motivi sono tanti, innanzitutto i programmi Java che includono immagini, animazioni e suoni possono essere veramente molto grandi, ed è possibile creare un file .ZIP con tutti i file necessari al funzionamento del programma, in modo da ridurne le dimensioni, però così facendo per essere utilizzati dal programma devono essere prima scompattati, questo package da questa possibilità. Un ‘ altro semplice motivo per usare questo package è che la compressione di dati in informatica è un problema abbastanza complesso, vengono usati degli algoritmi che si basano sull’algebra dei gruppi, materia non molto conosciuta al di fuori delle università scientifiche, sono quindi degli algoritmi abbastanza incomprensibili a chi non è “del mestiere”, questo package da la possibilità a chiunque di comprimere e decomprimere questi dati.  
Vediamo quindi cosa contiene il package java.util.zip

## Interfacce

Checksum

## Classi

CheckedInputStream CheckedOutputStream CRC32 Deflater ZLIB.  
DeflaterOutputStream GZIPInputStream GZIP.  
GZIPOutputStream Inflater InflaterInputStream l’algoritmo Deflate.  
ZipEntry ZipFile ZipInputStream ZipOutputStream

## Eccezioni

DataFormatException, errore di formato dei dati.  
ZipException

Ognuna di queste classi avrà i suoi metodi, per conoscerli tutti vi rimando alla documentazione del JDK, noi ne vediamo alcuni in un piccolo esempio.  
Il seguente programma apre la directory in cui si trova e cerca tutti i file .zip, per ogni file trovato ne va vedere il contenuto.

import java.util._;  
import java.util.zip._;  
import java.io.\*;

public class ReadZip  
{  
public static void main\(String \[\] a\)  
{  
File dir=new File\(“.”\);  
System.out.println\(“Apro la directory “+dir.getAbsolutePath\(\)\);  
File\[\] cont=dir.listFiles\(\);  
int MAX=cont.length;  
for \(int i = 0; i&lt;MAX; i++\)  
{  
String tmp=cont\[i\].getName\(\);  
if \(\(tmp.endsWith\(“.zip”\)\)\|\|\(tmp.endsWith\(“.ZIP”\)\)\)  
{  
// è un file .zip  
System.out.println\(“Ho trovato “+tmp\);  
controllaZip\(cont\[i\]\);  
};  
}  
}  
public static void controllaZip\(File f\)  
{  
System.out.println\(“Contenuto del file “+f.getName\(\)\);  
ZipFile Zf;  
try {Zf=new ZipFile\(f\);}  
catch \(ZipException e\){Zf=null;}  
catch \(IOException e1\){Zf=null;}  
;  
Enumeration files=Zf.entries\(\);  
while\(files.hasMoreElements\(\)\)  
System.out.println\(files.nextElement\(\)\);  
}  
}

Possiamo anche decomprimere questi files,il seguente programma prende tutti i .zip della directory dove viene eseguito e li decomprime.

import java.util._;  
import java.util.zip._;  
import java.io.\*;

public class Decomp  
{  
public static void main\(String \[\] a\)  
{  
File dir=new File\(“.”\);  
System.out.println\(“Apro la directory “+dir.getAbsolutePath\(\)\);  
File\[\] cont=dir.listFiles\(\);  
int MAX=cont.length;  
for \(int i = 0; i&lt;MAX; i++\)  
{  
String tmp=cont\[i\].getName\(\);  
if \(\(tmp.endsWith\(“.zip”\)\)\|\|\(tmp.endsWith\(“.ZIP”\)\)\)  
{  
// è un file .zip  
System.out.println\(“Ho trovato “+tmp\);  
try {controllaZip\(cont\[i\]\);}  
catch \(IOException e\){};  
};  
}  
}  
public static void controllaZip\(File f\) throws IOException  
{  
System.out.println\(“Decompressione del file “+f.getName\(\)+”:”\);  
ZipFile Zf;  
try {Zf=new ZipFile\(f\);}  
catch \(ZipException e\){Zf=null;}  
catch \(IOException e1\){Zf=null;}  
;  
Enumeration files=Zf.entries\(\);  
while\(files.hasMoreElements\(\)\)  
{  
ZipEntry tmpFile=\(ZipEntry \) files.nextElement\(\);  
System.out.println\(“decomprimo il file “+tmpFile.getName\(\)\);  
System.out.println\(“dimensione compresso “+  
tmpFile.getCompressedsize\(\)+” dimensione non compresso “+  
tmpFile.getsize\(\)+” CRC “+tmpFile.getCrc\(\)\);  
System.out.println\(“modificato “+tmpFile.getTime\(\)\);  
InputStream in= Zf.getInputStream\(tmpFile\);  
FileOutputStream out= new FileOutputStream\(tmpFile.getName\(\)\);

for \(int ch=in.read\(\);ch!=-1;ch=in.read\(\)\) out.write\(ch\);

out.close\(\);  
in.close\(\);  
}  
}  
}

Il package java.util.jar mette a disposizione del programmatore delle classi e interfacce per trattare i file di tipo Java Archive \(JAR\), in particolare è possibile leggerli, e scriverli. I file JAR sono basati sullo standard ZIP, con un file opzionale detto manifest .  
Il contenuto del package è il seguente:

Classi  
Attributes  
Attributes.Name  
JarEntry  
JarFile  
JarInputStream  
JarOutputStream  
Manifest

Eccezion  
JarExceptioni

Rimando alla documentazione del JDK per ulteriori informazioni.

