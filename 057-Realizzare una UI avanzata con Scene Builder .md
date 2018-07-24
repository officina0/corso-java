In questo capitolo completeremo la realizzazione del form del convertitore aggiungendo due componenti di input per l’acquisizione dei valori di conversione e il pulsante per l’evento di conversione. Facciamo notare che, per semplicità, il convertitore realizzato effettua la conversione soltanto in una direzione (Gradi-Radianti), un buon esercizio per renderlo più completo è l’aggiunta di codice che gestisca la conversione in entrambe le direzioni.

Iniziamo con l’aggiungere un container _GridPane_ al VBox del capitolo precedente per disprre i componenti secondo uno schema a griglia:

Figura 1. Convertitore con Scene Builder.

![Convertitore](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR6.png)

L’immagine mostra la selezione del _GridPane_ a lavoro ultimato con la disposizione 2×3 dei componenti. Si può notare il posizionamento della _Text_ per la _label_ dei Gradi in posizione (0,0), l’area di input associata immediatamente accanto nella posizione (0,1), lasciando vuota la terza colonna.

La seconda riga viene realizzata in modo simile con l’unica differenza della collocazione del pulsante di calcolo sull’ultima colonna. Una volta aggiunto il _GridPane_ la sua personalizzazione ci porta ad operare nelle 3 tab dell’Inspector avendo cura di aver selezionato preliminarmente il _GridPane_ stesso.

Facciamo quindi in modo che l’id nelle “Properties” sia impostato sul valore testuale `gridPane` ed il Layout presenti i valori indicati nella seguente figura:

Figura 2. Convertitore con Scene Builder.

![Convertitore](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR7.png)

I componenti di input sono rappresentati da `TextField` selezionabili all’interno della tab “Controls”. Le _Label_ sono gli stessi componenti `Text` (in “Shapes”) visti nel capitolo precedente. Il pulsante è rappresentato dal componente `Button` collocato all’interno di “Controls”. Per aggiungere questi componenti all’interno del _GridPane_ è sufficiente trascinarli sul _GridPane_ e seguire l’evidenziazione automatica delle aree di collocazione per posizionare il componente all’interno del punto desiderato nella griglia.

Si dovrebbe raggiungere facilmente il risultato mostrato in “Figura 1” agendo sulle larghezze dei componenti come visto nei capitoli precedenti oppure con le guide di _resize_ che compaiono automaticamente nel momento della loro selezione all’interno dell’interfaccia:

Figura 3. Convertitore con Scene Builder.

![Convertitore](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR8.png)

La personalizzazione delle dimensioni del pulsante segue esattamente le modalità viste per i campi di input e testuali. Per ottenere l’applicazione dell’effetto CSS di arrotondamento e gradiente nel colore dobbiamo assegnare un valore _id_ che lo colleghi alla proprietà CSS desiderata.

Per personalizzare il pulsante assegniamo quindi il valore testuale `button` sul campo id delle “Properties”. Per le aree di input specifichiamo il valore `txtGradi` sui campi `id` e `fx:id` del componente _TextField_ relativo ai gradi, e il valore `txtRadianti` sugli stessi campi del componente _TextField_ del valore dei radianti.

L’interfaccia del convertitore è completata ed aprendo l’alberatura del “Document” dovremmo visualizzare un risultato simile al seguente:

Figura 4. Convertitore con Scene Builder.

![Convertitore](http://www.html.it/wp-content/uploads/2017/06/SceneBuilderGR9.png)

Nel successivo capitolo completeremo il progetto definendo un controller e collegandolo all’interfaccia per poter rispondere agli eventi generati.