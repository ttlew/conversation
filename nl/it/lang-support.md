---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Lingue supportate
Il servizio {{site.data.keyword.conversationshort}} supporta le lingue elencate. Le singole funzioni del servizio sono supportate in misura maggiore o minore per ciascuna lingua.
{: shortdesc}

Nella seguente tabella, il livello di supporto per lingua e funzione è indicato da questi codici:

- **GA** - La funzione è generalmente disponibile e supportata per questa lingua. Nota che le funzioni possono continuare ad essere aggiornate anche dopo che sono generalmente disponibili.
- **Beta** - La funzione è supportata solo come release Beta ed è ancora in fase di test prima di essere resa generalmente disponibile in questa lingua.
- **Vuoto** - L'assenza di qualsiasi codice indica che una funzione non è disponibile in questa lingua.

|                  | **[Definizione di intenti](intents.html)**, **[entità](entities.html)** e **[dialogo](dialog-build.html)** | **[Punteggio assoluto e 'Contrassegna come irrilevante'](intents.html#mark-irrelevant)** | **Entità di sistema ([numero](system-entities.html#sys-number), [valuta](system-entities.html#sys-currency), [percentuale](system-entities.html#sys-percentage), [data, ora](system-entities.html#sys-datetime))** | **[Corrispondenza fuzzy di entità](entities.html#fuzzy-matching)** | **[Entità basate sul modello](entities.html#pattern-entities)** |
|:---|:---:|:---:|:---:|:---:|:---:|
| **Inglese (en)**                   | GA | GA | GA </br> Beta ([posizione](system-entities.html#sys-location), [persona](system-entities.html#sys-person)) | Beta (Stemming, errore di ortografia e corrispondenza parziale) | Beta |
| **Arabo (ar)**                    | GA | Beta | Beta | Beta (Solo errore di ortografia) | Beta |
| **Cinese (semplificato) (zh-cn)**   | Beta | Beta | Beta |  | Beta |
| **Cinese (tradizionale) (zh-tw)**  | Beta | Beta |  |  | Beta |
| **Ceco (cs)**                     | Beta | Beta | Beta | Beta (Solo errore di ortografia) | Beta |
| **Olandese (nl)**                     | Beta | Beta |  |  | Beta |
| **Francese (fr)**                    | GA | GA | GA | Beta (Solo errore di ortografia) | Beta |
| **Tedesco (de)**                    | GA | GA | GA | Beta (Solo errore di ortografia) | Beta |
| **Italiano (it)**                   | GA | GA | GA | Beta (Solo errore di ortografia) | Beta |
| **Giapponese (ja)**                  | GA | GA | GA | Beta (Solo errore di ortografia) | Beta |
| **Coreano (ko)**                    | GA | GA | Beta | Beta (Solo errore di ortografia) | Beta |
| **Portoghese (brasiliano) (pt-br)** | GA | GA | GA | Beta (Solo errore di ortografia) | Beta |
| **Spagnolo (es)**                   | GA | GA | GA | Beta (Solo errore di ortografia) | Beta ||

**Nota:** il servizio {{site.data.keyword.conversationshort}} supporta più lingue come indicato, ma l'interfaccia degli strumenti stessa (descrizioni, etichette, ecc.) è in inglese. Tutte le lingue supportate possono essere inserite e addestrate attraverso l'interfaccia inglese.

## Modifica di una lingua dello spazio di lavoro

Una volta creato uno spazio di lavoro, la sua lingua non può essere modificata. Se è necessario modificare la lingua supportata di uno spazio di lavoro, l'utente deve scaricare lo spazio di lavoro. Quindi, deve modificare il file JSON risultante in un editor di testo, cercando una proprietà JSON chiamata `language`.

La proprietà `language` deve essere impostata sulla lingua originale dello spazio di lavoro; ad esempio, inglese sarà `en`. Modifica il valore di questa proprietà, cambiandolo nella lingua desiderata (`fr` per francese, `de` per tedesco, ecc). Salva le modifiche nel file JSON e importa il file modificato nella tua istanza del servizio {{site.data.keyword.conversationshort}}.

## Configurazione di lingue bidirezionali
{: #configuring-bi-directional}

Per le lingue bidirezionali, come l'arabo, puoi modificare le preferenze dello spazio di lavoro di conseguenza. Dalla scheda dello spazio di lavoro, seleziona il menu a discesa *Azioni* e seleziona **Preferenze bidi** (questa opzione è disponibile solo per gli spazi di lavoro impostati su una lingua bidirezionale):

![Preferenze bidi](images/bidi_prefs.png)

Scegli tra le seguenti opzioni per il tuo spazio di lavoro:

- **Direzione GUI**: specifica la direzione di layout degli elementi, come pulsanti o menu, nell'interfaccia utente grafica. Scegli `LTR` (da sinistra a destra) o `RTL` (da destra a sinistra). Se non specificato, lo strumento segue l'impostazione della direzione della GUI del browser Web.
- **Direzione testo**: specifica la direzione del testo immesso. Scegli `LTR` (da sinistra a destra) o `RTL` (da destra a sinistra) oppure seleziona `Auto` che sceglierà automaticamente la direzione del testo in base alle impostazioni del tuo sistema. L'opzione `None` visualizzerà il testo da sinistra a destra.
- **Forma numerica**: specifica quale forma di numeri utilizzare quando si presentano cifre regolari. Scegli tra `Nominal`, `Arabic-Indic` o `Arabic-European`. L'opzione `None` visualizzerà i numeri occidentali.
- **Tipo di calendario**: specifica come scegli di filtrare le date nella IU dello spazio di lavoro. Scegli `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura` o `Gregorian`. **Nota**: questa impostazione non si applica al pannello "Provalo".

![Opzioni bidi](images/bidi_opts.png)

Al termine delle selezioni, fai clic su **Aggiorna** per salvare e ritornare alla scheda dello spazio di lavoro.

## Utilizzo di caratteri accentati
{: #working-with-accents}

In un contesto di conversazione, gli utenti possono utilizzare o meno accenti durante l'interazione con il servizio di conversazione. Di conseguenza, sia le versioni accentate che quelle non accentate delle parole possono essere trattate allo stesso modo per il rilevamento di intenti e il riconoscimento di entità.

Tuttavia per alcune lingue, come lo spagnolo, alcuni accenti possono modificare il significato dell'entità. Pertanto, per il rilevamento di entità, sebbene l'entità originale possa implicitamente avere un accento, il servizio può anche abbinare la versione non accentata della stessa entità, ma con un punteggio di affidabilità leggermente inferiore.

Ad esempio, per la parola "barrió", che ha un accento e corrisponde al passato del verbo "barrer" (spazzare), il servizio può abbinare anche la parola "barrio" (quartiere), ma con una confidenza leggermente inferiore.

Il sistema fornirà punteggi di affidabilità più alti nelle entità con corrispondenze esatte. Ad esempio, `barrio` non verrà rilevato se `barrió` è nel set di addestramento e `barrió` non verrà rilevato se `barrio` è nel set di addestramento.

Dovresti addestrare il sistema con i caratteri e gli accenti appropriati. Ad esempio, se prevedi `barrió` come risposta, non devi inserire `barrió` nel set di addestramento.

Sebbene non sia un segno di accento, lo stesso vale per le parole che utilizzano, ad esempio, la lettera spagnola `ñ` rispetto alla lettera `n`, ad esempio "uña" rispetto a "una". In questo caso, la lettera `ñ` non è semplicemente una `n` con un accento; è una lettera unica e specifica per lo spagnolo.

Puoi abilitare la corrispondenza fuzzy se ritieni che i tuoi clienti non utilizzeranno gli accenti appropriati o faranno errori di ortografia (incluso, ad esempio, mettere una `n` al posto di una `ñ`) o puoi includerli esplicitamente negli esempi di addestramento.
