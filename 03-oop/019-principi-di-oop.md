# 019 Principi Di OOP

Nelle sezioni precedenti, esaminando alcune caratteristiche di Java, abbiamo incontrato alcuni concetti di OOP ed alcuni dei suoi elementi cardine come, ad esempio, la definizione di classe e la distinzione tra classe e istanza.

Tuttavia per avere la nozione di _programmazione orientata agli oggetti_ comprende qualcosa in più, che prescinde dal linguaggio di programmazione specifico e riguarda in generale lo schema mentale da assumere nell’affrontare la modellazione, l’analisi e l’implementazione della soluzione di un problema.

> «The fact that you know Java doesn’t mean that you have the ability to transform that knowledge into well-designed object oriented systems.»  
> Developing Applications with Java and UML – _Paul R. Reed_

Questa citazione ci ricorda che la padronanza della sintassi di un linguaggio di programmazione orientato agli oggetti, anche fosse perfetta, non basta per organizzare e strutturare sistemi e programmi che rispecchino il paradigma OOP. La programmazione orientata agli oggetti infatti ha una sua complessità che richiede sia la conoscenza di nozioni specifiche \(alcune delle quali squisitamente teoriche\), sia un opportuno metodo ed una disciplina.

In questa e nelle prossime lezioni esaminiamo alcuni concetti fondamentali della OOP in Java, passando per la notazione UML e arrivando a parlare di OOD \(Object Oriented Design\).

## Design a oggetti vs Structured-design

Già dalla fine degli anni ’90, quando gran parte della teoria _OO_ è stata messa a punto, è emerso come analisi e design orientati agli oggetti fossero fondamentalmente differenti dalla tradizionale metodologia del design strutturato.

Il focus sugli oggetti ha bisogno di un diverso approccio alla decomposizione dei problemi e non deve sorprendere quindi che produca architetture software molto dissimili da quelle realizzate per mezzo dello _structured-design_:

* Con l’_approccio strutturale_ ci si concentra sulla scomposizione di algoritmi in procedure.
* Nell’_approccio “a oggetti”_ ci si focalizza sull’interazione di elementi \(oggetti\) che comunicano \(scambiano messaggi\) tra loro.

La OOP consente la descrizione di dinamiche complesse e la realizzazione di “procedure” più facilmente implementabili \(e mantenibili\) favorendo le interazioni tra oggetti sostanzialmente semplici.

> La complessità dei problemi non deve essere rappresentata spesso da oggetti complessi ma da interazioni complesse tra oggetti semplici.

Cerchiamo di comprendere quindi cos’è la programmazione orientata agli oggetti e …cosa non è, seguendo quanto più possibile la visione di G. Booch \(“OBJECT-ORIENTED ANALYSIS AND DESIGN”\) che ne è stato uno dei padri, ma tenendo anche presente la seguente affermazione di T. Rentsch \(“A Generalized Object Model” ACM SIGPLAN NOTICES – v17, n9\):

> "My guess is that object-oriented programming will be in the 1980s what structured programming was in the 1970s. Everyone will be in favor of it. Every manufacturer will promote his products as supporting it. Every manager will pay lip service to it. Every programmer will practice it \(differently\). And no one will know just what it is".

Booch commentava che la medesima affermazione era ancora vera alla fine degli anni novanta e la si applica ancora abbastanza bene.

## OOP, OOD & OOA

In modo del tutto analogo a quanto successo per il design strutturale \(sviluppatosi in modo da riuscire a costruire sistemi complessi utilizzando gli algoritmi come loro elemento\), l’object-oriented design si è evoluto in modo da dare supporto agli sviluppatori che utilizzano le classi e gli oggetti come loro elemento base nella fase di sviluppo di un progetto.

Per la verità il modello ad oggetti è di importanza capitale nella descrizione di numerose dinamiche anche non legate allo sviluppo del software e ha quindi subito influenze da diversi ambiti. Perciò il concetto di “modello ad oggetti” finale unifica visioni provenienti da mondi diversi: dall’informatica, alla strutturazione dei database, alla costruzione e design di interfacce fino ad arrivare all’architettura dei computer.

