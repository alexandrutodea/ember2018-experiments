# EMBER2018 Experiments Jupyter Notebook
This repository includes a Jupyter notebook that implements and evaluates various machine learning algorithms on the full [EMBER2018](https://github.com/elastic/ember) dataset, as well as on a feature-reduced version.

One of the experiments involves reproducing the neural network from the [Machine-learning-for-detecting-malware-in-pe-files](https://github.com/CollinConnors/Machine-learning-for-detecting-malware-in-pe-files) repository. The rest of the experiments apply the [Scikit-learn RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) and the [LightGBM LGBMClassifier](https://github.com/microsoft/LightGBM) on both the complete EMBER2018 dataset and a reduced version created using the [Scikit-learn VarianceThreshold](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html).

# How to Run the Project

## Cloning the Project and Installing the Dependencies

1. Run the following command in a directory of your choice

```
git clone https://github.com/alexandrutodea/ember2018-experiments
```

2. 
