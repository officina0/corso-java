Nei capitoli precedenti abbiamo utilizzato componenti di JavaFX come `Label`, `TextField` e `Button`, in questo capitolo faremo invece uso del **RadioButton**.

Un RadioButton è un componente che permette una selezione da un insieme di opzioni. Supponiamo di voler introdurre una classica scelta del genere di appartenenza in un form di registrazione, inoltre, all’attivazione dell’evento click, desideriamo mostrare l’icona di un uomo o una donna in base alle scelta effettuata, questo ci permetterà di introdurre anche il componente **ImageView** per la visualizzazione di immagini.

RadioButton e opzioni per la selezione
--------------------------------------

Concentrandoci sul codice all’interno del metodo `start()` (l’esempio completo è in allegato) iniziamo col definire le due scelte del RadioButton ed una ImageView:

        ImageView image = new ImageView();
        RadioButton rbUomo = new RadioButton("Uomo");
        rbUomo.setUserData("Uomo");
        RadioButton rbDonna = new RadioButton("Donna");
        rbDonna.setUserData("Donna");
        

Su un RadioButton possiamo impostare sia la `label` mostrata che il dato associato, in questo caso decidiamo di far coincidere i due valori. Carichiamo le immagini delle icone associandole ai due RadioButton appena definiti. Questa operazione consente di disegnare un’icona accanto al RadioButton:

        Image imgUomo = new Image(getClass().getResourceAsStream("uomo.png"));
        Image imgDonna = new Image(getClass().getResourceAsStream("donna.png"));

        rbUomo.setGraphic(new ImageView(imgUomo));
        rbDonna.setGraphic(new ImageView(imgDonna));
        

I diversi RadioButton devono essere collegati in modo tale da formare un gruppo, in questo modo soltanto uno alla volta potrà essere selezionato. Tale funzionalità è possibile grazie alla classe `ToggleGroup`.

Completiamo questa fase con la preselezione su una delle due scelte:

        ToggleGroup rbGroup = new ToggleGroup();

        rbDonna.setToggleGroup(rbGroup);
        rbUomo.setToggleGroup(rbGroup);
        rbUomo.setSelected(true);
        rbUomo.requestFocus();
        

Immagine e ImageView
--------------------

I RadioButton vengono aggiunti separatamente ad un Layout insieme all’`ImageView` nella quale disegneremo l’immagine selezionata:

        hBox.getChildren().add(rbUomo);
        hBox.getChildren().add(rbDonna);
        hBox.getChildren().add(image);
         

`hBox` è un oggetto del Layout HBox utilizzato all’interno del codice completo. Infine definiamo l’evento associato al click sul RadioButton utilizzando un’espressione Lambda:

            rbGroup.selectedToggleProperty().addListener(
                (ObservableValue<? extends Toggle> obsValue, Toggle oldToggle, Toggle newToggle) -> {
                    String radioSelected = rbGroup.getSelectedToggle().getUserData().toString();
                    Image img = new Image(getClass().getResourceAsStream(radioSelected + ".png"));
                    image.setImage(img);
                });
         

All’interno del metodo `listener` recuperiamo il dato del RadioButton e lo utilizziamo per costruire il nome dell’immagine associata posizionata all’interno dello stesso package della classe. Settiamo infine limmagine sul campo `ImageView` dichiarato precedentemente. Il risultato finale è la seguente form dimostrativa:

Figura 1. RadioButton

![RadioButton](http://www.html.it/wp-content/uploads/2017/04/radio.png)

Alternando il click tra un’opzione e l’altra vedremo l’immagine cambiare a seconda del genere scelto.