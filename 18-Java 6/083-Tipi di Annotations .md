### @Deprecated

L’annotazione @Deprecated viene utilizzata per specificare che l’elemento indicato è un elemento deprecato, cioè, attivo (per mantenere retrocompatibilità) ma non consigliato perché rimpiazzato da uno nuovo e supportato.

Listato 5.1. Esempio con un annotazione @Deprecated

public class TestDeprecated {  
  @Deprecated  
  public void metodoA() {  
    System.out.println(“Questo metodo è DEPRECATO, usa metodoB().”);  
  }  
  
  public void metodoB() {  
    System.out.println(“Questo metodo è SUPPORTATO.”);  
  }  
}

La compilazione di questa classe non darà alcun segnale, procederà tutto normalmente. Sarà la compilazione della classe che userà TestDeprecated a ricevere segnalazioni di warning dal compilatore quando viene utilizzato il metodo `metodoA()`.

TestDeprecated td=new TestDeprecated();  
Td.metodoA();

### @Override

L’annotation @Override è probabilmente la più utile in quanto consente di evitare degli errori, che in fase di codifica spesso accadono. L’annotazione dice che l’elemento indicato è un elemento che fa l’override (sovrascrive) del relativo elemento, del genitore da cui eredita.

L’esempio ci permetterà di capire meglio.

Listato 5.2. Esempio di Override

class A{  
  void metodo1(){  
    System.out.println(“Metodo 1”);  
  }  
}  
  
class B extends A{  
  @Override  
  void metodoo1(){  
    System.out.println(“Override A.metodo1()”);  
  }  
}

Abbiamo la classe genitore A, che presenta un metodo, `metodo1()`. Creiamo una classe B, erede di A. Vogliamo fare l’override di `metodo1()`, quindi annotiamo il metodo presente nella classe B con l’annotazione @Override, indicando che il metodo annotato è un metodo che sovrascrive un metodo del genitore.

Se provate a compilare il codice, il compilatore vi restituirà un errore. Se notate, infatti, ho inserito un errore di battitura nel nome del metodo. Senza l’annotazione @Override la compilazione sarebbe andata a buon fine e non ci saremmo accorti dell’errore.

### @SuppressWarning

L’annotazione @SuppressWarning è utile quando vogliamo sopprimere le indicazioni di warning da parte del compilatore, ad esempio, perché stiamo usando dei metodi deprecati.

Listato 5.3. Esempio di SuppressWarning

@SuppressWarnings({“deprecation”})  
  public void usaMetodoDeprecato() {  
   TestDeprecated t = new TestDeprecated();  
   t.metodoA();  
  }

Pur usando dei metodi deprecated, al compilatore abbiamo segnalato di sopprimere i warning.