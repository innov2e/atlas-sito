# atlas-sito

Sito di INNOV2E Atlas, pubblicato con GitHub Pages su `https://atlas.innov2e.it/`.

Il repository contiene solo file statici: nessun codice dell'app, nessuna credenziale, nessun Export.

## Contenuto

| Percorso                        | Cosa è                                                                    |
| ------------------------------- | ------------------------------------------------------------------------- |
| `index.html`                    | Pagina del prodotto in italiano                                           |
| `en/index.html`                 | Pagina del prodotto in inglese                                            |
| `stile.css`                     | Stile condiviso, senza risorse esterne                                    |
| `404.html`                      | Pagina non trovata                                                        |
| `aggiornamenti/latest-it.json`  | Versione annunciata alle app in italiano                                  |
| `aggiornamenti/latest-en.json`  | Versione annunciata alle app in inglese                                   |
| `CNAME`                         | Dominio personalizzato di GitHub Pages                                    |
| `.nojekyll`                     | I file si pubblicano così come sono                                       |

## Aggiornamenti dell'app

L'app chiede `https://atlas.innov2e.it/aggiornamenti/latest-it.json` (o `latest-en.json`) solo quando l'utente lo chiede. Quello che c'è in questi due file è ciò che vedono tutte le app installate: un commit su `main` qui equivale ad annunciare una versione.

Finché i due file non ci sono, l'app risponde che il controllo non è riuscito: è atteso prima del primo rilascio.

### Dopo un rilascio

I due JSON li scrive `scripts/release.sh` del repository dell'app, in `target/release/bundle/aggiornamenti/`. Si copiano qui **senza modificarli**, dopo aver verificato che il pacchetto si scarichi dalla Release pubblica (vedi `docs/checklist-rilascio.md` del repository dell'app, sezione «Dopo il rilascio»).

```sh
cp ‹repository dell'app›/target/release/bundle/aggiornamenti/latest-*.json aggiornamenti/
git add aggiornamenti/latest-it.json aggiornamenti/latest-en.json
git commit -m "aggiornamenti: versione X.Y.Z"
git push
```

GitHub Pages pubblica in uno o due minuti. Poi:

```sh
curl -fsS https://atlas.innov2e.it/aggiornamenti/latest-it.json
curl -fsS https://atlas.innov2e.it/aggiornamenti/latest-en.json
```

### Ritirare una versione annunciata

Se la prova di aggiornamento fallisce, torna ai JSON di prima:

```sh
git revert HEAD
git push
```

Se era il primo rilascio, il revert toglie i due file e le app tornano a non vedere alcuna versione.

## Pulsante di scarico

In `index.html` e `en/index.html` il blocco «in arrivo» ha accanto, in un commento, il blocco con il pulsante di scarico. Al primo rilascio pubblico si scambia l'uno con l'altro nelle due pagine.

## Regole per i testi

- Il nome del prodotto è «INNOV2E Atlas». Il nome di Caspio non entra nel nome, nel logo né nel dominio, e non è mai più in evidenza del marchio INNOV2E.
- Il prodotto si descrive come strumento per «app Caspio Bridge», non «app Caspio».
- Nessun simbolo ® accanto ai nomi di Caspio.
- Nessun logo, grafica, schermata o testo copiato da Caspio.
- La nota sui marchi resta in fondo a ogni pagina, uguale a quella dell'app.
- Niente script di statistiche, niente font o risorse caricati da altri domini.
