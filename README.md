# Phenotypes of SARS-CoV-2 spike mutants with possible predictive power for forecasting evolution

## Overview

This GitHub repository is designed to aggregate data about various phenotypes of the SARS-CoV-2 spike that may be of value for forecasting the virus's evolution.

The notebook draws on different data sources for the effects of mutations on SARS-CoV-2 phenotypes.
Currently, these data sources are:
  - [full spike pseudovirus deep mutational scanning](https://doi.org/10.1016/j.cell.2023.02.001)
  - [RBD yeast-display deep mutational scanning](https://doi.org/10.1016/j.cell.2020.08.012)
  - [EVEscape predictions](https://www.nature.com/articles/s41586-023-06617-0)

The idea is that if you are interested in making forecasts of viral evolution, this repository provides a way to obtain up-to-date data on how mutations have been measured or predicted to affect spike phenotypes.

## Running the notebook in the repo to get phenotypic predictions

The repository consists of a Jupyter notebook ([SARS2-spike-predictor-phenos.ipynb](SARS2-spike-predictor-phenos.ipynb)) that can be run with an appropriate YAML configuration file (e.g., [config.yaml](config.yaml)) to tabulate both the effects of mutations on spike phenotypes and the predicted phenotypes of different SARS-CoV-2 clades.

This notebook reads in data contained within the notebook about how mutations affect spike phenotypes: look at the `mutation_phenotype_csvs` key in [config.yaml](config.yaml) to understand these data sources.
The data itself on the spike phenotypes are in [./data/](data), and the README in that subdirectory provides more explanation.

The notebook reads those data and then generates four output files, which by default are as follows:

 - [results/mutation_phenotypes.csv](results/mutation_phenotypes.csv): how individual amino-acid mutations (Wuhan-Hu-1 numbering) affect the spike phenotypes.
 - [results/mutation_phenotypes_randomized.csv](results/mutation_phenotypes_randomized.csv): a version of the mutation-phenotypes randomized with the phenotypes randomized among mutations for different random number seeds.
 - [results/clade_phenotypes.csv](results/clade_phenotypes.csv): the predicted phenotypes of different SARS-CoV-2 clades, along with the clade parents, their mutations, whether they are descendants of key ancestor clades, and their estimated growth rates (where available).
 - [results/clade_phenotypes_randomized.csv](results/clade_phenotypes_randomized.csv): the clade phenotypes generated from the randomized mutation phenotypes.

If you want, you can just use the values in those CSVs.
However, although the input data in this repository with the spike predictor phenotypes is only updated sometimes (when new data become available), there are constantly new clades being designated and their estimated growth rates are being updated daily, so the clade phenotype estimates are constantly changing.

Therefore, if you want to get the latest predictions, your best bet is to clone this repository (perhaps as a submodule), and then run the notebook yourself.

After you have obtained the repo, first build the `conda` environment in [environment.yml](environment.yml), then activate it with:

    conda activate SARS2-spike-predictor-phenos

Then run the Jupyter notebook [SARS2-spike-predictor-phenos.ipynb](SARS2-spike-predictor-phenos.ipynb) using [papermill](https://papermill.readthedocs.io/) with:

    papermill -p config_yaml config.yaml SARS2-spike-predictor-phenos.ipynb results/SARS2-spike-predictor-phenos.ipynb

Note that you can pass a custom configuration file to the notebook using the `-p config_yaml <configuration YAML>`, so you can potentially make a different configuration than the default one in [config.yaml](config.yaml).
In particular, if you want reproducible output then you should specify specific versions of the `pango_json` and `pango_growth_json` keys in the YAML rather than just the latest versions as in the default [config.yaml](config.yaml).

## Interactive plot phenotypes
In addition to the CSV files in [./results/](results), running the notebook creates an interactive plot that allows you to look at scatter plots of the phenotypes for clades.
That plot is placed in [docs/index.html](docs/index.html), and is rendered on GitHub Pages at [https://jbloomlab.github.io/SARS2-spike-predictor-phenos/](https://jbloomlab.github.io/SARS2-spike-predictor-phenos/).

## Importance of the randomized phenotypes
When making predictions, there is always a danger of over-fitting or [failing to account for phylogenetic correlations](https://www.jstor.org/stable/2461605) in a way that makes phenotypes seem more predictive of evolution than they really are.
Therefore, the pipeline creates files (as described above) that randomize effects among mutations and generates clade phenotype predictions from these randomized data.
You should always compare the accuracy of predictions with the actual non-randomized data to those made with these randomized data: if the actual data are not any more predictive than the randomized data, then somehow you are overfitting or neglecting to account for phylogenetic correlations.

## Versioning
Each new run of this pipeline on GitHub has a tag indicating the date it was run as `YYYY-MM-DD`.
In addition, the [CHANGELOG](CHANGELOG.md) describes updates such as adding new data.

## Acknowledgments
This repository is maintained by Jesse Bloom.

Thanks to:

 - Cornelius Roemer for maintaining the [Pango clade sequence definitions](https://github.com/corneliusroemer/pango-sequences) used by this repo.
 - The Bedford lab for mantaining the [clade growth estimates](https://github.com/nextstrain/forecasts-ncov/) used by this repo.
 - The generators of the data that goes into the various spike phenotype predictors incorporated into this repo:
   - Bernadeta Dadonaite and the Bloom lab for the full-spike deep mutational scanning:
     - [Dadonaite et al (2023)](https://doi.org/10.1101/2023.11.13.566961)
   - The Starr lab for the RBD yeast-display deep mutational scanning of ACE2 affinity and RBD expression:
     - [Taylor and Starr (2023)](https://journals.plos.org/plospathogens/article?id=10.1371/journal.ppat.1011901)
   - The Cao lab for the RBD yeast-display antibody-escape deep mutational scanning, which is incorporated in the [Bloom lab antibody escape calculator](https://jbloomlab.github.io/SARS2-RBD-escape-calc/):
     - [Cao et al (2023)](https://www.nature.com/articles/s41586-022-05644-7)
     - [Yisimayi et al (2023)](https://www.nature.com/articles/s41586-023-06753-7)
     - [Greaney et al (2022)](https://academic.oup.com/ve/article/8/1/veac021/6549895)
  - The Marks lab for the EVEscape values:
     - [Thadani et al (2023)](https://www.nature.com/articles/s41586-023-06617-0) 
