# Data exploration

This directory contains the data exploration
for a phylogenetic biogeography analysis
of the plant family [Sapindaceae](https://en.wikipedia.org/wiki/Sapindaceae)
using the computer program [PhyGeo](https://github.com/js-arias/phygeo).

## Source data

### Geographic data model

The geographic data model is an equal area pixelation of the Earth,
with 120 pixels in the equatorial ring.

- `pixels-ids-120.tab`:
  This file contains the pixel IDs
  and their associated geographic locations.

### Paleogeographic model

Three paleogeographic models
are used for the data exploration.

The full plate motion model is Muller et al. (2022).
The paleolandscape model is based on an unrotated version
of Cao et al. (2017) for the 0-400 Ma period,
and an unrotated version of the PaleoMap model
(Scotese and Wright 2018),
for the period 405-540 Ma.
Then the pixels were rotated
using the Müller et al. (2022) plate motion model.

- `muller-motion-120-5.tab`:
  This file contains the pixelated version of the plate motion model,
  with e120 pixelation,
  and time slices for each 5 million years,
  from 600 Ma to present.
- `muller-landscape-cao-paleomap-120-5.tab`:
  This file contains the pixelated version of the paleolandscape model,
  with e120 pixelation,
  and time slices for each 5 million years,
  from 540 Ma to present.

A time-staged model
using the current landscape
was based on the landscape from the Cao model
at the present time stage,
but without any movement between time stages
and no changes in the geography.

- `no-motion-motion-120-5.tab`:
  This file contains a single plate
  that is immobile in all time stages.
  It uses e120 pixelation
  and time slices for each 5 million years,
  from 600 Ma to the present.
- `cao-landscaoe.nomotion-120-5.tab`:
  This file contains the pixelated version of the paleolandscape model,
  with e120 pixelation,
  and time slices for each 5 million years,
  from 540 Ma to present,
  but using the present landscape
  for all time stages.

A model without any movement and any landscape
(i.e., a homogeneous sphere)
is also used.

- `no-motion-motion-120.tab`:
  This file contains a single plate
  and a single time stage.
  It uses e120 pixelation.
- `no-motion-landscape-120.tab`:
  This file contains two landscape features
  defined for all pixels
  (used only for drawing)
  without any time stage.
  It uses e120 pixelation.

### Phylogeny

The same phylogeny as in the main study was used.

- `data-tree.tab`:
  This file contains the phylogeny as a tab-delimited table.

The tree was edited to remove the four faster branches
(*Lecanodiscus*,
*Podonephelium*,
*Tina*, and
*Toechima*).

- `tree-trim.tab`:
  This file contains the phylogeny
  with the four faster branches removed.

### Distribution records

The same distribution records used
for the main study were used,
but pixelated into an e120 pixelation.

- `data-points.tab`:
  The file with the pixelated records,
  using an e120 pixelation.

## Analysis

### Landscape

The landscape model for the full model
and the model with the current landscape
use the same priors as the main study.

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
  used in the analyses with landscape.

For the homogeneous sphere,
only two pixel types are defined;
both were set with a prior of 1.0.

Key | Prior | Environment
--- | ----- | -----------
  1 | 1.000 | ocean
  3 | 1.000 | land

- `noland-pix-prior.tab`:
  This file contains the definition of the pixel priors
  used in the analysis without landscape.

### Project

Projects are created in the same way as in the main study.

- `project-120.tab`:
  Project for the full model.

- `project-landscape-nomotion.tab`:
  Project for the current landscape.

- `project-120-noland.tab`:
  Project for the homogeneous sphere.

- `project-trim.tab`:
  The same as `project-120.tab`,
  but using the tree
  without the four faster branches.

## Results

### Estimation

The procedure for estimation
is the same as used for the main study.
Here only the likelihood estimates are reported,
as the resulting files are too large.

Project            | MLE Lambda | LogLike
------------------ | ---------- | --------
Full model         |       20.0 | -1135.61
Current landscape  |       17.1 | -1162.10
Homogeneous sphere |    \<0.005 | -1247.75
Trimmed tree       |       39.0 | -1015.63

### Output

The output is too large to be posted here,
but as the general results from the full model
are cursorily compared
with the model with only the current landscape,
the result from all nodes is posted here
in the directories `maps-full`
(for the full model)
and `maps-curr`
(for the current landscape).
