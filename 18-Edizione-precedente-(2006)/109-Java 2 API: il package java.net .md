Il Package java.net
-------------------

Java, come già detto in precedenza è nato come linguaggio per la rete, solo successivamente è divenuto un vero e proprio linguaggio di programmazione.  
Il suo ruolo di leader per la programmazione di rete è comunque indiscusso, come tale esso mette a disposizione del programmatore tutto vari package per effettuare tale programmazione.  
Visto che lo scopo finale del corso è quello di programmare Applet, una infarinatura del package la dobbiamo avere.  
Il package è molto ampio, il suo contenuto è il seguente:

Interfacce
----------

ContentHandlerFactory  
FileNameMap  
SocketImplFactory  
SocketOptions  
URLStreamHandlerFactory

Classi
------

Authenticator  
ContentHandler  
DatagramPacket  
DatagramSocket  
DatagramSocketImpl  
HttpURLConnection  
InetAddress  
JarURLConnection  
MulticastSocket  
NetPermission  
PasswordAuthentication  
ServerSocket  
Socket  
SocketImpl  
SocketPermission  
URL  
URLClassLoader  
URLConnection  
URLDecoder  
URLEncoder  
URLStreamHandler

Eccezioni
---------

BindException  
ConnectException  
MalformedURLException  
NoRouteToHostException  
ProtocolException  
SocketException  
UnknownHostException  
UnknownServiceException

Di queste noi ne vedremo solo alcune, iniziamo intanto a vedere cosa succede nella rete.  
I computer in Internet comunicano scambiandosi pacchetti di dati, chiamati pacchetti IP (Internet Protocol), questi pacchetti partono da un computer, attraversano vari nodi della rete (Server di rete) e arrivano a destinazione, per stabilire il percorso intermedio tra i due computer che vogliono comunicare si esegue un algoritmo di routing (ne esistono di vari tipi, a seconda delle esigenze e a seconda del tipo di rete).  
Per stabilire il mittente di una comunicazione, i destinatari, i nodi intermedi occorre che ogni computer collegato in rete abbia un nome che lo identifica univocamente, questo nome, è un numero, e si chiama indirizzo IP.  
L’indirizzo IP è un numero di 32 bit, che può essere scritto in vari formati, ma il più usato è il formato decimale separato da punti, un indirizzo IP è ad esempio 594. 24.114.462 (Il numero è un numero a caso).  
Siccome i computer ricordano bene i numeri, ma noi umani no, sono stati inventati i DNS (Domain Name System) che associano dei nomi a questi indirizzi IP.  
La prima classe che vediamo del package è la classe InetAddres, la quale gestisce questi indirizzi IP.  
La classe ha vai metodi, tra i quali uno che restituisce gli indirizzi Ip del computer sul quale si lavora, uno che dato un nome di dominio ne da l’indirizzo IP.  
Il seguente esempio ne mostra l’uso.

import java.net.*;  
public class IndirizziIP  
{  
public static void main(String [] a)  
{  
String dom=”developer.java.sun.com”;  
try {  
InetAddress loc=InetAddress.getByName(dom);  
System.out.println(“IP di “+dom+” : “+loc.getHostAddress());  
}  
catch (UnknownHostException e)  
{System.out.println(“Non esiste il dominio “+dom);};

try {  
InetAddress loc=InetAddress.getLocalHost();  
System.out.println(“IP locale: “+loc.getHostAddress());  
System.out.println(“Nome locale”+loc.getHostName());  
}  
catch (UnknownHostException e)  
{};  
}  
}

Come esercizio provare a far prendere come nome di dominio il primo argomento del programma, ad esempio  
java IndirizziIP html.it

Il package fornisce utili classi per trattare i socket fondamentali basati su TCP e su UDP, i quali sono i protocolli impiegati nella maggior parte delle applicazioni Internet. Noi queste non le vedremo, ma passiamo direttamente a classi che gestiscono applicazioni Web di livello più alto.  
Vediamo la classe URL.  
Che cos’è un URL? Un URL, o Uniform Resource Locator, è un puntatore ad una risorsa web.  
Un arisorsa web può essere qualsiasi cosa, una directory, un file, un oggetto in rete come ad esempio una interfaccia per fare delle query ad un database remoto, o per un motore di ricerca.  
Gli URL sono svariati, ognuno con un protocollo diverso, ma i più usati sono quelli che usano protocolli HTTP (HyperText transfer Protocol) e FTP (File transfer Protocol).  
La classe URL incapsula degli oggetti web, accedendovi tramite il loro indirzzo URL.

