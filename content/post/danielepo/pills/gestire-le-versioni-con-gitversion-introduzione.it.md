---
title: "Gestire le versioni con gitversion - Introduzione"
date: 2020-10-09T00:00:01+01:00
authors: ['danielepo']
categories: 
- "Devops pills"
tags: 
- "devops"
- "pill"
- "git"
- "gitversion"
- "versioning"
summary: 'Gestire le versioni delle librerie è fondamentale ma non è una pratica fatta da molti, vediamo come gitversion ci viene in aiuto'
---

Nella seguente pillola discutiamo dei principi base del perchè conviene versionare e diamo un primo sguardo a GitVersion che ci faciliterà nell'implementare una buona strategia di versionamento.

{{< youtube id="uy63C5CEyEA" >}}


## Transcript del video
Buongiorno a tutti e benvenuti a una nuova serie di DevOps pills, in questa serie parleremo di versionamento del software.

Il concetto di controllo di versione nasce ben prima del suo utilizzo in ambito di ingegneria del software, infatti veniva già usato per gestire l'evoluzione di disegni su carta di progetti ingegneristici di vario genere. Ogni disegno veniva archiviato con un numero di versione e possibilmente una data; di uno stesso disegno potevano esistere evoluzioni differenti in base alle necessità e versioni "definitive" venivano protocollate con un numero di protocollo accompagnato dalla data di protocollazione. 

È chiaro come questo paradigma si adatti molto bene al mondo software in cui il codice è in continua evoluzione e poter tornare in dietro a una versione o sviluppare un ramo parallelo può facilitare il lavoro dei programmatori. Ecco quindi che sono nati diversi strumenti per controllare l'evoluzione del software tra cui troviamo TFS, SVN e Git. Purtroppo però questi software si limitano a tracciare la storia delle modifiche del codice ma non ci forniscono alcuna informazione su quale versione dare al programma o libreria che viene generato da questo codice, a meno che non si usi l'id del commit. Tuttavia, questo id non è spesso parlante. 

Cerco di spiegarmi. Se sto sviluppando una funzionalità piuttosto complessa molto probabilmente farò vari commit durante la fase di sviluppo ma il software passerà dalla versione A di partenza alla versione B, che indica l'aggiunta della funzionalità, solamente quando la stessa sarà pronta per essere rilasciata.

La mancanza di uno standard o una best practice ha portato ogni team o azienda a studiare il proprio metodo reinventando la ruota o addirittura ignorando completamente il problema e non versionando in alcun modo le proprie librerie. D'altro canto, quando si tratta di codice interno che senso ha dare una versione a una libreria? Non basta fornire sempre l'ultima versione agli utilizzatori? Oppure non è più facile mettere tutte le librerie in una solution unica in modo da non doverle versionare? 

Come potete immaginare questa linea di pensiero porta a risultati catastrofici e alcuni li ho visti di persona: 
 * Dll senza informazione di versione passate via email
 * Dll messe in controllo di versione per essere sicuro che quella versione del codice venga fatta eseguire con quella particolare dll
 * Confrontare le "versioni" delle dll in base alla loro dimensione e ipotizzare che quella più grossa sia la più precente perché "contiene più funzionalità"
 * Solution così onerose che aprire tutti i progetti provocherebbe crash sul computer con tutti i problemi derivanti

È chiaro che bisogna trovare un modo per gestire il versionamento. Una serie di regole e convenzioni che aiutino gli sviluppatori a decidere se vale la pena passare alla versione successiva di una dipendenza o meno. 

La convenzione più comunemente utilizzata al giorno d'oggi è detta **Semantic Versioning** o, per gli amici, **SemVer** e trovate tutte le informazioni a riguardo al sito [semver.org](semver.org), link nella descrizione del video. Ecco un riassunto delle regole di base.

In pratica nella Semantic Versioning la versione è composta da tre numeri separati da punti detti rispettivamente **Major**, **Minor** e **Patch**, a cui è possibile accodare etichette per aggiungere altre informazioni utili, per esempio versione "1.5.3-beta". La Semantic Versioning è una serie di regole su come incrementare i differenti valori Major, Minor e Patch in base a cosa è stato modifcato nel codice. In poche parole:

* Incrementate la **Patch** quando fate piccole modifiche e correzioni di bug che non influenzano come il mondo esterno interagisce con il vostro programma o libreria

* Incrementate la **Minor** quando fate modifiche o aggiunte funzionalità senza però rompere la compatibilità con le versioni precedenti

* E infine, incrementate la **Major** quando fate modifiche che rendono parti del nuovo codice non più compatibile con le versioni precedenti e quindi che richiederebbero modifiche da parte dei programmi che dipendono dalla vostra libreria 

Seguendo questa convenzione un utilizzatore della vostra libreria riesce a identificare a colpo d'occhio se può aggiornare la versione senza sorprese o se invece gli conviene leggere attentamente i Change Log.

Ad esempio, immaginiamo di avere una libreria che fa i calcoli di conversione tra varie monete e la versione di questa libreria è "1.5.3"; a questo punto, arriva una segnalazione di errore su una stringa. Correggo e creo la versione "1.5.4", incrementando di uno la patch.

