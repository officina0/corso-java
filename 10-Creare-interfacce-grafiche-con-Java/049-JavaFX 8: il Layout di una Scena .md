La progettazione di una interfaccia grafica, o **Scena** in linea con la terminologia JavaFX, inizia con l’organizzazione dei componenti al suo interno, il **Layout** di una Scena. JavaFX fornisce dei Layout predefiniti ed in  
questo capitolo cercheremo di presentare i più comuni.

HBox e VBox
-----------

Iniziamo con il  
dire che la classe `javafx.layout.Pane` rappresenta la classe  
base dalla quale derivano tutte le classi di Layout, `HBox` è, ad esempio, ci consente di posizionare i nodi della Scena su una singola fila  
orizzontale:

```
....
         HBox hBox = new HBox();
         hBox.setSpacing(20);
         hBox.getChildren().add(new Label("Label1"));
         hBox.getChildren().add(new Label("Label2"));
         hBox.getChildren().add(new Label("Label3"));
         Scene scene = new Scene(hbox);
         stage.setScene(scene);
         stage.show();
         ....
```

Eseguendo il codice all’interno del metodo `start()` di una applicazione JavaFX, vedremo  
le tre label disposte orizzontalmente e distanziate di 20 px.

Un Layout  
simile al precedente, ma che organizza i nodi verticalmente, è `VBox`, la modifica del codice  
precedente con `VBox` al posto di `HBox` consente di verificarne il funzionamento.

BorderPane e StackPane
----------------------

Un Layout particolarmente utile è il `BorderPane` che definisce  
le posizioni superiore, sinistra, destra, inferiore e centrale all’interno della sua area; nelle  
posizioni di un `BorderPane` è possibile collocare nodi che possono contenere a loro volta  
altri Layout:

```
....
         BorderPane borderPane = new BorderPane();
         borderPane.setTop(new Label("Top"));
         borderPane.setLeft(new Label("Left"));
         borderPane.setCenter(new Label("Center"));
         borderPane.setRight(new Label("Right"));
         borderPane.setBottom(new Label("Bottom"));
         Scene scene = new Scene(borderPane);
         stage.setScene(scene);
         stage.show();
         .....
```

Uno `StackPane` consente invece di disporre i nodi uno sopra l’altro come in una  
pila. Il nodo aggiunto per primo viene posizionato sul fondo della pila e il nodo successivo  
al di sopra di essa, si prosegue quindi con l’aggiunta di altri nodi. L’utilizzo di `StackPane` segue  
la falsa riga di quelli precedenti risultando quindi abbastanza semplice.

AnchorPane
----------

Un `AnchorPane` consente di ancorare i nodi al suo interno  
specificando le distanze dal contorno. Cosi, ad esempio, possiamo collocare  
un pulsante ad una distanza di 10 px dal bordo destro dell’`AnchorPane` e una label  
a 20 px dal bordo sinistro e di 10 px dal bordo superiore.

Vediamo un esempio e collochiamo 4 nodi in un `AnchorPane`. Il primo nodo sia una label  
distanziata dal bordo sinistro e dal bordo superiore, la seconda una label collocata specificando distanza da bordo destro  
e superiore. Aggiungiamo poi due aree di testo con le stesse distanze  
dai bordi laterali e leggermente più in basso rispetto al bordo superiore:

```
AnchorPane anchorPane = new AnchorPane();
         Label label1 = new Label("Label1");
         Label label2 = new Label("Label2");
         TextField text1 = new TextField("Text1");
         TextField text2 = new TextField("Text2");
         AnchorPane.setLeftAnchor(label1,20d);
         AnchorPane.setTopAnchor(label1,10d);
         AnchorPane.setLeftAnchor(text1,20d);
         AnchorPane.setTopAnchor(text1,40d);
         AnchorPane.setRightAnchor(label2,165d);
         AnchorPane.setTopAnchor(label2,10d);
         AnchorPane.setRightAnchor(text2,50d);
         AnchorPane.setTopAnchor(text2,40d);
         anchorPane.setPrefSize(400, 100);
         anchorPane.getChildren().addAll(label1, label2, text1, text2);
         Scene scene = new Scene(anchorPane);
         stage.setScene(scene);
         stage.setResizable(false);
         stage.show();
```

Ecco il form risultante:

Figura 1. AnchorPane

![AnchorPane](https://tbm-html.s3.amazonaws.com/app/uploads/2017/04/anchorpane.png)

GridPane
--------

Il layout `GridPane` dispone i nodi della Scena in una griglia di righe e colonne  
in modo simile ad una tabella HTML. Gli elementi della griglia sono numerati  
con indici di riga e colonna che iniziano dal valore 0. Alcune proprietà ne consentono la personalizzazione attraverso i metodi della classe:

Proprietà

Descrizione

**alignment**

Allineamento di un nodo in una cella (Top, Left, Right, Center).

**hgap**

Distanza orizzontale tra colonne.

**vgap**

Distanza verticale tra righe.

TilePane
--------

Un altro interessante Layout è `TilePane`. I nodi  
aggiunti sono disposti in forma di piastrelle (_tiles_) di dimensioni uniformi. Proprietà fondamentali  
sono:

Proprietà

Descrizione

**alignment**

Allineamento di un nodo in una _tile_.

**hgap**

Distanza orizzontale tra _tiles_.

**vgap**

Distanza verticale tra _tiles_.

**orientation**

modalità con cui aggiungere _tiles_ (orizzontale, da sinistra a  
destra o verticale, dall’alto al basso).

Vediamo un esempio:

```
....
      TilePane tilePane = new TilePane() ;
      tilePane.setOrientation(Orientation.HORIZONTAL) ;
      tilePane.setTileAlignment(Pos.CENTER_LEFT) ;
      tilePane.setHgap(10);
      tilePane.setVgap(10);
      tilePane.getChildren().addAll(new Label("Label1"), new Label("Label2"),
               new Label("Label3"), new Label("Label4"),
               new Label("Label5"), new TextField("Text1"),
               new TextField("Text2"), new TextField("Text3"),
               new TextField("Text4"), new TextField("Text5"));
       ....
```

Risultato:

Figura 2. TilePane

![TilePane](https://tbm-html.s3.amazonaws.com/app/uploads/2017/04/tilepane.png)