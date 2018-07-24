# 125 Disegno

Capito il funzionamento della paint\(\) siamo pronti a disegnare qualsiasi cosa su un’applet o su un generico componente.  
La cosa più semplice da disegnare è la linea, per farlo usiamo il metodo drawLine\(\) di Graphics:

drawLine\(Iniziox, Inizioy, Finex, Finey\)

il quale disegna una linea che parte dal punto \(Iniziox, Inizioy\) e arriva al punto \(Finex, Finey\)

Prima di usare la drawLine\(\) capiamo come si possono cambiare i colori delle cose disegnate.  
I colori si cambiano usando il metodo setColor\(Color c\) della classe Graphics.  
Color è un’altra classe di awt che permette di definire un colore, alcuni suoi costruttori sono:

Color\(float r, float g, float b\), crea un colore specificando i valori RGB \(Red – Green – Blue, ovvero Rosso – Verde – Blu\) con dei valori compresi tra 0 e 1. Color\(float r, float g, float b, float a\), come il precedente, solo che permette di definire anche il valore alfa Color\(int r, int g, int b\) , crea un colore specificando i valori RGB \(Red – Green – Blue, ovvero Rosso – Verde – Blu\) con dei valori compresi tra 0 e 255. Color\(int r, int g, int b, int a\), come il precedente, solo che permette di definire anche il valore alfa

Alcuni colori semplici sono dati da delle costanti nella classe color:

Color.black  
Color.blue  
Color.cyan  
Color.darkGray  
Color.gray  
Color.green  
Color.lightGray  
Color.magenta  
Color.orange  
Color.pink  
Color.red  
Color.white  
Color.yellow

Nel seguente applet si disegnano alcune linee colorate.

import java.awt.Graphics; import java.awt.Color; import java.awt.Button; import java.awt.borderLayout; import java.awt.GridLayout; import java.awt.Panel; import java.awt.event.ActionEvent; import java.awt.event.ActionListener; import java.applet.\*; public class linee extends Applet { final int SI=14; final int NO=0; Button R=new Button\("Rosso"\); Button G=new Button\("Verde"\); Button B=new Button\("Blu"\); int r_=0; int g_=0; int b_=0; int ir=0; int ig=0; int ib=0; public void init\(\) { setLayout\(new borderLayout\(\)\); Panel np=new Panel\(new GridLayout\(1,3\)\); np.add\(R\); np.add\(G\); np.add\(B\); R.addActionListener\(new ActionListener\(\) { public void actionPerformed\(ActionEvent e\) { ir=SI; ig=NO; ib=NO; repaint\(\); } } \); G.addActionListener\(new ActionListener\(\) { public void actionPerformed\(ActionEvent e\) { ir=NO; ig=SI; ib=NO; repaint\(\); } } \); B.addActionListener\(new ActionListener\(\) { public void actionPerformed\(ActionEvent e\) { ir=NO; ig=NO; ib=SI; repaint\(\); } } \); add\(np,borderLayout.SOUTH\); } public void paint\(Graphics g\) { for \(int i=0;i&lt;200;i+=10\) { g.setColor\(new Color\(r_,g_,b_\)\); r_=r_+ir; g_=g_+ig; b_=b_+ib; g.drawLine\(0,i,i,200\); } g.setColor\(Color.black\); g.drawLine\(0,0,0,200\); g.drawLine\(0,200,200,200\); r_=0; g_=0; b\_=0; } }

Chiamiamo il file linee.java e lo carichiamo con il file html seguente \(linee.html\):

Applet Linee \(linee.class\)

