# Generazione del file XML

Per generare un feed (il file si chiamerà ad esempio `feed.xml`) che superi i controlli di validazione ([Link](https://validator.w3.org/feed/check.cgi)) occorre:

- tutti i campi `pubDate` deve contenere una data in formato RFC 822; ad esempio:

    ```
    <pubDate>Sat, 4 Jul 2020 00:00:00 +0200</pubDate>
    ```

- possiamo usare l'elemento `category` in modo che il dominio rimandi al podcast della categoria:

    ```
    <category domain="https://testup.unisalento.it/podcast/light">Light</category>
    ```

- dobbiamo generare un elemento `guid` con un permalink univoco alla notizia:

    ```
    <guid isPermaLink="false">
        id-univoco
    </guid>
    ```

    Occorre che l'attributo isPermalink sia falso, in modo da poter utilizzare un id alfanumerico qualsiasi e non obbligatoriamente un link. Questo sarà poi utilizzato per generare un link che consenta di aprire la notizia corrispondente.
- per poter generare il link di download al file mp3, possiamo utilizzare il seguente metodo: poichè il link alla puntata in mp3 è un link alla playlist m3u8, si fa in modo che sostituendo nel link la string `m3u8` con la stringa `mp3` si ottenga il link di download. Pertanto nel filesystem del generatore andrà creato un file mp3 nella stessa cartella del file m3u8 e con lo stesso nome (la parte precedente al prefisso).

# Generazione dell'archivio

Man mano che genereremo contenuti per il podcast, il feed xml conterrà solo gli ultimi `n` articoli. Gli articoli più vecchi potrebbero però essere stati condivisi o i link conservati tra i bookmark degli utenti. Per consentire a questi link di essere attivi occorre generare un file `.xml` aggiuntivo per ogni articolo. Il file avrà nome `news-"id-univoco".xml` e verrà salvato insieme a `feed.xml`. 
Il contenuto del file sarà uguale a `feed.xml` ma conterrà solo i dati del singolo articolo corrispondente a `id-univoco`. Il template RSS si occuperà di caricare il giusto file `.xml` visualizzando anche le notizie non presenti nel feed xml.

# Idea: navigazione tra i precedenti articoli

Ovviamente possiamo anche generare dei feed di archivio contenenti ciascuno `n` notizie. Questi potranno essere numerati progressivamente (ad esempio `feed-00001.xml`, `feed-00002.xml`, ecc) e l'ultimo di questi potrà apparire in un elemento del feed xml attuale in modo da consentire una navigazione all'indietro nei precedenti feed. Per questa feature il template non è però pronto.