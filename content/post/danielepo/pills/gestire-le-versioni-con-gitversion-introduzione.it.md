---
title: "Gestire le versioni con gitversion - Introduzione"
date: 2020-10-05T00:00:01+01:00
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


## Transcript
Buongiorno a tutti e benvenuti ad una nuova serie di DevOps pills, in questa serie parleremo di versionamento del software.

Il concetto di controllo di versione nasce ben prima del suo utilizzo in ambito di ingegneria del software, infatti veniva già usato per gestire l'evoluzione di disegni su carta di progetti ingegneristici di vario genere. Ogni disegno veniva archiviato con un numero di versione e possibilmente una data; di uno stesso disegno potevano esistere evoluzioni differenti in base alle necessità e versioni "definitive" venivano protocollate con un numero di protocollo accompagnato dalla data di protocollazione. 

È chiaro come questo paradigma si adatti molto bene al mondo software in cui il codice è in continua evoluzione e poter tornare in dietro ad una versione o sviluppare un ramo parallelo può facilitare il lavoro dei programmatori. Ecco quindi che sono nati diversi strumenti per controllare l'evoluzione del software tra cui troviamo TFS, SVN e Git. Purtroppo però questi software si limitano a tracciare la storia delle modifiche del codice ma non ci forniscono alcuna informazione su quale versione dare al programma o libreria che viene generato da questo codice, questo se non usiamo l'id del commit, però questo id il più delle volte non è per niente parlante. 

Cerco di spiegarmi. Se sto sviluppando una funzionalità piuttosto complessa molto probabilmente farò vari commit durante la fase di sviluppo ma il software passerà dalla versione A di partenza alla versione B, che indica l'aggiunta della funzionalità, solamente quando la stessa sarà pronta per essere rilasciata.

La mancanza di uno standard od una best practice ha significato che ogni team o azienda si è dovuto pensare il proprio metodo spesso reinventando la ruota o addirittura ignorando completamente il problema e non versionando in alcun modo le proprie librerie. D'altro canto, quando si tratta di codice interno che senso dare una versione in una libreria? Non basta fornire sempre l'ultima versione agli utilizzatori? Oppure non è più facile mettere tutte le librerie in una solution unica in modo da non doverle versionare? 

Come potete immaginare questa linea di pensiero porta a risultati catastrofici ed alcuni li ho visti di persona: 
Dll senza informazione di versione passate via email
Dll messe in controllo di versione per essere sicuro che il quella versione del codice venga fatta eseguire con quella particolare dll
Confrontare le "versioni" delle dll in base alla loro dimensione ed ipotizzare che quella più grossa sia la più precente perché "contiene più funzionalità"
Solution così grosse che aprire tutti i progetti farebbe crashare il computer con tutti i problemi derivanti

È chiaro che bisogna trovare un modo per gestire il versionamento. una serie di regole e convenzioni che aiutino gli sviluppatori a decidere se vale la pena passare alla versione successiva di una dipendenza o meno. 

La convenzione più comunemente utilizzata al giorno d'oggi è detta semantic versioning o, per gli amici, SemVer e trovate tutte le informazioni approposito al sito semver.org, link nella descrizione del video. Io ora vi faccio un riassunto delle regole di base: 

in pratica nella Semantic Versioning la versione è composta da tre numeri separati da punti detti rispettivamente Major, Minor e Patch, a cui è possibile accodare ettichette per aggiungere altre informazioni utili, per esempio versione uno punto cinque punto tre trattino beta

la Semantic Versioning è una serie di regole su come incrementare i differenti valori Major, Minor e Patch in base a cosa è stato modifcato nel codice. In poche parole 

Incrementate la Patch quando fate piccole modifiche e correzioni di bug che non influenzato come il mondo esterno interagisce con il vostro programma o libreria

Incrementare la Minor quando fate modifiche o aggiunte funzionalità senza però rompere la compatibilità con le versioni precedenti

ed infine, incrementate la Major quando fate modifiche che rendono parti del nuovo codice non più compatibile con le versioni precedenti e quindi che richiederebbero modifiche da parte dei programmi che dipendono dalla vostra libreria 

Seguendo questa convenzione un utilizzatore della vostra libreria riesce a identificare a colpo d'occhio se può aggiornare la versione senza sorprese o se invece gli conviene leggere attentamente i Change Log.

Per esempio facciamo finta che ho una libreria che fa i calcoli di conversione tra varie monete e la versione di questa libreria è "uno cinque tre", mi segnalano che una stringa ha un errore, quando lo correggo creerò la versione "uno cinque quattro", incrementando di uno la patch.

se invece sto creando una nuova funzionalità per esempio aggiungo una moneta alla lista di monete tra cui posso fare la conversione dovrò incrementare la Minor azzerando allo stesso tempo la Patch, creo quindi la versione uno sei zero.

