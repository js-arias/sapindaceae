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
