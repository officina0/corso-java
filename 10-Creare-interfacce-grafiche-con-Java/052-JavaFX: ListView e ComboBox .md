ListView
--------

La `ListView` consente di definire  
un elenco che è possibile scorrere orizzontalmente o verticalmente, adatto  
per liste di dimensioni ridotte. Se l’elenco da rappresentare è particolarmente lungo, si pensi ad esempio alla lista dei comuni italiani, la `ComboBox` rappresenta la scelta  
migliore, in quanto consente uno scorrimento più efficiente e compatto attraverso l’apertura  
di una tendina.

La demo che andiamo a realizzare consente di valutare il funzionamento congiunto  
di questi due controlli mostrando sia la modalità di passaggio della lista di elementi che  
l’evento di selezione su uno di essi. Come nei precedenti capitoli faremo uso di un Layout  
di tipo HBox, creiamo quindi un oggetto `ListView` definendo una preferenza sulle sue dimensioni:

```
ListView<String> listView = new ListView<>();
        listView.setPrefWidth(120);
        listView.setPrefHeight(50);
```

Per poter popolare la `ListView` abbiamo bisogno di una lista di tipo `ObservableList<String>` che  
incapsuli al suo interno una normale lista di stringhe Java da mostrare nel controllo:

```
ObservableList<String> viewItems =FXCollections.observableArrayList (
            "Data1", "Data2", "Data3", "Data4","Data5","Data6","Data7");
        listView.setItems(viewItems);
```

Vogliamo adesso interagire con il componente e, in particolare, desideriamo  
selezionare una stringa dalla lista e mostrarla in una `Label`; continuiamo quindi  
la stesura del codice definendo una `Label` il cui contenuto verrà aggiornato  
sull’evento di selezione di un elemento della `ListView`:

```
Label valueLbl = new Label("Data ...");
        listView.getSelectionModel().selectedItemProperty().
                addListener((ObservableValue<? extends String> obs, String oldVal, String newVal) -> {
            valueLbl.setText(newVal);
        });
```

Il parametro `newVal` contiene l’elemento selezionato che, a sua volta, viene settato come testo sulla `Label`.

ComboBox
--------

L’utilizzo di una ComboBox è molto simile a quello di una `ListView`, istanziamo un suo oggetto  
al quale passiamo la `ObservableList<String>` degli elementi che intendiamo visualizzare:

```
ObservableList<String> comboItems = FXCollections.observableArrayList(
            "Value 1",
            "Value 2",
            "Value 3"
            );
        ComboBox comboBox = new ComboBox(comboItems);
```

Aggiungiamo quindi un listener sulla selezione:

```
comboBox.valueProperty().addListener((ObservableValue observable, Object oldValue, Object newValue) -> {
            valueLbl.setText(newValue.toString());
        });
```

Possiamo arricchire la demo con un ulteriore frammento di codice che mostri come recuperare  
un valore selezionato da una `ListView` all’interno dell’evento click di un `Button`:

```
Button button = new Button("Mostra Selezione");
        button.setOnAction(actionEvent -> {
            String elemento = listView.getSelectionModel().getSelectedItem();
            if(elemento!=null)
                     valueLbl.setText("Button:"+elemento);
        });
```

L’interazione con gli oggetti `ListView`  
e `ComboBox` è sempre gestita attraverso un **Selection Model**. Nel caso del `Button`  
infatti, recuperando il `Section Model` sull’oggetto `ListView` riusciamo ad accedere  
al suo stato interno recuperando la selezione corrente.

Esecuzione della demo
---------------------

Eseguendo la demo avremo  
un’interfaccia simile alla seguente per testare il corretto funzionamento dei componenti:

Figura 1. ListView e ComboBox.

![ListView e ComboBox](https://tbm-html.s3.amazonaws.com/app/uploads/2017/04/listview_combobox.png)