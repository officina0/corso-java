# 104 Gestire Le Eccezioni

Le **eccezioni** sono un modo chiaro e strutturato per controllare gli errori, senza confondere il codice con tante istruzioni di controllo dell’errore. Quando si verifica una situazione di errore viene lanciata una eccezione, che se viene in seguito catturata permette di gestire l’errore, altrimenti viene eseguita dal supporto a tempo di esecuzione di Java una ruotine di default.

Esiste in Java una **classe Exception**, e le eccezioni sono degli oggetti appartenenti a questa classe, questo ci permette di trattarle come gli altri componenti del linguaggio.

Vediamo subito come **creare una eccezione**. Si deve creare una classe che estende Throwable, o meglio Exception , la quale è una estensione di Throwable. Supponiamo di volere creare una eccezione per dire che riferiamo una stringa vuota, scriveremo:

public class ErroreStringaVuota extends Exception {

ErroreStringaVuota\(\) { super\("Attenzione, stai riferendo una stringa non inizializzata"\); } }

Questa classe si deve trovare in un file chiamato come la classe, ovvero “`ErroreStringaVuota.java`“. Dopo averla creata vorremmo un modo per lanciarla, ovvero per mettere in risalto la situazione di errore, che a questo punto spero sia catturata da qualcuno. Le eccezioni vengono lanciate con l’**istruzione throw**, la quale deve essere in un metodo che deve essere dichiarato con una clausola _throws_, ad esempio, nel nostro caso se voglio creare un metodo di stampa della stringa che controlla anche l’errore, scriverò nella mia classe:

public void Stampa \(String a\) throws ErroreStringaVuota /\* Significa che questo metodo può  
lanciare un'eccezione di tipo ErroreStringaVuota \*/ { if \(a == null\) throw new ErroreStringaVuota\(\); else System.out.println\(a\); }

Quindi a questo punto il metodo Stampa manderà sullo schermo la stringa passatagli come parametro, e genererà una eccezione di tipo `ErroreStringaVuota` se il puntatore era null. L’eccezione lanciata da noi si dice **eccezione controllata**, questa deve essere obbligatoriamente gestita, mentre quelle lanciate da Java non per forza devono essere catturate e gestite.

## Il blocco try/catch

Vediamo ora come **catturare le eccezioni**, ad esempio la nostra `EccezioneStringaVuota`. Le eccezioni vengono catturate quando vengono invocati i metodi che sono dichiarati con la clausola throws, per catturarle bisogna invocare il metodo in blocchi **try**, essi hanno la sintassi seguente:

try BLOCCO // Questo è un blocco pericoloso, può lanciare eccezioni.

catch \(Tipo di Eccezione da catturare\) BLOCCO // Questo è un blocco di ripristino.

catch \(Tipo di Eccezione da catturare\) BLOCCO // Questo è un blocco di ripristino. ... finally BLOCCO

Il corpo di questa istruzione viene eseguito fino alla fine oppure fino alla visione di una eccezione, in caso di eccezione si esaminano le clausole **catch** per vedere se esiste un gestore per quella eccezione o per una sua superclasse, se nessuna clausola catch cattura l’eccezione, essa viene lanciata nel metodo che l’ha provocata \(che l’ha lanciata\).

Se nel try è presente una clausola **finally**, il suo codice viene eseguito dopo aver completato tutte le altre operazioni del try, indipendentemente dal fatto che questa abbiano lanciato una eccezione o no.

Ad esempio il nostro metodo `Stampa` può essere chiamato così: Sia `X` la stringa da stampare:

try { Stampa\(X\); } catch \(ErroreStringaVuota e\) { System.out.println \("Spiacente"\); }

In questo caso se `X` è null verrà lanciata l’eccezione e catturata, quindi verrà stampata la stringa `"Spiacente"`, altrimenti `X`. Se scrivessimo `Stampa(X)` fuori dalla `try` avrei avuto errore, perché le eccezioni da noi generate devono obbligatoriamente essere catturate, questo ci aiuta a scrivere codice coerente. Le eccezioni già previste da Java possono essere invece catturate oppure no.

Il seguente esempio riprende il CiaoMondo del primo capitolo, che se invocato senza parametri dava una eccezione di ArrayIndexOutOfBoundsException, che questa volta viene gestita;

class CiaoMondo {

public static void main\(String\[\] args\) { System.out.print \("Ciao mondo, sono il primo programma in Java "\);

```text
String Nome;
String Cognome;

try { Nome = args\[0\]; }
catch (ArrayIndexOutOfBoundsException e) { 
  Nome="Non hai inserito il tuo Nome"; 
}

try { Cognome=args\[1\]; }
catch (ArrayIndexOutOfBoundsException e) { 
  Cognome="Non hai inserito il tuo Cognome"; 
}

System.out.println ("di "+Nome+" "+Cognome);
```

} }

Possiamo verificare il comportamento del programma con input diversi ad esempio questi:

java CiaoMondo Nome Cognome java CiaoMondo Nome java CiaoMondo

