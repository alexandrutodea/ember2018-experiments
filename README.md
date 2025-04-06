# EMBER2018 Experiments Jupyter Notebook
This repository includes a Jupyter notebook that implements and evaluates various machine learning algorithms on the full [EMBER2018](https://github.com/elastic/ember) dataset, as well as on a feature-reduced version.

One of the experiments involves reproducing the neural network from the [Machine-learning-for-detecting-malware-in-pe-files](https://github.com/CollinConnors/Machine-learning-for-detecting-malware-in-pe-files) repository. The rest of the experiments apply the [Scikit-learn RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) and the [LightGBM LGBMClassifier](https://github.com/microsoft/LightGBM) on both the complete EMBER2018 dataset and a reduced version created using the [Scikit-learn VarianceThreshold](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html).

# How to Run the Project

## Downloading and Getting the EMBER2018 Dataset Ready

### Downloading and Decompressing the Dataset

1. Access [the Download section in the EMBER2018 repository](https://github.com/elastic/ember/tree/master?tab=readme-ov-file#download)
2. Click the URL to download the compressed EMBER2018 dataset with the second feature version. It may take a while.
![CleanShot 2025-04-06 at 16 34 50@2x](https://github.com/user-attachments/assets/f97fa1a7-0db9-4354-97b1-4437e0a6fa28)
3. Decompress the file downloaded in step 2
4. Rename the decompressed file to "ember2018_data"

### Cloning and Modifying the EMBER2018 repository

1. Run the following command in a directory of your choice

```
git clone https://github.com/elastic/ember
```
 
2. Change the directory to the newly cloned project

```
cd ember
```

3. Open the newly cloned project in a file explorer:

```
open .
``` 

4. Open the features.py file inside the ember directory in the newly cloned project and modify line 192 to the following (see [this comment](https://github.com/elastic/ember/issues/103#issuecomment-1623975101) in [elastic/ember issue #103](https://github.com/elastic/ember/issues/103)):

```
entry_name_hashed = FeatureHasher(50, input_type="string").transform([[raw_obj['entry']]]).toarray()[0]
```

### Using Docker to Get the Dataset Ready

1. Ensure that [Docker](https://www.docker.com) version 4.37.2 or later is installed and running
2. Make sure you are inside the EMBER2018 project directory that you cloned from GitHub
3. Run the command:
   
```
docker build -t ember-image .
```

4. Run the following command, replacing **INSERT_PATH_TO_EMBER2018_DATA_DIRECTORY_ON_HOST** with the path of the folder that was obtained as a result of decompressing the compressed EMBER2018 dataset downloaded from the EMBER2018 repository. The name of the folder must be **ember2018_data**:

```
docker run -it -v INSERT_PATH_TO_EMBER2018_DATA_DIRECTORY_ON_HOST:/ember ember-image
```

5. Once the Docker container has started, run the following command inside it:

```
touch vectorize_features.py
```

6. Run the following command inside the Docker container:


```
echo -e 'import ember\nember.create_vectorized_features(".")\n_ = ember.create_metadata(".")' > vectorize_features.py
```

7. Run the following command inside the Docker container:

```
python vectorize_features.py
```

8. Once the execution has finished, run the following command:

```
exit
```

9. The following files:
- X_train.dat (7,62 GB)
- y_train.dat (3,2 MB)
- X_test.dat (1,9 GB)
- y_test.dat (800 KB)

should now be located in the ember2018_data folder on the host machine.

## Cloning the Project, Installing the Dependencies, and Configuring the Jupyter Notebook

### Cloning the Project

1. Install Python 3.12 on your system if it is not already installed

2. Run the following command in a directory of your choice

```
git clone https://github.com/alexandrutodea/ember2018-experiments
```

### Installing the Dependencies

1. Change the directory to the newly cloned project

```
cd ember2018-experiments
```

2. Create a virtual environment

```
python3 -m venv ember2018-experiments-venv
```

3. Activate the virtual environment

```
source ember2018-experiments-venv/bin/activate
```

4. Install the Dependencies

```
pip install -r requirements.txt
```

### Configuring the Jupyter Notebook

1. Start Jupyter Notebook using the following command

```
jupyter notebook .
```

2. Open the **ember2018-experiments.ipynb** file
3. Use Ctrl + F or Command + F to search for "UPDATE THIS PATH". Two occurrences should appear.
4. For the first occurrence, replace **/Users/alexandrutodea/Downloads/ember2018_data** with the path to the **ember2018_data** folder which was populated with the EMBER2018 .dat files using Docker in the previous section. Keep the double quotes.

```python
#UPDATE THIS PATH
X_train, y_train, X_test, y_test = ember.read_vectorized_features("/Users/alexandrutodea/Downloads/ember2018_data")
```
5. Navigate to the second occurrence of "UPDATE THIS PATH" using the arrows in the top right corner of the Jupyter Notebook

![CleanShot 2025-04-06 at 17 46 27@2x](https://github.com/user-attachments/assets/31369912-0d60-4eb0-a77e-0aca82342011)

6. Once you have arrived at the second occurrence, replace **./DLModel.weights.h5** with the location of the file where the weights after the best epoch of the neural network model will be saved. The file has to end in **.weights.h5**. Keep the double quotes.

```python
# UPDATE THIS PATH
path = "./DLModel.weights.h5"
```

7. Any of the cells in the Jupyter Notebook can now be run using **Shift + Enter**