Nel caso in cui stia creando una nuova funzionalità (per esempio aggiungo una moneta alla lista di monete per cui posso fare la conversione) dovrò incrementare la Minor azzerando allo stesso tempo la Patch. Creo quindi la versione "1.6.0".

Ora, immaginiamo di dover invertire l'ordine dei parametri della funzione di calcolo. Tale modifica renderebbe non retrocompatibile il codice dei nostri utilizzatori e richiederebbe loro di modificare il codice. Per questa ragione aggiorneremmo la Major azzerando Minor e Patch allo stesso tempo. Quindi la nostra versione diventa "2.0.0".

spero sia chiaro quanto una convenzione di questo genere sia utile per gli sviluppatori che ora possono decidere se possono aggiornare liberamente le librerie o se è meglio che si prendano un po' di tempo per farlo per evitare sorprese. 

Ora che conosciamo alcuni motivi per cui conviene gestire le versioni del software, in particolare delle librerie, possiamo vedere come automatizzare la creazione della versione grazie a un programma chiamato gitversion. Come potete immaginare dal nome gitversion sfrutta git quindi se ancora non state usando git, beh, questo è un altro tra i tanti motivi per aggiornare il vostro sistema di controllo di versione software. 

Per ora concentriamoci sull'installazione e sull'utilizzo in locale e in un prossimo video vedremo come utilizzarlo nella nostra pipeline di build e rilascio.

Nel sito di documentazione di gitversion questo viene definito semplicemente come uno strumento utile a implementare il semantic versioning nei tuoi progetti. Sebbene questa definizionre riassuma perfettamente l'utilità di gitversion, per apprezzarlo al massimo è meglio vederlo in azione. Procediamo quindi a installarlo.

Come vi dicevo prima voglio usarlo localmente e successivamente vedremo altri metodi di utilizzo, quindi per trovare le istruzioni su come installarlo vado nella pagina "Getting started e poi Command line"

Qua vedete che é possibile installare git version con Chocolatey se siete su windows oppure con homebrew se siete su Mac. Io sono su windows e quindi vi mostro come installarlo con chocolatey (per inciso, se non usate chocolatey non sapete cosa vi perdete)

Semplicemente apro powershell in modalità amministratore e lancio il commando `choco install gitversion.portable -y` e premo invio.  Possiamo vedere che l'istallazione è partita e una volta completata posso confermare che l'installazione sia andata a buon fine lanciando il comando  `gitversion /version`

Un modo alternativo che non richiede l'utilizzo di un package manager, nel caso non siate abilitati a usarlo è quello di scaricare il pacchetto nuget GitVersion.CommandLine e dentro trovate l'eseguibile. Questa procedura non è documentata da nessuna parte quindi prendetela con le pinzette

Ora che l'abbiamo installato sulla nostra macchina è il momento di cominciare a trarne dei vantaggi. 

Per vederlo in azione creiamo un progetto dotnet core con la CLI , niente di esoterico, una semplice console application. Facciamo `dotnet new console` e vediamo che viene creato il programma, poi con `dotnet run` lo eseguiamo e vediamo il classico "Hello world!". 

Ora, per usare gitversion abbiamo bisogno di … git, quindi inizializziamo un repositori con `git init`. Andiamo su [github.com/github/gitignore](https://github.com/github/gitignore) e scarichiamo un gitignore adatto al nostro ambiente. Scegliamo visualstudio e lo salviamo direttamente nella cartella del progetto togliendo tutto quello che sta prima di `.gitignore`.

Ora possiamo fare `git add .` e `git commit -m "first commit"`

Ora che abbiamo un repository con almeno un commit proviamo a eseguire gitversion. Possiamo da subito vedere che il commando torna una serie di informazioni sulla versione. Prima di tutto come default propone la versione "0.1.0". 

Alcune delle informazioni interessanti che possiamo trovare nella risposta sono
 * **BranchName** che fornisce il nome del branch
 * Lo **Sha** del commit cosi come la versione corta in **ShortSha**
 * **FullSemVer** che ha questo curioso `+0` alla fine che vedremo tra un po' a cosa serve
 * **InformationalVersion** che fornisce una label completa con tutte le informazioni del commit

Ora apriamo vscode e facciamo una piccola modifica. Salviamo il file e facciamo il commit. 

Bene. Ora proviamo a fare di nuovo `gitversion`. Vediamo che la nostra semver non è cambiata, ci troviamo sempre ad avere "0.1.0". Ma il valore di **BuildMetaData** è cambiato, è passato da 0 a 1 ed, in particolare, questo valore viene utilizzato per comporre FullSemVer.

Di default per far incrementare la semver dobbiamo usare i tag nei commit. Infatti se creiamo un tag con `git tag` e poi proviamo a lanciare `gitversion` vediamo che la semver diventa "0.1.1" come il tag che abbiamo aggiunto.

Gitversion è molto configurabile e di default supporta alcune modalità di utilizzo. Vedremo nei prossimi video quali sono i modi di utilizzo di default e come configurarlo, e vedremo anche come utilizzarlo nella nostra pipeline di CI/CD

Per ora è tutto, se vi è piaciuto il video mettete un like. Per avere altre pillole su DevOps, iscrivetevi al canale e cliccate sulla campanellina per rimanere sempre aggiornati

Alla prossima.