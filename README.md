# Tabulate phenotypes of SARS-CoV-2 spike that might have predictive power in understanding evolution for different mutations and clades

## Running the notebook that makes the predictions
First build the `conda` environment in [environment.yml](environment.yml), then activate it with:

    conda activate SARS2-spike-predictor-phenos

Then run the Jupyter notebook [SARS2-spike-predictor-phenos.ipynb](SARS2-spike-predictor-phenos.ipynb) using [papermill](https://papermill.readthedocs.io/) with:

    papermill -p config_yaml config.yaml SARS2-spike-predictor-phenos.ipynb results/SARS2-spike-predictor-phenos.ipynb
