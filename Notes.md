# Workflow and Github Actions notes

```yaml
  prepare-annunce:
    runs-on: ubuntu-18.04
    outputs:
      titles: ${{ toJson(fromJson(steps.read-announce-data.outputs.response).posts.*.title) }}
      urls: ${{ toJson(fromJson(steps.read-announce-data.outputs.response).posts.*.url) }}
      counter: ${{ steps.count-post.outputs.counter }}
```

Il job *prepare-annunce* viene eseguito su un agent *ubuntu-18.04* e genera gli output: *titles*, *urls*, *counter*.

**titles**: è un array JSON che contiene i titoli dei post in *annunce.data* pronti per essere annunciati

**urls**: è un array JSON che contiene le url alle pagine di pubblicazione dei post in *annunce.data* pronti per essere annunciati.

**counter**: è un vettore JSON che contiene un cursore autogenerato necessario al job successivo per la visita dei vettori con i dati di pubblicazione.

**fromJson**: è una funzione d'infrastruttura che converte stringa in oggetto JSON.

**toJson**: è una funziona d'infrastruttura che converte da oggetto in stringa JSON.

```yaml
  publish-matrix:
    needs: prepare-annunce
    runs-on: ubuntu-18.04
    strategy:
      matrix: 
        counter: ${{fromJson(needs.prepare-annunce.outputs.counter)}}
    steps:
    - run: |
          echo ${{fromJson(needs.prepare-annunce.outputs.titles)[matrix.counter]}}
          echo ${{fromJson(needs.prepare-annunce.outputs.urls)[matrix.counter]}}
```

**needs**: definisce il jobs da cui dipende l'esecuzione del job corrente e l'oggetto interfaccia da cui acquisire le variabili di uscita da utilizzare all'interno del jobs.

**strategy.matrix**: definisce un elenco di job paralleli (fino ad un massimo di 256) che sono il prodotto del numero di elementi presenti nei attributi assegnati all'oggetto *matrix* come, ad esempio, *counter*. Gli attributi di *matrix* sono oggetti vettoriali JSON.