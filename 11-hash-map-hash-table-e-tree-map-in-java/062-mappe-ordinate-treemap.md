# 062 Mappe Ordinate Tree Map

Abbiamo visto nei capitolo precedenti che HashMap ed HashTable rappresentano la soluzione maggiormente adottata per rappresentare mappe in Java.

In situazioni in cui è richiesto un ordinamento sulle chiavi possiamo considerare la classe `TreeMap`. Si tratta di una classe la cui complessità di tempo per le varie è operazioni è logaritmica nella dimensione della mappa. L’ordinamento è quindi una funzione in più che otteniamo, rispetto ad HashMap ed HashTable, perdendo qualcosa in termini di performance.

Nel caso di una `TreeMap` è importante evidenziare che le classi utilizzate come chiavi devono produrre **oggetti confrontabili**. Possiamo rispettare questo requisito facendo in modo che la classe che rappresenta le chiavi implementi l’interfaccia `Comparable`, oppure utilizzando un costruttore di TreeMap che accetti come input un oggetto di una classe che implementi l’interfaccia `Comparator`.

Vediamo un esempio, realizziamo una `TreeMap` che memorizzi le coppie cognome-nome di una persona ordinandole alfabeticamente sul cognome. Creiamo una classe applicativa, `TreeMapApp`, che al suo interno contenga la classe per rappresentare una chiave della `TreeMap`:

public class TreeMapApp {

```text
static class LastName implements Comparable<LastName> {

    private String value;

    public LastName(String lastName){
      this.value = lastName;    
    }

    @Override
    public int compareTo(LastName lastName) {
        char\[\] chars1 = this.value.toCharArray();
        char\[\] chars2 = lastName.value.toCharArray();

        int size1 = chars1.length;
        int size2 = chars2.length;

        int min = size1 > size2 ? size1 : size2;

        for (int i = 0; i < min; i++) {
            int value = Character.compare(chars1\[i\], chars2\[i\]);
            if (value < 0) {
                return -1;
            } else if (value > 0) {
                return 1;
            }
        }
        return 0;
    }
    @Override
    public String toString(){
        return value;
    }
}

public static void main(String\[\] args) {
    ....
}
```

}

La classe `LastName` implementa l’interfaccia `Comparable` specificando se stessa come tipo di comparazione. `Comparable` richiede di implementare il metodo `compareTo()` che nel nostro caso conterrà la logica per determinare quale cognome viene prima di un altro nell’ordinamento alfabetico. Successivamente nel metodo `main` creaimo una `TreeMap` inserendo le entry senza alcun ordinamento:

```text
     TreeMap<LastName,String> rubrica = new TreeMap<>();

     rubrica.put(new LastName("Rossi"), "Mario");
     rubrica.put(new LastName("Gialli"), "Laura");
     rubrica.put(new LastName("Marrone"), "Laura");
     rubrica.put(new LastName("Verdi"), "Monica");
     rubrica.put(new LastName("Bianchi"), "Luigi");
     rubrica.put(new LastName("Magenta"), "Osvaldo");
     rubrica.put(new LastName("Neri"), "Luca");
```

Ed infine testiamo il comportamento della `TreeMap` per verificare con una stampa che il tutto funzioni correttamente:

```text
    rubrica.forEach((lastName,firstName)->
    System.out.println(lastName+"-"+firstName));
```

Il risultato sulla console di output dovrebbe essere il seguente:

Bianchi-Luigi Gialli-Laura Magenta-Osvaldo Marrone-Laura Neri-Luca Rossi-Mario Verdi-Monica

