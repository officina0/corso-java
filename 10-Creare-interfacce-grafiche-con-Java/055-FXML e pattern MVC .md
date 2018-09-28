File e classi
-------------

Nel capitolo precedente abbiamo visto come realizzare una GUI JavaFX facendo uso del linguaggio FXML ed  
inserendo al suo interno codice Javascript per la gestione dell’evento `click` sul pulsante. Vediamo ora  
come realizzare la stessa applicazione attraverso il **pattern MVC** creando con FXML la sola interfaccia grafica e  
lasciando ad una classe Controller Java, collegata alla GUI FXML, il compito di gestire gli eventi e la logica applicativa.

Iniziamo con il creare un nuovo file FXML, `Form2.fxml`, nel package di default del progetto `NetBeans`.  
Il file è ottenibile dal `Form1.fxml` del precedente capitolo eliminando il tag `fx:script`, contente il codice Javascript, e  
modificando il tag `BorderPane` in modo da specificare la classe Controller:

```
<BorderPane id="borderPane" xmlns:fx="http://javafx.com/fxml"
            fx:controller="helloworldjavafx.GradiRadiantiController">
            .....
         </BorderPane>
```

Realizziamo quindi la classe `GradiRadiantiController` definendola nel package `helloworldjavafx`. Un Controller deve implementare  
l’interfaccia `javafx.fxml.Initializable` che richiede l’implementazione del metodo:

```
public void initialize(java.net.URL arg0, ResourceBundle arg1){...}
```

Prima di procedere con i metodi `initialize()` e `calcola()`, necessario a contenere la logica  
di conversione gradi-radianti, è importante capire come poter avere nel codice Java i riferimenti ai controlli della GUI  
realizzata precedentemente.

JavaFX rende semplice questa operazione. Per riferirsi ad un controllo della UI  
è sufficiente fare in modo che sia dotato di un id ed utilizzare l’annotation `@FXML` su una variabile di istanza della classe  
dello stesso tipo del controllo e con il nome coincidente con l’id. Ad esempio, nel nostro caso abbiamo la necessità di riferirci  
ai controlli `TextField` relativi a gradi, radianti e al messaggio di errore da valorizzare se necessario.

Per questi controlli abbiamo  
definito gli id `txtGradi`, `txtRadianti` e `txtMsg`:

```
<TextField fx:id="txtGradi" GridPane.rowIndex="0" GridPane.columnIndex="1" />
         <TextField  fx:id="txtRadianti" GridPane.rowIndex="1" GridPane.columnIndex="1" />
         <Text fx:id="txtMsg" text=""  fill="yellow">
              <font>
                  <Font name="Arial Black" size="14.0"/>
              </font>
          </Text>
```

A questo punto per ottenere dei riferimenti è sufficiente dichiarare delle variabili  
all’interno della classe `GradiRadiantiController`:

```
....
        @FXML
        private TextField txtGradi;
        @FXML
        private TextField txtRadianti;
        @FXML
        private Text txtMsg;
        ....
```

Inizializzazione e test
-----------------------

Siamo quindi pronti per realizzare il codice di inizializzazione per il metodo `initialize()`:

```
txtGradi.setText("");
         txtRadianti.setText("");
```

E quello per il pulsante `calcola()`:

```
@FXML
          private void calcola(final ActionEvent event) {
	        event.consume();
	        String gradiStr = txtGradi.getText();
	        Pattern pattern = Pattern.compile("[0-9]+");
	        Matcher matcher = pattern.matcher(gradiStr);
	        if(!matcher.matches()) {
	            txtMsg.setText("Errore: inserire un valore numerico per i gradi");
	            return;
	        }
           float gradi = Float.parseFloat(txtGradi.getText());
           float radianti = gradi * 3.14f / 180;
           txtRadianti.setText(String.valueOf(radianti));
          }
```

In questo caso `@FXML` consente di definire `calcola()` come un metodo di evento. Per fare in modo che  
venga eseguito sull’evento click del pulsante dobbiamo modificare il tag `Button` utilizzando l’attributo `onAction`:

```
<Button fx:id="button" text="Calcola" GridPane.rowIndex="1" GridPane.columnIndex="2"
                    onAction="#calcola" />
```

Modifichiamo il codice all’interno della classe `GradiRadiantiFXMLJavaFX` in modo tale che utilizzi il file `Form2.fxml`:

```
primaryStage.setTitle("Convertitore da gradi a radianti");
           FXMLLoader loader = new FXMLLoader(getClass().
                getClassLoader().getResource("Form2.fxml"));
           BorderPane root = (BorderPane) loader.load();
           Scene scene = new Scene(root);
           primaryStage.setScene(scene);
           primaryStage.setResizable(false);
           primaryStage.show();
```

Eseguendo l’applicazione dovremmo avere la stessa schermata del capitolo precedente:

Figura 1. Convertitore

![Convertitore](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/convertitore_fxml-1.png)