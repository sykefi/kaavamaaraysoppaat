# Kaavamaaraysoppaat

Kaavamääräysoppaiden erillissivusto. Sivustoa hallinnoi [Suomen ympäristökeskus](https://www.syke.fi/) yhdessä [ympäristöministeriö](https://ym.fi/)n kanssa.

Kehittäjädokumentaatio on vielä kesken.


## Linkitetyt repot

Sivusto on rakennettu siten, että sen sisältö koostuu pääosin toisista git-repoista  noudettavista lähdekooditiedostoista. Linkitys kaavamaaraykset-reposta toisiin git-repoihin on toteutettu [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)-mekanismilla. Näin voidaan esittää samalla sivustolla useamman kuin yhden kaavamääräysoppaan version kuvaus linkittämällä ko. kaavamääräysrepon eri release-tagien mukaisiin committeihin.

Sivulle voidaan linkittää mitä tahansa julkisia git-repoja, ja GitHub Pages -osaa noutaa ko. linkkien asetetun commitin mukaiset sisällöt automaattisesti sivuston buildauksen yhteydessä. Käytännössä ainakin sisältömodulien julkaisuversioden tulee olla [sykefi -GitHub-organisaation](https://github.com/sykefi/) alla, jotta niiden pysyvyys voidaan taata. **Huom**: Mikäli moduli ei ole julkaistu GitHub:issa, sen sivumetatieto-laatikon muutostietojen esittäminen ei onnistu, sillä sen tiedot haetaan JavaScriptillä käyttäen [GitHub REST API](https://docs.github.com/en/rest)a.

| Nimi                 | versio | hakemistopolku          | linkitetty git-repo          | tagi / haara / commit  | huom |
-----------------------------|--------|-------------------------|------------------------------|--------------------|----------|
| Asemakaava           | dev    | [docs/asemakaava/dev](../docs/asemakaava/dev/) | [github.com/ilkkarinne/kmo-asemakaava](https://github.com/ilkkarinne/kmo-asemakaava) | develop | TODO: siirto ilkkarinne -> sykefi |
| Yleiskaava           | dev  | [docs/yleiskaava/dev](../docs/yleiskaava/dev/) | [github.com/ilkkarinne/kmo-yleiskaava](https://github.com/ilkkarinne/kmo-yleiskaava) | develop | TODO: siirto ilkkarinne -> sykefi |
| Yhteiset Sisältömakrot | | [docs/_includes/common](../docs/_includes/common) | [github.com/ilkkarinne/rytm-jekyll-includes](https://github.com/ilkkarinne/rytm-jekyll-includes) | main | TODO: siirto ilkkarinne -> sykefi

Kulloinkin linkatut git submodulet ja niiden tilan saa tulostettua (linux-tyyppisessä komentoriviympäristössä) seuraavalla loitsulla:
```sh
$ git submodule foreach --quiet 'printf "\n$sm_path: linked to " && git remote get-url origin && printf "at " && git describe --tags --first-parent --dirty --always'

docs/_includes/common: linked to https://github.com/ilkkarinne/rytm-jekyll-includes.git
at 873a038

docs/asemakaava/dev: linked to https://github.com/ilkkarinne/kmo-asemakaava.git
at 56ca83c

docs/yleiskaava/dev: linked to https://github.com/ilkkarinne/kmo-yleiskaava.git
at 7bed8a1

```

## Paikallinen kehitysympäristö

Sivuston kehittämisessä on huomattavasti hyötyä paikallisesta kehitysympäristöstä, jossa tietoihin tehtävät muutokset saa näkyviin esikatseluna ilman tietojen viemistä GitHub Pages - mekanismilla julkaistulle [Kaavamääräykset](https://sykefi.github.io/kaavamaaraysoppaat/)-sivustolle.

GitHub Pages -sivugenerointia voi simuloida varsin uskottavasti paikallisella työasemalla ajaen [Docker GitHub Pages](https://github.com/Starefossen/docker-github-pages) -Docker-konttia:

1. [Asenna Docker Engine](https://docs.docker.com/engine/install/), ja [komentorivi-git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), mikäli ei jo asennettu.
1. Tee juurihakemisto koneelle kloonatuille git-repoille ja siirry sinne
1. Tee [oma fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) git-reposta [sykefi/kaavamaaraysoppaat](https://github.com/sykefi/kaavamaaraysoppaat)
1. [Kloonaa](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) forkkisi paikalliselle työasemalle:
   ```sh
   $ git clone https://github.com/<oma-gh-tunnus>/kaavamaaraysoppaat.git
   ```
1. Siirry hakemistoon ```kaavamaaraysoppaat/docs```
1. Käynnistä docker-github-pages -kontti porttiin 4000:
   ```sh
   $ docker run -it --rm -v "$PWD":/usr/src/app -p "4000:4000" starefossen/github-pages
   ```
1. Sivusto tulee buildauksen jälkeen näkyviin osoitteeseen http://localhost:4000/ 
   Mikäli tulee virheilmoitus "docker: Cannot connect to the Docker daemon...", varmista, että asentamasi Docker Engine on käynnissä taustalla.
1. Muokkaa sivuston tietoja tarpeellisilta osin, varmista, että kaikki näyttää hyvältä osoitteesta http://localhost:4000/
1. On hyvä käytäntö olla tekemättä muutos-committeja suoraan linkitettyjen submodulien sisältöihin. Voit toki kokeilla muutoksia paikallisesti, commitoida ne sitten muutoksina suoraan linkitettyihin repoihin, pushata, ja tehdä pullin ry-tietomallit -repon paikallisen klooniin ko. modulin hakemistossa.
