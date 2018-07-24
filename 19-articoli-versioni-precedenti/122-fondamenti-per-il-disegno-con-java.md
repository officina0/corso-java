# 122 Fondamenti Per Il Disegno Con Java

Nel capitolo precedente abbiamo visto come usare i componenti GUI di Abstract Window Toolkit per costruire delle interfacce utente grafiche per i nostri programmi, e abbiamo detto che un componente GUI è in sostanza un oggetto con delle qualità grafiche.

Il componente GUI è un componente di alto livello del pacchetto awt, esso è costruito usando i componenti a basso livello del pacchetto, ovvero le primitive grafiche, e gli eventi.

Pensiamo ad esempio ad un bottone, esso graficamente non è altro che una scatola con un testo all’interno, che cambia aspetto quando viene schiacciato, ovvero quando sente l’evento ActionEvent di esso.

Usando le caratteristiche di basso livello, possiamo costruirci i nostri GUI personalizzati, oppure modificare quelli esistenti secondo i nostri bisogni, ma non solo, possiamo estendere un componente particolare, un componente tela, su cui possiamo disegnare quello che vogliamo e metterlo nelle nostre interfacce come un qualsiasi altro GUI.

Se usiamo gli applet piuttosto che le applicazioni possiamo evitare di usare la tela, in quanto l’applet ha implicitamente una tela. Ma iniziamo a vedere concretamente come è possiblie disegnare su un componente qualsiasi o su un applet, iniziamo a parlare delle applet, per poi estendere i ragionamenti fatti ai componenti in generale.

