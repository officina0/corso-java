Nei capitoli precedenti abbiamo visto come realizzare interfacce stile form per applicativi  
che possiamo collocare nell’ambito gestionale.

Ma le applicazioni JavaFX  
possono essere eseguite sia in modalità standalone che in  
un browser, se infatti esploriamo la directory `dist` di un progetto Netbeans JavaFX noteremo la produzione di un file  
con estensione **jnlp**, si tratta appunto del formato web dell’applicazione. In questo capitolo non affronteremo  
il lato web di JavaFX, concentrandoci invece, sulle **API per la grafica 3D**, è però la loro introduzione che porta a parlare di “WEB 3.0”.

Con un libreria per la grafica 3D siamo in grado di portare complesse applicazione grafiche all’interno di un browser, basti pensare alla visualizzazione di immagini tridimensionali all’interno di una web App.

Descriveremo quindi un esempio di animazione realizzando un cubo animato che ruota continuamente. Iniziamo con il creare  
un nuovo progetto JavaFX specificando _JavaFXTrasfromations_ come nome per il progetto e la classe applicativa. Al termine della fase di creazione apriamo la classe `JavaFXTrasformations` ed iniziamo ad editare  
il metodo `start()`:

```
Box testBox = new Box(5, 5, 5);
  testBox.setMaterial(new PhongMaterial(Color.GOLD));
  testBox.setDrawMode(DrawMode.FILL);
```

Un Box rappresenta un cubo, il costruttore specificato consente la creazione di un cubo definendo larghezza, altezza e  
profondità. La classe `PhongMaterial` permette di rappresentare le caratteristiche visuali di un oggetto utilizzando il **modello  
di illuminazione di Phong**. Essenzialmente gestisce l’interazione della luce con la Mesh di superficie dell’oggetto alla quale è applicata.

Nel nostro caso facciamo in modo che l’oggetto sia dotato di un materiale color oro. L’ultima riga di codice consente di disegnare  
il cubo riempiendolo del materiale specificato (`DrawMode.FILL`) oppure in modalità Mesh (`DrawMode.LINE`). Per una scena 3D abbiamo  
bisogno di definire una camera per una visione in prospettiva:

```
PerspectiveCamera camera = new PerspectiveCamera(true);
   camera.getTransforms().addAll(
                new Rotate(30, Rotate.X_AXIS),
                new Translate(0, 0, -20));
```

Non entriamo nel dettaglio del funzionamento di una trasformazione di prospettiva in quanto argomento di computer grafica, ci limitiamo  
ad evidenziare che il codice appena editato posiziona la camera in modo tale da centrare e visualizzare il cubo.  
Per poter animare il cubo in modo tale che ruoti con continuità, facciamo invece uso della classe `RotateTransition`:

```
RotateTransition rotateTransition = new RotateTransition(
                Duration.millis(3000), testBox);
  rotateTransition.setFromAngle(0);
  rotateTransition.setToAngle(360);
  rotateTransition.setCycleCount(Timeline.INDEFINITE);
  rotateTransition.setInterpolator(Interpolator.LINEAR);
  rotateTransition.play();
```

`RotateTransition` consente di creare un’animazione in stile rotazione. Nel nostro caso utilizziamo il costruttore  
della classe che richiede il tempo di durata dell’animazione e l’oggetto da animare. L’oggetto è chiaramente il cubo definito precedentemente,  
mentre la durata influirà sulla velocità di rotazione: più il valore specificato è piccolo più la rotazione sarà veloce.

Specifichiamo ora una rotazione ciclica indefinita con una interpolazione lineare, essa è fondamentale per evitare strappi sull’animazione, il suo valore rende quindi  
fluida l’animazione stessa. Infine, con il metodo `play()`, diamo inizio alla rotazione del cubo e concludiamo collegando tutti gli oggetti alla scena:

```
Group root = new Group();
        root.getChildren().add(camera);
        root.getChildren().add(testBox);
        SubScene subScene = new SubScene(root, 300, 300);
        subScene.setFill(Color.DARKSLATEBLUE);
        subScene.setCamera(camera);
        Group group = new Group();
        group.getChildren().add(subScene);
        Scene scene = new Scene(group, 300, 300);
        primaryStage.setTitle("Hello World!");
        primaryStage.setScene(scene);
        primaryStage.show();
```

Siamo quindi pronti per lanciare l’applicazione e visualizzarne l’effetto:

Figura 1. Cubo Fill

![Cubo Fill](https://tbm-html.s3.amazonaws.com/app/uploads/2017/06/cuboFill.png)

Figura 2. Cubo Line

![Cubo Line](https://tbm-html.s3.amazonaws.com/app/uploads/2017/06/cuboLine.png)