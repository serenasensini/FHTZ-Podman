# Podman

Concepito come strumento di debugging per semplificare il lavoro con i cluster Kubernetes, è diventato un software dal motore autonomo e completo per la gestione di container.

## What

Podman (abbr. **Pod Manager**) è un container engine rilasciato nel 2018. È stato sviluppato da Red Hat e somiglia a Docker per molti aspetti, come l'utilizzo della riga di comando. Questo rende infatti possibile utilizzare i comandi tipici di Docker su Podman, utilizzando l’alias alias docker=podman. Nella maggior parte dei casi il passaggio da Docker a Podman è relativamente semplice; la novità però che caratterizza Podman è l'assenza di un demone centrale come istanza di controllo del singolo container e questo offre la possibilità di accedere alle diverse applicazioni virtualizzate anche senza permessi di root.

Podman gira su tutte le distribuzioni di Linux disponibili come Ubuntu, Fedora, CentOS, Debian e RHEL ma anche su Raspbian. Per la maggior parte dei casi, questo motore si può installare direttamente attraverso la gestione dei pacchetti del relativo sistema.

## What: pod
I  pod rappresentano una caratteristica fondamentale che contraddistingue Podman da Docker; i pod, che si rifanno al concetto dei pod di Kubernetes, rappresentano un insieme di più container all’interno di uno stesso namespace di Linux che condividono risorse ben definite per combinare in modo flessibile diverse applicazioni virtualizzate.

Podman ricorre ai namespace dell’utente del kernel di Linux, che assegnano permessi e ID ai processi. In realtà il container è eseguito in qualità di amministratore e questo conferisce all’ambiente virtualizzato di Podman uno standard di sicurezza elevato.

I cosiddetti **infra container** rappresentano il cuore pulsante di un pod e sono responsabili delle funzionalità relative all’unione dei vari pod, gestendo e garantendo a questo scopo le risorse singole come namespace, porte di rete, CPU, memoria, ecc. Inoltre, per la gestione dei pod, Podman ricorre allo strumento di monitoraggio  Conmon, che monitora le singole componenti virtualizzate e il che rende i log sicuri. Per di più, questo strumento funge da interfaccia per il terminale del container corrispondente. 

## HowTo: Configurazione e funzionamento di Podman

### Installare Podman su Debian, Ubuntu, Raspbian

```
sudo apt-get update -qq
sudo apt-get -qq -y install podman

```

### Installare Podman su Fedora, CentOS, Amazon Linux 2, RHEL 7:

```
sudo yum -y install podman
```

### Installare Podman con OpenSUSE:
```
sudo zipper install podman
```

### Installare Podman su Windows o macOS

L’unico requisito necessario è quello di avere una VM o un server Linux su cui installare Podman; in tal casosarà sufficiente utilizzare Podman Remote Client per stabilire una connessione SSH con il back end di Podman.

## Commands: main

### Command: pull
Per l’installazione dei container desiderati, sarà possibile attingere dal grande pool di immagini Docker già pronte all’uso, conosciuto anche come Docker Hub. Con l’aiuto del comando `pull`, sarà possibile scaricare da lì tutte le immagini desiderate dell’applicazione, per esempio l’ultima versione di Ubuntu:

```
$ podman pull hub.docker.com/_/ubuntu:latest
```

> Il percorso di salvataggio di default per le immagini è “.local/share/containers/”.

### Command: images
Riepilogo immagini presenti in locale

```
$ podman images
```

> Per un elenco delle immagini root bisogna anteporre “sudo”.

### Command: ps
Riepilogo dei container in esecuzione

```
$ podman ps
```

### Command: logs
Riepilogo dei log

```
$ podman logs
$ podman logs -t my-webapp
```

### Command: help
Aiuto con l'uso dei comandi o relativo all'uso dei parametri di un certo comando.

```
$ podman --help
$ podman pull --help
```

### Command: search
Ricerca di un'immagine per parola chiave

```
$ podman search parola_chiave
$ podman search parola_chiave --filter=is-official
```

### Command: run
Esecuzione dell'istanza di un'immagine

```
$ podman run -dt -p 8080:8080/tcp my-webapp
```

### Command: stop
Arresto di un container o di un pod

```
$ podman stop my-webapp
```

### Command: rm
Rimozione di un container o di un pod

```
$ podman rm my-webapp
```

## Commands: pods

### Command: create
Creazione pod

```
$ podman pod create
```

### Command: list
Riepilogo dei pod

```
$ podman pod list
$ podman ps -a --pod
```

### Command: run
Associare un container ad un pod

```
$ podman run -dt --pod pod_name container top # top per vedere l'output
$ podman run -dt --pod new:trtest -p 31000:80 nginx # crea pod e deploy container
```
