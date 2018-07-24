# 047 Un Primo Programma Con Java FX

In JavaFX uno **Stage** rappresenta l’intera finestra applicativa, mentre una Scena, costruita attraverso uno Scene Graph \(struttura grafo\), è l’effettiva interfaccia con cui l’utente interagisce. Ogni elemento della Scena è denominato nodo, un nodo è un oggetto grafico e può includere :

* oggetti 2D o 3D;
* controlli UI \(pulsanti, aree di testo, label;
* pannelli di layout per organizzare i contenuti;
* elementi media \(audio, video, immagini\).

Ciascun nodo della Scena ha un solo genitore e il nodo che non ha genitore è noto come **nodo root**; ogni nodo ha uno o più figli e un nodo senza figli è definito come **leaf node**. Ai nodi di una Scena possono essere aggiunti degli effetti grafici, utilizzando JavaFX è anche possibile incorporare HTML in una Scena. Il componente **WebView** di JavaFX aggiungere questa funzionalità e utilizza il motore di rendering Web Kit che offre il supporto a HTML5, CSS, JavaScript, DOM ed SVG.

Con WebView, è quindi possibile:

* il rendering HTML;
* gestire lo storico e la navigazione “Avanti” e “Indietro”;
* applicare effetti;
* mkodificare l’HTML;
* eseguire JavaScript;
* gestire eventi.

Per la realizzazione di un applicativo di conversione gradi – radianti. Abbiamo bisogno di una classe che estenda `javafx.application.Application` e che contenga il metodo `main` con istruzione `launch()` per lanciare l’applicativo:

```text
     public class GradiRadiantiJavaFX extends Application {

      public static void main(String\[\] args) {
         launch(args);
      }

      @Override
      public void start(Stage primaryStage) {
        .....
      }
```

Proseguiamo con la definizione di uno Scena nel metodo `start()` e notiamo come esso contenga un parametro Stage quale riferimento alla finestra contenitrice. Definiamo un titolo per la finestra ed inseriamo dei pannelli per aree di testo, label e pulsanti:

```text
    primaryStage.setTitle("Convertitore da gradi a radianti");

    BorderPane borderPane = new BorderPane();
    borderPane.setPadding(new Insets(10,50,50,50));

    HBox horizontalBox = new HBox();
    horizontalBox.setPadding(new Insets(20,20,20,30));

    GridPane gridPane = new GridPane();
    gridPane.setPadding(new Insets(20,20,20,20));
    gridPane.setHgap(5);
    gridPane.setVgap(5);
```

I pannelli di JavaFX ci consentono di disporre opportunamente i vari controlli all’interno della Scena, proseguiamo quindi con la costruzione dell’interfaccia aggiungendo un pulsante e due aree di testo per i gradi e i radianti con le label per l’utente:

```text
    Label lblGradi = new Label("Gradi");
    TextField txtGradi = new TextField();
    Label lblRadianti = new Label("Radianti");
    TextField txtRadianti = new TextField();   
    Button btnCalcola = new Button("Calcola");
```

Colleghiamo questi nodi al nodo padre `gridPane`:

```text
    gridPane.add(lblGradi, 0, 0);
    gridPane.add(txtGradi, 1, 0);
    gridPane.add(lblRadianti, 0, 1);
    gridPane.add(txtRadianti, 1, 1);
    gridPane.add(btnCalcola, 2, 1); 
```

Aggiungiamo degli effetti decorativi di riflessione per `gridPane` e di ombra per un titolo:

```text
    Reflection reflection = new Reflection();
    reflection.setFraction(0.7f);
    gridPane.setEffect(reflection);

    DropShadow dropShadow = new DropShadow();
    dropShadow.setOffsetX(3);
    dropShadow.setOffsetY(3);

    Text text = new Text("Convertitore da gradi a radianti");
    text.setFont(Font.font("Arial", FontWeight.BOLD, 28));
    text.setEffect(dropShadow);

    horizontalBox.getChildren().add(text);
```

Prima di proseguire definiamo un foglio di stile CSS \(`form.css`\) per applicare effetti grafici a pannelli, aree di testo e pulsante:

```text
        #gridPane {
         -fx-background-color:  linear-gradient(lightgray, gray);
         -fx-border-color: white;
         -fx-border-radius: 20;
         -fx-padding: 10 10 10 10;
         -fx-background-radius: 20;

        }
        #borderPane {
         -fx-background-color:  linear-gradient(gray, DimGrey );

        }
        #button {
            -fx-background-radius: 30, 30, 29, 28;
            -fx-padding: 3px 10px 3px 10px;
            -fx-background-color: linear-gradient(orange, orangered );
        }
        #text {
         -fx-fill:  linear-gradient(orange , orangered);
        }
```

Colleghiamo le regole CSS ai controlli della Scena:

```text
    borderPane.setId("borderPane");
    gridPane.setId("gridPane");
    btnCalcola.setId("button");
    text.setId("text");
```

Aggiungiamo con un’espressione Lambda il codice di conversione attivato dal click del pulsante:

```text
   btnCalcola.setOnAction((ActionEvent event) -> {
        event.consume();
        float gradi = Float.parseFloat(txtGradi.getText());
        float radianti = gradi * 3.14f / 180;
        txtRadianti.setText(String.valueOf(radianti));
    });
```

E completiamo la Scena caricando il foglio di stile:

```text
 borderPane.setTop(horizontalBox);
 borderPane.setCenter(gridPane);  

 Scene scene = new Scene(borderPane);
 scene.getStylesheets().add(getClass().getClassLoader().
 getResource("form.css").toExternalForm());

 primaryStage.setScene(scene);

 primaryStage.setResizable(false);
 primaryStage.show();
```

In allegato potete trovare il codice completo con le importazioni e il CSS della cartella `Source` del progetto NetBeans.

Possiamo finalmente lanciare il programma e visualizzare la nostra prima applicazione JavaFX 8:

Figura 1. Convertitore gradi – radianti

![Convertitore gradi-radianti](http://www.html.it/wp-content/uploads/2017/03/convertitore.png)

Per mantenere il programma leggibile si è evitata l’implementazione di controlli sul corretto formato dei dati di input nelle aree di testo. Aggiungeremo questa parte nei successivi capitoli.

