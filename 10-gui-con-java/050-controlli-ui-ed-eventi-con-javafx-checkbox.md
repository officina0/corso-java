# 050 Controlli UI Ed Eventi Con Java FX Check Box

Il controllo **CheckBox** di JavaFX consente la selezione multipla da un insieme di possibili scelte. Immaginiamo un form nel quale si chieda di selezionare i linguaggi di programmazione conosciuti tra un insieme di quelli indicati. Il primo passo è quello di definire un controllo `CheckBox` per ogni possibile scelta:

```text
    CheckBox chkJava = new CheckBox("Java");
    CheckBox chkCplusPlus = new CheckBox("C++");
    CheckBox chkPhp = new CheckBox("Php");
    CheckBox chkCsharp = new CheckBox("C#");
```

Definiamo una label nella quale mostreremo la selezione delle scelte:

```text
    Label lblChoice = new Label();
    lblChoice.setAlignment(Pos.TOP_LEFT);
```

Ed aggiungiamo i vari componenti ad un `HBox` come fatto nei precedenti capitoli:

```text
    hBox.getChildren().addAll(chkJava, chkCplusPlus, chkPhp, chkCsharp, lblChoice);
```

Per poter utilizzare effettivamente una CheckBox, abbiamo bisogno di poter rispondere all’evento `change` corrispondente alla selezione o deselezione dell’oggetto. Implementiamo una classe `CheckListener` idonea a gestire questo evento, avendo cura di passare come come input al suo costruttore la label di visualizzazione, il controllo `CheckBox` e una lista di stringhe per la memorizzazione delle scelte effettuate:

```text
    List<String> choiceList = new ArrayList<>();

    CheckListener checkListenerJava = new CheckListener(lblChoice, choiceList, chkJava);
    CheckListener checkListenerCPlusPlus = new CheckListener(lblChoice, choiceList, chkCplusPlus);
    CheckListener checkListenerPhp = new CheckListener(lblChoice, choiceList, chkPhp);
    CheckListener checkListenerCSharp = new CheckListener(lblChoice, choiceList, chkCsharp);

    chkJava.selectedProperty().addListener(checkListenerJava);
    chkCplusPlus.selectedProperty().addListener(checkListenerCPlusPlus);
    chkPhp.selectedProperty().addListener(checkListenerPhp);
    chkCsharp.selectedProperty().addListener(checkListenerCSharp);
```

La classe `CheckListener` implementa l’interfaccia `ChangeListener<Boolean>` fornendo quindi un’implementazione al metodo `changed()` che verrà invocato sulla selezione o deselezione di un `CheckBox`. Riportiamo la parte di codice fondamentale, l’esempio completo è presente in allegato al capitolo.

```text
    class CheckListener implements ChangeListener<Boolean>{
        ...
    @Override
    public void changed(ObservableValue<? extends Boolean> observable, Boolean oldValue, Boolean newValue) {
        String checkBoxTxt = checkBox.getText();
        if (newValue) {
            if (!choiceList.contains(checkBoxTxt)) {
                choiceList.add(checkBoxTxt);
            }
        } else {
            choiceList.remove(checkBoxTxt);
        }

        lblChoice.setText("");
        StringBuilder newLblValue = new StringBuilder();
        choiceList.forEach((String choice) -> {
            newLblValue.append(choice).append(" ");
        });
        lblChoice.setText(newLblValue.toString());
    }
    ...
    }
```

All’interno del metodo `changed()` viene recuperato il valore testuale del `CheckBox` selezionato, se il valore corrente è cambiato \(`newValue=true`\), e se la lista non contiene la scelta selezionata, il valore viene aggiunto alla lista delle scelte effettuate. Con `newValue=false`, la scelta è stata deselezionata e viene quindi rimossa dalla lista.

Successivamente la label `lblChoice` viene pulita ed aggiornata con la nuova lista di selezioni. Se eseguiamo la demo avremo un’interfaccia simile alla seguente:

Figura 1. CheckBox.

![CheckBox](http://www.html.it/wp-content/uploads/2017/04/checkbox.png)

Selezionando e deselezionando le varie opzioni vedremo la lista testuale aggiornarsi opportunamente.

