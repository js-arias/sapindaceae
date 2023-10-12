# Phylogenetic biogeography of Sapindaceae

This repository contains the data and results
of a phylogenetic biogeography analysis
of the plant family [Sapindaceae](https://en.wikipedia.org/wiki/Sapindaceae)
using the computer program [PhyGeo](https://github.com/js-arias/phygeo).

## Source data

### Geographic data model

The geographic data model is an equal area pixelation of the Earth,
with 360 pixels in the equatorial ring.

- `pixels-360.tab`:
  This file contains the pixel IDs
  and their associated geographic locations.

### Paleogeographic model

The plate motion model is Muller et al. (2022).
The paleolandscape model is based on an unrotated version
of Cao et al. (2017) for the 0-400 Ma period,
and an unrotated version of the PaleoMap model
(Scotese and Wright 2018),
for the period 405-540 Ma.
Then the pixels were rotated
using the Müller et al. (2022) plate motion model.

- `muller-motion-360-5.tab`:
  This file contains the pixelated version of the plate motion model,
  with e360 pixelation,
  and time slices for each 5 million years,
  from 600 Ma to present.
- `muller-landscape-cao-paleomap-360-5.tab`:
  This file contains the pixelated version of the paleolandscape model,
  with e360 pixelation,
  and time slices for each 5 million years,
  from 540 Ma to present.

### Phylogeny

The phylogenetic tree was built using the Sapindaceae branch
from the phylogenomic analysis of the Sapindales by Joyce et al. (2023),
which is quite similar in content
(at genus level)
to previous biogeographic analyses of the group
(Buerki et al. 2011, 2013).
As the original publication does not provide a machine-readable file,
the relationships and ages were extracted manually
from the figures.
The phylogeny was augmented
with a few terminals from Buerki et al. (2013),
mostly to enlarge the sampling of a few genera.
The species *Matayba tenax* was excluded,
as it does not match any *Maytaba* species or synonym
in the [Plants of the World](https://powo.science.kew.org/taxon/urn:lsid:ipni.org:names:30000506-2) database,
as this particular terminal
float in a previous analysis
(Buerki et al., 2021),
and the genus *Matayba* did not appear as monophyletic
in previous studies (Buerki et al., 2011, 2013).

The tree was then updated
with the taxonomy from [Plants of the World](https://powo.science.kew.org/taxon/urn:lsid:ipni.org:names:30000506-2)
in the file `term-taxonomy.tab`,
removing synonyms from the tree.

- `data-tree.tab`:
  This file contains the phylogeny as a tab-delimited table.
- `tree-joyce2023.svg`:
  This file contains a drawing of the phylogenetic tree.

### Distribution records

Specimen data were obtained
from a search of geo-referenced preserved specimens of Sapindaceae in
[GBIF](https://doi.org/10.15468/dl.tjpzv2).
The initial number of records was 387.463 occurrences.

To process the raw occurrence records in GBIF,
first a taxonomy using the terminal names was built
using the [GBIFer](https://github.com/js-arias/gbifer) tool:

```bash
gbifer tax add --rank species --file term-taxonomy.tab < terminals.txt
```

Then the taxonomy is filled with all potential taxon names
from the occurrence file from GBIF
that are synonyms or sub-species of the names
already in the taxonomy file:

```bash
gbifer tax match --file term-taxonomy.tab < occurrence.txt
```

The taxonomy file was edited to correct spelling errors
and match the [GBIF](https://www.gbif.org/species/6657) taxonomy
with the taxonomy from the [Plants of the World](https://powo.science.kew.org/taxon/urn:lsid:ipni.org:names:30000506-2).
This updated taxonomy,
in the file `term-taxonomy.tab`,
is used to update the phylogenetic tree,
removing synonyms from the tree.

The taxonomy file was used to extract country information
from the specimen records:

```bash
gbifer country --tax term.taxonomy.tab < occurrence.txt > countries.tab
```

The resulting file `countries.tab` was edited
by removing the countries not explicitly defined in [Plants of the World](https://powo.science.kew.org/taxon/urn:lsid:ipni.org:names:30000506-2)
as native.

Then the occurrence table from GBIF was filtered using both the taxonomy file and the country file.

```bash
gbifer filter -tax term-taxonomy.tab -country countries.tab < occurrence.txt > occu-in-tree-geo.txt
```

Then the filtered points are converted into a file of points
to be used with the [taxRange](https://github.com/js-arias/ranges) tool:

```bash
gbifer export -tax term-taxonomy.tab < occu-in-tree-geo.txt > raw-gbif-records.tab
```

The filtered file,
stored as `raw-gbif-records.tab`,
contains 68.307 occurrences.

As there are no geo-referenced specimen records for *Euchorium cubense*,
a record file based on [a material citation for the taxon](https://www.gbif.org/occurrence/4135894102)
is stored in the file `raw-euchorium-records.tab`.

Finally,
using the [taxRange](https://github.com/js-arias/ranges) tool,
the filtered GBIF records are transformed into a file
with presence pixels.

```bash
taxrange imp.points -e 360 -f text -o raw-points.tab raw-gbif-records.tab
taxrange imp.points -e 360 -f text -o raw-euchorium-points.tab raw-euchorium-records.tab
```

The resulting file is stored in `raw-points.tab`.
The directory `terminals` stores the maps of the used distribution ranges.

## Analysis

### Landscape

Key | Prior | Environment
--- | ----- | -----------
  1 | 0.001 | oceanic plateaus
  2 | 0.005 | continental shelf
  3 | 1.000 | lowlands
  4 | 1.000 | highlands
  5 | 0.001 | ice sheets

- `landscape-key.tab`:
  This file contains the keys for the landscape features
  of the paleolandscape model.
- `model-pix-prior.tab`:
  This file contains the definition of the pixel priors
  used in the analysis.

### Project

To set up a project,
all input data is added to the project.
Here the project is stored in the `project.tab` file:

```bash
phygeo geo add -type geomotion project.tab muller-motion-360-5.tab 
phygeo geo add -type landscape project.tab muller-landscape-cao-paleomap-360-5.tab
phygeo geo prior -add model-pix-prior.tab project.tab
phygeo tree add -f data-tree.tab project.tab data-tree.tab
phygeo range add -f data-points.tab -type points project.tab raw-points.tab 
phygeo range add -type points project.tab raw-euchorium-points.tab 
```

## Results

### Estimation

Maximum likelihood
was estimated using the command `diff ml` of `PhyGeo`.
The output log is stored in log-ml.txt file:

```bash
phygeo diff ml project.tab > log-ml.txt
```

The maximum likelihood estimation of $\lambda$ was 35.9.

Then,
an estimation of the posterior distribution
using a uniform prior between 0 and 200
was made with the command `diff integrate`,
and storing the output in the log file `log-like.txt`:

```bash
phygeo diff integrate -parts 100 -max 200 project > log-like.txt
```

Using these results,
the posterior distribution of the pixels
is calculated by approximating
the posterior distribution of $\lambda$
with a [gamma distribution](https://en.wikipedia.org/wiki/Gamma_distribution),
with $\alpha$=108, $\beta$=3 (rate),
and making 1000 samples from this distribution,
and calculating 100 stochastic mappings
from each sample:

```bash
phygeo diff integrate -distribution gamma="108,3" -p 100 --parts 1000 project
```

### Outputs

The output is quite large to be posted here
(about 4.3 GB,
or 962 MB when zip compressed),
and a link for the data will be provided
in the future
if data hosting is available.

Nevertheless,
the main result maps are given in the directory `b-r95`
and `b-u95`.
The maps are built using the command `diff map`,
with a KDE of $\lambda$ 1000.
The directory `b-r95`
contains the posterior of the pixels
under the paleogeographic model.
The directory `b-u95`
contains the same posteriors,
but rotated into current geographic locations,
which might be helpful
to understand the results.
Each map has the convention
`<type>-<tree>-n<node id>-<age>.png`,
in which `<type>`
indicates the type of the reconstruction
(r95 for maps with paleogeography,
u95 for maps with current geographic locations),
`<tree>` indicates the tree
(in this case joyce2023),
`<node id>` is the identifier of the node
(that can be consulted in the file `tree-joyce2023.svg`),
and `<age>` is the age in million years.

```bash
phygeo diff map -c 1440 -key landscape-key.tab -gray -kde 1000 -i project.tab-joyce2023-sampling-1000x100.tab -o "b-r95/r95" project.tab
phygeo diff map -c 1440 -key landscape-key.tab -gray -kde 1000 -unrot -contour <contour-map> -i project.tab-joyce2023-sampling-1000x100.tab -o "b-u95/u95" project.tab
```

The speed is calculated with the command `diff speed`,
a tree with the speed of branches
is stored as `speed-joyce2023.svg`,
and a log file with the distances,
the confidence interval,
and the average velocity is stored in `speed-branch.txt`.

```bash
phygeo diff speed -tree speed -step 5 -box 10 -i project.tab-joyce2023-sampling-1000x100.tab project.tab > speed-branch.txt
```

## References

References are also available as BiBTeX in the file `biblio.bib`.

Buerki, S. et al.
(2011)
An evaluation of new parsimony-based versus parametric inference methods in biogeography: a case study using the globally distributed plant family Sapindaceae.
Journal of Biogeography, 38, 531-550.
DOI: [10.1111/j.1365-2699.2010.02432.x](https://doi.org/10.1111/j.1365-2699.2010.02432.x).

Buerki, S. et al.
(2013)
The abrupt climate change at the Eocene–Oligocene boundary and the emergence of South-East Asia triggered the spread of sapindaceous lineages.
Annals of Botany, 112, 151-160.
DOI: [10.1093/aob/mct106](https://doi.org/10.1093/aob/mct106).

Buerki, S. et al.
(2021)
An updated infra-familial classification of Sapindaceae based on targeted enrichment data.
American Journal of Botany, 108, 1234-1251.
DOI: [10.1002/ajb2.1693](https://doi.org/10.1002/ajb2.1693).

Cao, W. et al.
(2017)
Improving global paleogeography since the late Paleozoic using paleobiology.
Biogeosciences, 14, 5425-5439.
DOI: [10.5194/bg-14-5425-2017](https://doi.org/10.5194/bg-14-5425-2017).

GBIF.org
(2023)
GBIF occurrence download.
DOI: [10.15468/dl.tjpzv2](https://doi.org/10.15468/dl.tjpzv2).

Joyce, E. M. et al.
(2023)
Phylogenomic analyses of Sapindales support new family relationships, rapid Mid-Cretaceous Hothouse diversification, and heterogeneous histories of gene duplication.
Frontiers in Plant Science 14: 1063174.
DOI: [10.3389/fpls.2023.1063174](https://doi.org/10.3389/fpls.2023.1063174)

Müller, R. D. et al.
(2022)
A tectonic-rules-based mantle reference frame since 1 billion years ago – implications for supercontinent cycles and plate–mantle system evolution.
Solid Earth, 12, 1127-1159.
DOI: [10.5194/se-13-1127-2022](https://doi.org/10.5194/se-13-1127-2022).

PoWO
(2023)
Plants of the World Online.
URL: <http://www.plantsoftheworldonline.org/>.

Scotese, C.S., Wrigth, N.
(2018)
PALEOMAP Paleodigital elevation models (PaleoDEMs) for Phanerozoic.
URL: <https://www.earthbyte.org/paleodem-resource-scotese-and-wright-2018/>.
