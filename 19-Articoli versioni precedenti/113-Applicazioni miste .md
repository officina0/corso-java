A questo punto, dopo avere visto cosa sono i Frame e cosa sono gli Applet, ci viene in mente una domanda, ma sarà possibile combinare le due tecniche, se le vogliamo chamare così, per creare delle applicazioni miste?  
La risposta è ovviamente si. Possiamo fare dei programmi Java che sono dei misti tra applicazione e applets. In effetti data la modularità del linguaggio possiamo creare delle appliczioni Java che usano altre applicazioni Java, cosa che non accade nei normali linguaggi di programmazione, ove è solo possibile invocare da un programma altri programmi.  
Supponiamo ad esempio di avere creato un programma Java chiamato Calcolatrice.java, con il suo main e i suoi metodi, tra cui uno che prende due interi e ne restituisce la somma.  
Supponiamo a questo punto di averlo compilato in Calcolatrice.class e di averlo anche eseguito.  
Infine facciamo un altro programma di qualunque genere, in questo noi possiamo usare il programma Calcolatrice.class, ovvero la classe Calcolatrice, e tutte le sue funzioni pubbliche, tra cui quella della somma.

Tornando al discorso degli applet che chiamano dei frame, quardate questo esempio:

import java.awt.*;  
import java.applet.*;  
  
public class Mista extends Applet  
{  
Frame F;  
public void init()  
{  
F = new Frame(“Ciao, questo è un Frame”);  
F.setSize(100,50);  
F.show();  
}  
public void paint(Graphics g)  
{  
g.drawString(“Io sono un’applet”,10,20);  
new p();  
}  
public void destroy()  
{  
String \[\] param={“”};  
Finestra.main(param);  
}  
}  
class p extends Frame  
{  
p()  
{  
setTitle(“Un’altro Frame lanciato dall’applet”);  
setSize(250,100);  
setLocation(200,300);  
show();  
new Dialog(this,”Questa è una dialog”);  
}  
public void paint(Graphics g)  
{  
g.drawString(“Potrei essere un banner pubblicitario”,10,40);  
}  
}  

  

lo metterete in un file chiamato Mista.java, e lo lancerede creando un file Mista.html contenente:

<html>  
<head>  
<title>mista</title>  
</head>  
<body>  
Il seguente è un applet:<BR>  
<applet code=”Mista.class” width=200 height=100>Il tuo browser è vecchio, cambialo!</APPLET>  
</body>  
</html>

  
  

Lanciatelo e vedete cosa succede, ogni volta che l’applet passa in secondo piano, ovvero ci piazzate un’altra finestra sopra, e poi ritorna in promo piano.