## OOP: Object Oriented Programming

Si definisce **Object Oriented Programming** \(o semplicemente OOP\) un metodo di implementazione in cui i programmi sono organizzati attraverso un insieme di oggetti, ognuno dei quali è un’istanza di una classe, e queste classi sono tutte parte di una gerarchia di entità unite fra di loro da una relazione di ereditarietà.

Nella precedente definizione ci sono tre parti particolarmente importanti:

1. OOP utilizza un insieme di **oggetti** \(non algoritmi, e gli oggetti sono la parte fondamentale della costruzione logica\);
2. ogni oggetto è istanza di una **classe**;
3. ogni classe è legata alle altre attraverso una relazione detta **eredità**.

Se in un programma manca anche solo una di queste caratteristiche, non lo si può definire object-oriented.

Tra i molti che hanno cercato di dare, con esiti alterni, una definizione delle caratteristiche che un linguaggo deve avere per essere esso stesso definito object oriented, è inreressante la prospettiva di Cardelli e Wegner \(_On Understanding Types, Data Abstraction, and Polymorphism_ – Computing Surveys, Vol 17 n. 4, pp 471-522\) per i quali un linguaggio può essere definito come orientato agli oggetti se e solo se soddisfa le seguenti caratteristiche:

1. Supporta l’astrazione dei dati in una forma che permetta di parlare di strutture per le quali è definito un insieme di **operazioni** che possono ad essa essere applicate ed è al contempo in grado di garantire l’isolamento dello **stato** delle variabili interne alla struttura.
2. Gli oggetti hanno dei **tipi** \(detti classi\) associati che ne definiscono i possibili comportamenti.
3. I tipi possono essere messi in una relazione di interdipendenza, detta **eredità** che permette ad ogni tipo di poter ereditare attributi da altri tipi che in questo caso sono detti “super-classi” Questa caratteristica di un linguaggio di programmazione apparentemente fumosa risulta chiara se si pensa che significa che in un linguaggio OO deve essere possibile esprimere la frase “è un” come relazione fra le classi.

Per esempio la “Gibson Les Paul” è una chitarra elettrica, che è una chitarra, che è uno strumento musicale e, nell’immaginario caso in cui si stia pensando ad un sistema di acquisti online, è un oggetto di un dato peso e con un imballo di una data dimensione.

La relazione di **eredità tra classi** tanto importante da far concludere che se un linguaggio di programmazione consente di creare tipi \(indi classi\) ma non supporta il concetto di eredità, non lo si può definire object oriented.

## OOD: Object-Oriented Design

Possiamo definire l’**object-oriented design** come una metodologia di progettazione che comprende il processo di decomposizione ad oggetti e una notazione per rappresentare modelli logica e fisica nonché gli aspetti statici e dinamici del sistema in fase di progettazione.

C’è da osservare che la definizione di OOD prevede una decomposizione delle problematiche in oggetti \(in contrapposizione al design procedurale che mira a decomporre le problematiche in algoritmi\) e che già nella sua definizione prevede l’adozione di una **notazione** \(astratta, cioè slegata dal particolare linguaggio di programmazione che verrà eventualmente usato alla fine per l’implementazione del design\) atta a descrivere le interazioni tra gli oggetti ed a comunicare e modellare le peculiarità del sistema in esame.

