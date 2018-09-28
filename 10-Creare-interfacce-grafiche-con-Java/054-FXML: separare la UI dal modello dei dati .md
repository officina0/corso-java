Il linguaggio FXML
------------------

Il linguaggio **FXML** permette di separare la presentazione dalla logica applicativa di un software scritto con JavaFX.

Un file FXML descrive il layout di una GUI permettendo l’implementazione  
del pattern MVC, per illustrare le basi di questo linguaggio riprendiamo il convertitore Gradi-Radianti e definiamone l’UI.

Nel package di default del progetto `NetBeans` definiamo il file `form1` con estensione `fxml` aggiungendo al suo interno le righe di codice:

```
<?xml version="1.0" encoding="UTF-8"?>
<?language JavaScript?>
<?import java.lang.*?>
<?import java.net.*?>
<?import java.util.*?>
<?import javafx.scene.*?>
<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>
<?import javafx.geometry.Insets?>
<?import javafx.scene.text.Text?>
<?import javafx.scene.effect.DropShadow?>
<?import javafx.scene.text.Font?>
<?import javafx.scene.effect.Reflection?>
```

Quindi: definiamo un file `xml` con il classico tag iniziale,  
specifichiamo di voler utilizzare **Javascript** nel file `fxml`  
e inseriamo una serie di `import` dei tag  
corrispondenti ai controlli JavaFX utilizzati per il convertitore.

Sappiamo che il tag di layout `BorderPane` è anche  
il tag root da cui discende tutta l’alberatura dell’interfaccia, iniziamo quindi con l’aggiungerlo:

```
<BorderPane id="borderPane" xmlns:fx="http://javafx.com/fxml">
		    <stylesheets>
		        <URL value="@form.css"/>
		    </stylesheets>
    		<padding>
        		<Insets top="10" bottom="50" left="50" right="50" />
    		</padding>
    	</BorderPane>
```

Da notare che la sintassi per l’importazione del file **CSS** prevede  
il riferimento ad un file situato nella stessa posizione del file `fxml`.

Continuiamo inserendo il tag `top`  
del `BorderPane` immediatamente dopo il tag `padding`. All’interno del `top` aggiungiamo una `text`  
per il titolo del programma e un’altra (`txtMsg`) per mostrare un messaggio all’utente:

```
<top>
        <VBox>
            <padding>
                <Insets top="10" bottom="50" left="50" right="50" />
            </padding>
            <Text id="text" text="Convertitore da gradi a radianti">
                <font>
                    <Font name="Arial Black" size="28.0"/>
                </font>
                <effect>
                    <DropShadow offsetX="3" offsetY="3" />
                </effect>
            </Text>
            <Text fx:id="txtMsg" text=""  fill="yellow">
                <font>
                    <Font name="Arial Black" size="14.0"/>
                </font>
            </Text>
        </VBox>
    	</top>
```

Rispetto alla versione originaria del programma decidiamo di **utilizzare un layout VBox** per far in modo che i controlli  
del titolo e del messaggio compaiano uno sotto l’altro.

Completiamo l’interfaccia con l’utilizzo del tag `center` e del tag `GridPane`  
per aggiungere label, aree di testo per l’inserimento dei dati e un pulsante per la logica di calcolo. Il tag `center`  
deve chiaramente seguire il tag `top`:

```
<center>
        <GridPane id="gridPane" hgap="5" vgap="5">
            <effect>
                <Reflection fraction="0.7" />
            </effect>
            <padding>
                <Insets top="20" bottom="20" left="20" right="20" />
            </padding>
            <Label text="Gradi"  GridPane.rowIndex="0" GridPane.columnIndex="0" />
            <TextField fx:id="txtGradi" GridPane.rowIndex="0" GridPane.columnIndex="1" />
            <Label text="Radianti"  GridPane.rowIndex="1" GridPane.columnIndex="0" />
            <TextField  fx:id="txtRadianti" GridPane.rowIndex="1" GridPane.columnIndex="1" />
            <Button fx:id="button" text="Calcola" GridPane.rowIndex="1" GridPane.columnIndex="2" />
         </GridPane>
         </center>
```

Facciamo notare l’utilizzo di `GridPane.rowIndex` e `GridPane.columnIndex`  
per specificare una posizione all’interno del layout `GridPane` per ciascuno dei controlli aggiunti.

Javascript e file FXML
----------------------

Come anticipato  
un file FXML consente di utilizzare Javascript per inserire la logica  
all’interno del file stesso. Vediamo un esempio di utilizzo di Javascript definendo una funzione che implementi  
la logica di calcolo.

Chiamiamo la funzione `calcola` e facciamo in modo che venga invocata  
al click del `button`. Affinchè questo avvenga abbiamo bisogno di inserire l’attributo `onAction="calcola();"`  
nel tag `button`. Inseriamo quindi il codice Javascript utilizzando il tag `script`:

```
<Button fx:id="button" text="Calcola" GridPane.rowIndex="1" GridPane.columnIndex="2"
                    onAction="calcola();" />
            <fx:script>
                function calcola() {
                    txtMsg.setText("");
                    if( !isNaN(txtGradi.getText()) && !txtGradi.getText().isEmpty() )
                        txtRadianti.setText(txtGradi.getText()*3.14/180);
                        else{
                        txtMsg.setText("Errore: inserire un valore numerico per i gradi");
                    }
                }
         </fx:script>
```

Per poter vedere in azione l’interfaccia creata,realizziamo una classe applicativa JavaFX di nome `GradiRadiantiFXMLJavaFX`  
con il seguente codice all’interno del metodo `start`:

```
primaryStage.setTitle("Convertitore da gradi a radianti");
           FXMLLoader loader = new FXMLLoader(getClass().
                getClassLoader().getResource("Form1.fxml"));
           BorderPane root = (BorderPane) loader.load();
           Scene scene = new Scene(root);
           primaryStage.setScene(scene);
           primaryStage.setResizable(false);
           primaryStage.show();
```

Eseguendo l’applicazione dovremmo avere una schermata simile alla seguente:

Figura 1. Convertitore

![Convertitore](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/convertitore_fxml.png)