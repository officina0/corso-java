In questo capitolo mostreremo l’utilizzo di Scene Builder per la realizzazione dell’interfaccia di un convertitore  
Gradi-Radianti. Evidenzieremo soltanto gli step  
fondamentali, all’interno dell’allegato del capitolo è possibile visionare tutto il codice completo.  
Vedremo quindi come utilizzare le varie aree dell’editor per comporre la GUI cosi come  
illustra la seguente immagine:

Figura 1. Convertitore con Scene Builder

![Convertitore](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/SceneBuilderGR1.png)

Il primo componente che andiamo ad aggiungere alla form dell’area centrale dell’IDE è il _BorderPane_, un particolare tipo di Container. In Scene Builder tutti i container di layout sono situati all’interno della  
tab “Containers” della Library:

Figura 2. Containers in Scene Builder

![Containers in Scene Builder](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/SceneBuilderGR2.png)

Una volta trascinato ed aggiunto il componente, spostiamo l’attenzione sull’Inspector e personalizziamo alcune caratteristiche.  
Con il componente _BorderPane_ selezionato attraverso un click su di esso all’interno del form o dall’area “Document”,  
espandiamo la tab “Properties” dell’Inspector valorizzando la proprietà “stylesheets” con il valore `form.css`.

Assegnamo inoltre  
un id al componente stesso attraverso il campo collocato al di sotto della proprietà “stylesheets”. Con questa operazione  
riusciamo ad applicare un foglio di stile (il `form.css` utilizzato nei capitoli precedenti) ai componenti dell’interfaccia per personalizzarne l’aspetto. Lo andiamo quindi a recuperare e ad aggiungere nel package `convertitorejavafx2`  
del nuovo progetto.

Sempre con il _BorderPane_ selezionato  
all’interno della tab “Layout” settiamo i valori di `padding`, `width` ed `height`:

Figura 3.Layout in Scene Builder

![Layout in Scene Builder](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/SceneBuilderGR3.png)

Completiamo il lavoro sul `BorderPane` espandendo la tab “Code” dell’Inspector ed assegnando il valore  
`borderPane` al campo “fx:id”. Quest’ultima operazione è particolarmente importante in quanto consente  
di identificare un generico componente, nel nostro caso un _BorderPane_, all’interno del codice Java di un controller  
JavaFX.

Continuiamo l’editing aggiungendo un **container VBox** e due aree di testo al suo interno per il titolo e per il messaggio  
di errore. Il layout VBox è presente in “Containers”, mentre il componente  
_Text_ per le label di testo è situato nella tab “Shapes”, nella Library. Con il VBox selezionato  
impostiamo le seguenti proprietà di Layout all’interno dell’Inspector:

Figura 4.Layout in Scene Builder

![Layout in Scene Builder](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/SceneBuilderGR4.png)

Figura 5.Layout in Scene Builder

![Layout in Scene Builder](https://tbm-html.s3.amazonaws.com/app/uploads/2017/05/SceneBuilderGR5.png)

Successivamente selezioniamo la prima _Text_ partendo dall’alto, apriamo la tab “Properties” dell’Inspector ed editiamo  
il campo “Text” con il titolo del form: “Convertitore da gradi a radianti”. Rimanendo all’interno della “Properties”  
scriviamo nel campo “id” la stringa `text`. La seconda _Text_ verrà invece utilizzata per visualizzare un messaggio di errore.

Con la seconda _Text_ selezionata, definiamo il suo id con il valore `txtMsg` ed impostiamo un riempimento di colore giallo sfruttando  
il campo “Fill” all’interno di “Properties”. La label appena definita verrà referenziata all’interno del codice  
di programmazione, abbiamo quindi  
la necessità di impostare un valore per il campo “fx:id” nella tab “Code” e scegliamo di definire la stringa  
“txtMsg” come `fx:id`.

Nei successivi due capitoli completeremo l’interfaccia e definiremo un controller per la gestione dell’evento  
di calcolo della conversione.