Questa notazione astratta è oggi universalmente riconosciuta essere il linguaggio di modellazione detto [UML](http://www.html.it/guide/guida-uml/): Unified Modelling Language del quale cercheremo di dare rudimenti pratici nelle prossime sezioni.

## OOA: Object-Oriented Analysis

**Object-oriented analysis** è l’analisi di un problema e la costruzione di modelli del mondo reale con una visione orientata agli oggetti: in altre parole è una _metodologia di analisi che esamina le necessità di un problema dal punto di vista delle classi e degli oggetti_.

## Quale relazione lega OOA, OOD e OOP?

Fondamentalmente, i prodotti dell’analisi orientata agli oggetti servono come modelli da cui si possa avviare una progettazione orientata agli oggetti; i prodotti di progettazione orientata agli oggetti possono poi essere utilizzati come modelli per l’attuazione completa di un sistema utilizzando metodi di programmazione orientati agli oggetti.

Anche se in teoria questo approccio che prevede OOA seguita da OOD e OOP \(con successive iterazioni e revisioni\) sembra una colossale \(e burocratica\) complicazione ed un inutile allungamento dei tempio di sviluppo, l’esperienza conferma che seguendo buone pratiche OO il software prodotto risulta migliore e soprattutto mantenibile e modificabile.

## Caratteristiche dello “stile” object oriented

Si individuano solitamente nello stile Object Oriented quattro elementi caratterizzanti:

* Astrazione;
* Encapsulation \(Incapsulamento\);
* Gerarchia;
* Modularità.

Ogni modello che manca di anche solo una di queste caratteristiche non può essere definito “orientato agli oggetti”.

## Astrazione, applicazione pratica alla progettazione

L’astrazione ci consente di evidenziare le caratteristiche fondamentali di un oggetto e di classificarlo simile ad altri dello stesso tipo e distinto da tutti gli altri tipi e quindi ci consente di tracciare confini concettuali ben definiti per descriverlo all’interno di un certo **contesto di osservazione** che ci interessa.

Stabilire il giusto insieme di elementi di astrazione per un dato oggetto è il problema centrale nella progettazione object-oriented. Cerchiamo di rendere più chiaro quanto appena detto con un esempio.

Riprendiamo l’esempio della chitarra Gibson e consideriamo questo oggetto nel contesto della realizzazione di un sistema per il trasporto di beni. Sarebbe probabilmente una buona astrazione quella di considerare lo strumento musicale come appartenente alla categoria degli oggetti trasportabili, con associato un peso ed un volume. Anche l’appartenenza alla categoria degli oggetti fragili e di valore potrebbe essere corretta ma al fine della specifica prospettiva non avrebbe molto senso. Si può classificarlo poi come oggetto ad uso dei gruppi musicali pop oppure in quello degli strumenti a corda: entrambe le affermazioni sono vere ma non pertinenti alla astrazione consona al problema in oggetto.

È importante osservare che l’astrazione va utilizzata come strumento che permette di **focalizzare l’attenzione su una visione esterna di un oggetto**, in modo da separare quella che è l’implementazione di un comportamento dal suo ruolo nella dinamica globale di un determinato processo.

Sempre parlando della nostra chitarra nel contesto di un software per l’impacchettamento di item in immaginari mezzi di trasporto, il fatto di averla classificata come “oggetto fragile” significherà che dovrà essere possibile chiedere all’oggetto Chitarra quale sia il massimo peso che può sopportare \(quando impilata in un container\) che servirà per schedulare l’ordine degli oggetti quando caricati. Non conta come l’oggetto Chitarra calcolerà il massimo peso sostenibile \(implementazione\), quello che è importante è che l’oggetto sia in grado di darci l’informazione \(interazione\).

Al contempo, se tra gli oggetti da trasportare si aggiungeranno \(anche in un secondo momento\) le “scatole per uova” non sarà necessario al sistema di conoscere come il massimo peso sostenibile sia calcolato nel caso delle uova ed in cosa questo differisca da quello per le chitarre \(come avremmo forse dovuto fare in un contesto procedurale se avessimo previsto una procedura per calcolare il massimo peso sostenibile di un dato collo\) ma basterà che anche le scatole per uova implementino la medesima interfaccia delle chitarre, cioè quella prevista per gli oggetti fragili.

## Incapsulamento

Come si capisce dall’esempio dei “colli fragili” c’è un secondo concetto che scaturisce naturalmente dall’astrazione e che è rappresentato dal fatto che le implementazioni del calcolo del massimo peso sostenibile per le chitarre e le uova vengono definite in opportune sezioni “private” cioè pertinenti solamente a quegli specifici oggetti, incapsulati insomma in precise aree di codice.

I concetti di astrazione e di incapsulamento sono quindi complementari: l’astrazione focalizza l’attenzione sul comportamento e le caratteristiche osservabili di un oggetto, mentre l’incapsulamento si concentra sull’implementazione che riesce a riprodurre queste caratteristiche.

L’incapsulamento è molto spesso ottenuto attraverso l’**aggregazione di informazioni**, che è il processo di nascondere tutte le informazioni private di un oggetto che non contribuiscono a definire quelle che sono le sue caratteristiche essenziali nell’interazione con le altre entità del sistema in esame.

Tipicamente la struttura di un oggetto viene mantenuta nascosta, così come l’implementazione dei suoi metodi consentendoci di creare delle vere e proprie barriere fra i diversi tipi di astrazioni che possono essere definite per un oggetto, senza creare alcun tipo di confusione e soprattutto senza dover duplicare le informazioni che e diverse astrazioni condividono.

In definitiva si può definire l’incapsulamento come segue: l’incapsulamento è il processo di suddivisione degli elementi di un’astrazione che ne costituiscono la struttura e il comportamento; incapsulamento serve a separare l’interfaccia costruita per un certo tipo di astrazione e la sua attuazione.

Britton e Parnas chiamano questi elementi incapsulati i "segreti" di un’astrazione.

## Gerarchia

“Abstraction is a good thing, but in all except the most trivial applications, we may find many more different abstractions than we can comprehend at one time.”

Nella maggior parte dei problemi che si affrontano non esiste una unica astrazione da tenere in considerazione ma spesso ne esistono molteplici e la possibilità di organizzarle in gerarchie è di vitale importanza per una modellazione efficace.

L’incapsulamneto ci aiuta nella gestione di questa complessità cercando di mantenere nascosta all’utente finale la nostra astrazione e la modularità ci da il modo per gestire le relazioni logiche dell’astrazione.

Tuttavia questo non sembra essere abbastanza, infatti molto spesso molte astrazioni formano insieme una gerachia e identificando queste gerarchie riusciamo a semplificare enormemente il problema.

Le gerarchie che comunemente si descrivono nella programmazione sono quelle che potremmo definire strutturali \(un determinato oggetto “è un” cioè “appartiene alla tal categoria”\) oppure quelli compontamentali “si comporta come un”.

Le chitarre nel sistema per il carico dei container è un item fragile ma magari ha senso dire che si comporta anche come un oggetto vendibile e che quindi ha associato un metodo che ne ritorna il prezzo utilizzabile per redigere il valore degli item che compongono un carico.

La distinzione tra “è un” e “si comporta come un” scompare quasi completamente in linguaggi che, come il C++, supportano il concetto di ereditarietà multipla ma è invece importante da tenere a mente in Java che forza una unica appartenenza strutturale.

## Modularità

Il significato di modularità può essere visto come l’azione di partizionare un programma \(o se vogliamo un qualsiasi problema\) in componenti che riescano a ridurne il grado di complessità.

Risulta infatti evidente nella modellazione di sistemi complessi che la sola astrazione in entità \(e gerarchie\) e l’incapsulamento non bastano per renndere trattabili alcuni problemi ma è necessario ricorrere anche alla modularizzazione, cioè alla individuazione di gruppi di entità \(classi\) affini per funzionalità, area di utilizzo, comportamento e conseguente divisione del problema in sottoproblemi più facilmente affrontabili.

Questa partizione in moduli consente anche di creare un certo numero elementi ben definiti e separati all’interno del programma stesso \(eventualmente anche compilati separatamente\), ma che sia possibile connettere fra di loro per orchestrare una soluzione globale al problema in esame.

Determinare come modularizzare un determinato problema è una operazione certamente iterativa ed è caratterizzata dal medesimo livello di complessità della astrazione: quest’ ultima consiste nella dereminazione delle entità che è necessario modellare nelal soluzione del problema in oggetto mentre la modularizzazione consiste nel determinare gruppi ‘assimilabilì di oggetti da relegare in un modulo da sviluppare separatamente.

* [Developing Applications with Java and UML](http://www.amazon.com/Developing-Applications-Java¿-Paul-Reed/dp/0201702525/)