adesso per qualche strano motivo invertiamo l'ordine in cui passo i dati alla funzione di calcolo, invece di essere quantità, moneta di partenza, moneta di cui voliamo sapere il valore mettiamo le due monete come primi parametri e la quantità la passiamo per ultimo. questa modifica romperebbe il codice dei nostri utilizzatori e richiederebbe loro di modificare il loro codice. per questa ragione aggiorneremmo la Major azzerando minor e patch allo stesso tempo. quindi la nostra versione diventa due zero zero.

spero sia chiaro quanto una convenzione di questo genere sia utile per gli sviluppatori che ora possono decidere se possono aggiornare liberamente le librerie o se è meglio che si prendano un po' di tempo per farlo per evitare sorprese. 

Ora che conosciamo alcuni motivi per cui conviene gestire le versioni del software, in particolare delle librerie, possiamo vedere come automatizzare la creazione della versione grazie ad un programma chiamato gitversion. Come potete immaginare dal nome gitversion sfrutta git quindi se ancora non state usando git, beh, questo è un altro tra I tanti motivi per aggiornare il vostro sistema di controllo di versione software. 

Per ora ci concentriamo sull'installazione e sull'utilizzo in locale e in un prossimo video vedremo come utilizzarlo nella nostra pipeline di build e rilascio.

Nel sito di documentazione di gitversion questo viene definito semplicemente come uno strumento ti aiuta ad implementare il semantic versioning nei tuoi progetti. Sebbene questa reazione riassuma perfettamente l'utilità di gitversion, per meglio apprezzarlo è meglio vederlo in azione. Procediamo quindi ad installarlo

Come vi dicevo prima voglio usarlo localmente e successivamente vedremo altri metodi di utilizzo, quindi per trovare le istruzioni su come installarlo vado nella pagina "Getting started e poi Command line"

Qua vedete che potete installare git version con Chocolatey se siete su windows oppure con homebrew se siete su Mac. Io sono su windows e quindi vi mostro come installarlo con chocolatey (per inciso, se non usate chocolatey non sapete cosa vi perdete)

Semplicemente apro powershell in modalità amministratore e lancio il commando `choco install gitversion.portable -y` e premo invio.  Possiamo vedere che l'istallazione è partita ed una volta completata posso confermare che l'installazione sia andata a buon fine lanciando il comando  `gitversion /version`

Un modo alternativo che non richiede l'utilizzo di un package manager, nel caso non siate abilitati ad usarlo è quello di scaricare il pacchetto nuget GitVersion.CommandLine e dentro trovate l'eseguibile. Questa procedura non è documentata da nessuna parte quindi prendetela con le pinzette

Ora che l'abbiamo installato sulla nostra macchina è il momento di cominciare a trarne dei vantaggi. 

Per vederlo in azione creiamo un progetto dotnet core con la CLI , niente di esoterico, una semplice console application. Facciamo `dotnet new console` e vediamo che viene creato il programma, poi con `dotnet run` lo eseguiamo e vediamo il classico "Hello world!". 

Ora, per usare gitversion abbiamo bisogno di … git, quindi inizializziamo un repositori con `git init`. Andiamo su github punto com slash github shash gitignore e scarichiamo un gitignore adatto al nostro ambiente. Scegliamo visualstudio e lo salviamo direttamente nella cartella del progetto togliendo tutto quello che sta prima di puntogitignore.

 Ora possiamo fare `git add .` e `git commit -m "first commit"`

Ora che abbiamo un repository con almeno un commit proviamo ad eseguire gitversion. Possiamo da subito vedere che il commando torna una serie di informazioni sulla versione. Prima di tutto come default propone la versione zero punto uno punto zero. 

Alcune delle informazioni interessanti che possiamo trovare nella risposta sono

BranchName che fornisce il nome del branch
Lo Sha del commit cosi come la versione corta in ShortSha
FullSemVer che ha questo curioso più zero alla fine che vedremo tra un po' a cosa serve
Ed InformationalVersion che fornisce una label completa con tutte le informazioni del commit. 

Ora apriamo vscode e facciamo una piccola modifica. Salviamo il file e facciamo il commit. 

Bene. Ora proviamo a fare di nuovo gitversion. Vediamo che la nostra semver non è cambiata, ci troviamo sempre ad avere zero punto uno punto zero. Ma il valore di BuildMetaData è cambiato, è passato da 0 a 1 ed, in particolare, questo valore viene utilizzato per comporre FullSemVer.

Di default per far incrementare la semver dobbiamo usare i tag nei commit. Infatti se creiamo un tag con `git tag` e poi proviamo a lanciare gitversion vediamo che la semver diventa zero punto uno punto uno come il tag che abbiamo.

Gitversion è molto configurabile e di default supporta alcune modalità di utilizzo. Vedremo nei prossimi video quali sono i modi di utilizzo di default e come configurarlo, e vedremo anche come utilizzarlo nella nostra pipeline di CI/CD

Per ora è tutto, alla prossima.

