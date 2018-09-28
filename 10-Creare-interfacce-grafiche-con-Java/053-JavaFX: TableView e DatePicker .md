Con JavaFX realizzare una tabella che mostri gli elementi di una lista e permetta d’interagire con essi  
è molto semplice grazie a **TableView**. Un altro componente utile  
nella costruzione delle UI è il **DatePicker**, esso permette di aprire  
un calendario a comparsa e selezionare una data.

TableView
---------

Anche in questo caso aggiungeremo i componenti ad un layout HBox evidenziando le parti di codice fondamentali. Una TableView può visualizzare lo stato degli oggetti di un particolare classe  
collegando le sue colonne alle variabili di istanza della classe stessa.

Consideriamo una classe d’esempio, `Person`,  
che modella il concetto di una persona, con due campi per memorizzare il nome ed il cognome:

```
public class Person {
        private String name;
        private String lastName;
        public Person(String name, String lastName) {
            this.name = name;
            this.lastName = lastName;
        }
        // metodi get e set ...
        @Override
        public String toString() {
            return name + " " + lastName;
        }
    }
```

Creiamo quindi una TableView in grado di visualizzare oggetti di classe `Person`. Il primo passo  
è istanziare un oggetto `TableView` specificando il tipo Generics `Person`:

```
TableView<Person> tableView = new TableView<>();
```

Successivamente definiamo due oggetti di tipo `TableColumn` che definiscono le colonne di cui sarà dotata la TableView.  
Un oggetto `TableColumn` deve essere collegato al campo della classe il cui valore intendiamo visualizzare in corrispondenza  
della colonna da esso definita.

Quest’ultima operazione è resa possibile dal metodo `setCellValueFactory()` al quale  
viene passato un oggetto `PropertyValueFactory` specificando nel suo costruttore il nome del campo della classe da collegare  
alla colonna:

```
TableColumn nameCol = new TableColumn("Name");
        nameCol.setCellValueFactory(new PropertyValueFactory<>("name"));
        TableColumn lastNameCol = new TableColumn("Last Name");
        lastNameCol.setCellValueFactory(new PropertyValueFactory<>("lastName"));
```

Infine utilizziamo il metodo `getColumns().addAll()` per inserire le colonne appena definite nella TableView:

```
tableView.getColumns().addAll(nameCol, lastNameCol);
```

Per poter aggiungere elementi alla TableView creata, abbiamo bisogno di utilizzare una `javafx.collections.ObservableList`.  
L’utilizzo di questo tipo di lista, che implementa il pattern **Observable**, consente la sincronizzazione tra il modello, rappresentato dalla lista stessa, e la view rappresentata dalla TableView.

In altre parole ogni modifica effettuata  
sulla `ObservalbeList` sarà visualizzata automaticamente dalla TableView.

```
Label lblValue = new Label();
        ObservableList<Person> values = FXCollections.
                observableArrayList();
        values.add(new Person("Laura", "Verdi"));
        values.add(new Person("Mario", "Rossi"));
        tableView.setColumnResizePolicy(TableView.CONSTRAINED_RESIZE_POLICY);
        tableView.setItems(values);
```

La Label `lblView` è stata definita per visualizzare l’elemento selezionato sulla TableView attraverso la seguente gestione  
dell’evento click su un record della TableView stessa:

```
tableView.getSelectionModel().selectedIndexProperty()
                .addListener((observable, oldValue, newValue) -> {
                    lblValue.setText(values.get(newValue.intValue()).toString());
                });
```

Per vedere in azione la sincronizzazione tra modello e TableView, aggiungiamo un pulsante che consenta di inserire  
un nuovo elemento nella lista:

```
Button button = new Button("Aggiungi");
            button.setOnAction(actionEvent -> {
            values.add(new Person("Luigi", "Bianchi"));
        });
```

Eseguendo la demo avremo:

Figura 1. TableView.

![Tableview](https://tbm-html.s3.amazonaws.com/app/uploads/2017/04/tableView.png)

Con un click su un record della tabella la label mostra il valore,  
mentre un click sul pulsante “Aggiungi” visualizzerà un TableView modificata con il nuovo elemento.

DatePicker
----------

Concludiamo il capitolo illustrando l’utilizzo del DatePicker.  
E’ possibile istanziare un DatePicker e ottenerne il valore attraverso un evento click  
tramite le seguenti linee di codice che aggiungiamo alla demo della TableView:

```
DatePicker datePicker = new DatePicker();
         datePicker.setOnAction(event -> {
            LocalDate date = datePicker.getValue();
            lblValue.setText("Date: " + date);
        });
```

Se eseguiamo nuovamente il programma e selezioniamo una data sul DatePicker,  
vedremo la label mostrarne il valore:

Figura 2. TableView e DatePicker

![Tableview e DatePicker](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/datePicker.png)