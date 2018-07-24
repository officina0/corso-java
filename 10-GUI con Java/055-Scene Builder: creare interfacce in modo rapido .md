Realizzare interfacce JavaFX editando direttamente i file FXML non rappresenta la via ottimale da un punto di vista del tempo richiesto e della complessità del lavoro. Per un’implementazione rapida, visuale ed intuitiva è però disponibile **Scene Builder**, un IDE che consente di evitare la creazione manuale di un file FXML.

In questo capitolo ci concentreremo sulla sua installazione e configurazione con Netbeans, nel successivo vedremo il suo utilizzo per la definizione dell’interfaccia del convertitore Gradi-Radianti.

Installazione e configurazione di Scene Builder con NetBeans
------------------------------------------------------------

Possiamo effettuare il download di [Scene Builder](http://www.oracle.com/technetwork/java/javafxscenebuilder-1x-archive-2199384.html) accedendo al sito Oracle. All’interno della pagina individuiamo il link come mostrato nell’immagine:

Figura 1. Scene Builder

![Scene Builder](http://www.html.it/wp-content/uploads/2017/05/SceneBuilderDL.png)

Al termine del download eseguiamo l’installazione **annotando il percorso di destinazione**:

Figura 2. Installazione Scene Builder

![Scene Builder Setup](http://www.html.it/wp-content/uploads/2017/05/SceneBuilderSetup.png)

Dopo aver installato Scene Builder abbiamo la necessità di configurarlo per l’uso con NetBeans. Eseguiamo Netbeans e selezioniamo “Options” all’interno del menù “tools”, ci troveremo di fronte ad una schermata simile alla seguente:

Figura 3. Configurazione Scene Builder

![Scene Builder Configurazione](http://www.html.it/wp-content/uploads/2017/05/SceneBuilderConf.png)

All’interno della tab “JavaFX”, facciamo in modo che la “Scene Builder Home” punti alla directory d’installazione annotata in precedenza e riavviamo l’IDE.

Creare un progetto con Scene Builder
------------------------------------

Per l’utilizzo di Scene Builder creiamo un nuovo progetto JavaFX, “ConvertitoreJavaFX2”:

Figura 4. Progetto Convertitore

![Scene Builder Progetto](http://www.html.it/wp-content/uploads/2017/05/SceneBuilderProgetto.png)

Il nuovo progetto porta alla generazione di un file FXML di base che consentirà di creare l’interfaccia del convertitore. Al termine della procedura guidata di creazione del progetto, apriamo il file FXML attraverso Scene Builder con un doppio click o selezionando “Open” dal menù a tendina. Dovremmo veder partire in automatico l’apertura separata dell’IDE di Scene Builder trovandoci di fronte ad una schermata come la seguente:

Figura 5. Scene Builder

![Scene Builder](http://www.html.it/wp-content/uploads/2017/05/SceneBuilderView.png)

L’immagine evidenzia attraverso i colori le aree fondamentali dell’IDE. La parte centrale, di colore bianco, rappresenta il controllo _AnchorPane_ inserito dal Wizard nella fase di creazione del progetto. L’area evidenziata in arancione racchiude la libreria di componenti (“_Library_“).

Attraverso la _Library_ possiamo aggiungere alla GUI qualsiasi componente JavaFX: pannelli di layout, pulsanti e aree di testo. La parte immediatamente sotto la Library, evidenziata in giallo (“Document”), mostra la visione ad albero della gerarchia di componenti aggiunti all’interfaccia. L’area marcata in verde del lato destro rappresenta l’**Inspector**, essa consente di accedere alle proprietà di un componente selezionato all’interno dell’interfaccia, cosi come alle sue configurazioni di layout o codice.

La costruzione di un’interfaccia tramite Scene Builder si articola essenzialmente nella selezione di componenti (dalla libreria) e il loro inserimento via Drag and Drop sul form centrale, e nell’uso dell’Inspector per modificare caratteristiche di stile e layout. Nel capitolo successivo mostreremo un esempio di GUI realizzando l’interfaccia del Convertitore Gradi-Radianti.