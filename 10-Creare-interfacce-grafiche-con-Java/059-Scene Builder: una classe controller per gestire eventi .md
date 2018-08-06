Concludiamo la realizzazione del nostro convertitore in Scene Builder definendo una classe controller per la gestione degli eventi da collegare alla GUI. Non deve sorprendere il fatto che essa sarà utilizzabile esattamente come la classe `GradiRadiantiController` descritta nei capitoli precedenti, Scene Builder infatti è soltanto uno strumento per facilitare la realizzazione di un file FXML con la stessa struttura che avremmo avuto editandolo manualmente.

Recuperiamo quindi la classe controller dal precedente progetto ed inseriamola nel package `convertitorejavafx2`. Le parti fondamentali del controller sono le variabili di istanza collegate ai campi dell’interfaccia ed il metodo `calcola`:

            @FXML
            private TextField txtGradi;
            @FXML
            private TextField txtRadianti;
            @FXML
            private Text txtMsg;
		    
            @FXML
            private void calcola(final ActionEvent event) {
               ....
            }
        

Per poter utilizzare il controller `GradiRadiantiController` con il form abbiamo la necessità di specificare la classe all’interno del file `form.fxml` utilizzando l’attributo `fx:controller` sul tag `AnchorPane`. Apriamo quindi in “editing” il file `form.fxml` e modifichiamo il tag come segue:

        <AnchorPane id="AnchorPane" minHeight="-Infinity" minWidth="-Infinity" prefHeight="227.0" 
        prefWidth="355.0" xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1" 
        fx:controller="convertitorejavafx2.GradiRadiantiController">
        .....
        </AnchorPane>
        

Dopo aver salvato la modifica apriamo il form con Scene Builder, spostandoci all’interno dell’area “Document” dovremmo poter vedere la comparsa del tab “Controller”:

Figura 1. Tab “Controller”.

![Tab ](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR10.png)

Il controller è quindi collegato alla GUI, questo significa che i campi del form sono collegati ai campi del controller in base al funzionamento descritto nei capitoli precedenti riguardo all’uso dell’annotation `@FXML`.

Nel codice del controller anche il metodo `calcola` è marcato con annotation `@FXML`, questa marcatura rende il metodo eseguibile nell’ambito di un evento legato all’interfaccia grafica. Nel nostro caso desideriamo che `calcola` sia eseguito sull’evento `click` del pulsante `Button`. Per realizzare questa operazione selezioniamo il pulsante nel form all’interno di Scene Builder ed apriamo la tab “Code” dell’Inspector:

Figura 2. Tab “Code” dell’Inspector.

![Tab ](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR10.png)

All’interno del campo “On Action” digitiamo, o selezioniamo, il metodo `calcola`. L’evento `On Action` risponde al click sul pulsante eseguendo il corrispondente metodo sul controller. Per vedere il tutto in azione definiamo una classe `application` JavaFX che utilizzi il nuovo form:

         public class ConvertitoreJavaFX2 extends Application {
    
          @Override
          public void start(Stage stage) throws Exception {
           Parent root = FXMLLoader.load(getClass().getResource("form.fxml"));
          
           Scene scene = new Scene(root);
           stage.setScene(scene);
           stage.show();
          }

          public static void main(String\[\] args) {
           launch(args);
          }
         }