A differenza dei web browser, l’http:// davanti all’indirizzo è indispensabile, perché individua il protocollo dell’URL, protocollo che il Navigator e l’Explorer intuiscono anche se omesso.

Quando programmeremo gli applet useremo solo gli URL per accedere anche ai file locali, vedendoli come risorse di rete. Questo lo faremo perché negli applet è vietato usare i file, per motivi di sicurezza di rete, quindi per leggere un file bisogna vederlo come una risorsa di rete.  
Le ultime versioni di Java stanno eliminando questa limitazione del linguaggio, usando delle firme, che rendono la lettura e la scrittura di file controllata anche sulla rete, quindi in futuro sarà possibile usare anche i file negli applet, a patto che ci si assuma qualche responsabilità.  
Vediamo alcuni costruttori di oggetti URL:

URL(String spec) , crea ul oggetto URL in base alla rappresentazione a stringa, ad esempio  
“html.it”  
URL(String protocol, String host, int port, String file), crea un oggetto URL, specificando tutto,  
anche la porta.  
URL(String protocol, String host, int port, String file, URLStreamHandler handler), come il  
precedente, solo che specifica anche l’URLStreamHandler, il quale è la superclasse comune per tutti i protocolli(HTTP,FTP,Gopher).  
URL(String protocol, String host, String file), crea un oggetto URL specificando protocollo, host e File sul server host, ad esempio: URL(“http”,”www.cli.di.unipi.it”,”~castellu/index.html”).

Discutiamo ora alcuni metodi della classe

boolean equals(Object obj), confronta due oggetti URL.  
Object getContent(), da il contenuto dell’oggetto URL.  
String getFile(), da il filename dell’URL.  
String getHost(), l’host  
int getPort(), il numero della porta  
String getProtocol(), il nome del protocollo.  
String getref(), da il puntatore all’URL.  
int hashCode(), da il codica hash dell’oggetto.  
URLConnection openConnection(), apre una connessione con l’oggetto remoto indicato dall’URL.  
InputStream openStream(), apre una connessione con l’oggetto web indicato dall’url sotto forma di stream in lettura.  
String toExternalForm(), da una stringa rappresentante l’URL.  
String toString(), da una rappresentazione dell’oggetto URL.

Ecco di seguito un piccolo esempio sull’uso della classe URL.

import java.net.*;  
import java.io.*;  
public class getPage  
{  
public static void main(String[] arg)  
{  
String un;  
try {un=arg[0];}  
catch (ArrayIndexOutOfBoundsException e)  
{  
un=”http://www.html.it”;  
System.out.println(“Nessun URL definito, prendo “+un);  
};  
System.out.println(“URL:”+un);  
URL url;  
boolean pippo=false;  
try {url= new URL(un);}  
catch (MalformedURLException e)  
{  
System.out.println(“URL errato, prendo http://www.html.it “);  
url = null;  
pippo=true;  
};  
if (pippo) try {url = new URL (“http://www.html.it “);}  
catch (MalformedURLException e){};  
BufferedReader stream;  
try {stream = new BufferedReader (new InputStreamReader (url.openStream()));}  
catch (IOException e){  
System.out.println(“Errore di apertura del file”);  
stream=null;  
System.exit(0);  
};  
File out=new File(“.”+url.getFile());  
FileWriter Output;  
try {Output=new FileWriter(out);}  
catch (IOException e) {Output=null;};  
String l;  
try  
{  
while ((l=stream.readLine())!=null)  
{

Output.write(l);  
};  
Output.flush();  
Output.close();  
}  
catch (IOException e){System.out.println(“Errore di lettura.”);};  
}  
}

Il programma prende un url dai parametri d’ingresso, se non è valido o non ci sono parametri apre per default l’url http://html.it , preleva la pagina letta e la salva su disco.  
Come vedete questo è una specie di web browser, basterebbe un po di grafica e si potrebbe anche visualizzare la pagina.  
Provate ad eseguire il programma mettendo i seguenti indirizzi:

java getPage http://www.cli.di.unipi.it/~castellu/index.htm  
java getPage http://www.cli.di.unipi.it/~castellu/pietro1.htm  
java getPage http://www.cli.di.unipi.it/~castellu/chisono.htm

e semplicemente java getPage, e vedete i risultati.