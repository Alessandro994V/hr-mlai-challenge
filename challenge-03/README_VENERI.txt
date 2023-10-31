Questa challenge non mi è molto chiara. Il notebook nella cartella è un tentativo di
analizzare i dati formato json che mi avete fornito.

Innanzitutto ho letto i dati sul terreno ("https://nft.17tons.tech/explorer/lands/5") che forniscono informazioni generali sul terreno selezionato, ma soprattutto descrivono la geometria
del terreno. Non vengono forniti altri elementi come l'umidità o l'acidità del terreno

Il secondo set di dati ('https://nft.17tons.tech/explorer/trees/land/5') fornisce molte più informazioni, andando sullo specifico dell'analisi dei singoli alberi piantati. Non ne ricavo però alcuna informazione utile se non la posizione degli alberi. La maggior parte degli attributi è vuoto, oppure le informazioni sono le stesse per ogni elemento.

Mi sono limitato a convertire i dati da formato json a pandas per migliorarne la chiarezza.