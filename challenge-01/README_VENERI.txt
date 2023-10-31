Questa cartella contiene uno script Jupyter per analizzare l'immagine ed estrarne delle informazioni che possono essere utili (First_challenge_AV.ipynb). Si utilizza in modo estensivo la libreria gdal.
L'idea dello script è quella di estrarre delle geometrie shapefile dall'immagine rasterizzata. Una volta ottenuto il relativo shapefile, possiamo estrarre numero di elementi, geometrie, etc.




                           ####### STRUTTURA DEL CODICE #########
La sezione "Image bands" è un primo step per verificare quali bande siano presenti nell'immagine. In particolare, ce ne sono 4, 3 rgb ed una alpha band per distinguere le parti da escludere nell'immagine rettangolare.

Un altro blocco "Creating histograms" serve per estrarre la frequenza dei pixel e la loro intensità per ogni banda, è utilissimo per chiarire quali sezioni di esse sia meglio considerare per estrarre informazioni sui vari elementi che compongono l'immagine.

Prima di continuare con l'analisi vera e propria, eseguiamo una equalizzazione dell'istogramma, dove questo viene "appiattito" il più possibile, permettendo una separazione migliore tra zone d'ombra e zone di luce. Dovrebbe aiutare a distinguere meglio le parti scure con alberi da altre parti scure. Ad occhio, un primo picco più scuro appare nel range 50-60 Pixel Value, useremo questa gap per estrarre la sezione di alberi.

Proseguo con la classificazione. Ho optato per una classificazione dell'immagine in bianco e nero, in cui mi focalizzo sulla sezione dell'istogramma dove ritengo sia più probabile estrarre gli alberi. Questo viene fatto nella sezione "Classify images skewed". In particolare, considero una sotto-banda nel range 52-54, che sembra ottimale. Viene prodotta l'immagine "classified_type_2.tiff"

Lo step successivo nella sezione "Extracting features from images" è molto schematica e standardizzata. Vengono estratti in forma di shapefile gli elementi chiari dell'immagine classificata nella sezione precedente.

Infine, nella sezione "Analysis of the shapefile", cerchiamo di estrarre qualche informazione. In particolare, capiamo il numero di elementi (ovvero il numero di alberi), e il rapporto tra l'area coperta da alberi e l'area totale dell'immagine.

                            ####################################

     				     #### COMMENTI ####

Il numero di elementi totali sembra essere 61809. Mi sembrano troppi. Identifico due problemi: Il primo è l'errata identificazione di elementi scuri, la quale avviene soprattutto al centro dell'immagine, il secondo è la (possibile) errata suddivisione di un singolo elemento in più elementi, se questo contiene zone più luminose e meno luminose. Non credo esista soluzione con i metodi semplici forniti da librerie come gdal o simili, i quali fanno fede solamente a bande di colori e intensità luminose.

La soluzione al primo problema può essere l'esclusione di elementi la cui area è inferiore a quella media di un albero. Questo però non risolverebbe il secondo problema.
La soluzione più adeguata è secondo me l'utilizzo di un algoritmo neural network. Infatti, in quest'ottica, diverse immagini vengono analizzate nella maniera descritta nel codice Jupyter sviluppato in questa sede, ma vengono successivamente confrontate con risultati noti. Questa è una fase di training dei dati. La rete neurale viene quindi allenata ad escludere automaticamente gli elementi di disturbo nell'immagine.

Viene infine estratto il rapporto tra l'area occupata dagli elementi e quella totale dell'immagine. Il risultato si aggira al 0.8%. Anche in questo caso, mi sembra un risultato errato. Sembra suggerire che singoli elementi vengano suddivisi in sotto-elementi, lasciando degli spazi vuoti in un singolo oggetto-albero. La soluzione implicante una rete neurale rimane, a mio avviso, la più valida.