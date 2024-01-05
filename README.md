# MATH70103: Unstructured Data Analysis
## Final Project - CID: 00926223

## README 

<b>The order of the Jupyter notebook is in the right order and should be simply run from top to bottom</b>

<b>Make sure that the youtube.rar file is first extracted into the same directory where the notebook is stored. Without extraction the code will fail to run</b>

The only exception to this are the first two code cells which need to be run once, followed by a Kernel restart. Once the Kernel restart has been done, the code can be run again all the way from the top. Even if the first two cells are run again, they won't affect the notebook as the required settings and packages would already have been installed on the first run. 

## Execution Time Extention Enablement on Jupyter Notebook
<b>Lets enable the jupyter nbextension that will show the run time for each cell.</b>
This is also mentioned in the Jupyter Notebook. It will require the Kernel to be restarted which only needs to be done once.  

```bash
pip install pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
jupyter nbextension enable execute_time/ExecuteTime
```

## Required Packages

The below bullet list also shows the versions under which each module was invoked and run. 
<b>PLEASE NOTE: </b>Even though there might be some backward compatiblity for the modules used in this report, it is better to either have the same version of the python module or an upgraded version to avoid any compatibility issues. This is unlikely to happen as modules used are quite generic generally speaking. 

<ol>
<li>numpy == 1.21.5</li>
<li>pandas == 1.4.2</li>
<li>matplotlib == 3.5.1</li>
<li>seaborn == 0.11.2</li>
<li>scikit-learn == 1.0.2</li>
<li>nltk == 3.7</li>
<li>wordcloud == 1.9.3</li>
<li>gensim == 4.1.2</li>
<li>multiprocessing - Built-in package</li>
</ol>

Jupyter Notebook Version: 6.4.8

Please note that module "subprocess" should already be present as its an inbuilt module. 
This module will be used to run the next chunk of code that automatically checks and installs and missing modules. See below:

```python
import subprocess

def check_and_install(package):
    try:
        __import__(package)
        print(f'{package} is already installed.')
    except ImportError:
        print(f'{package} is not installed. Installing...')
        subprocess.check_call(["pip", "install", package])

# List of required libraries
required_libraries = ['numpy', 'pandas', 'matplotlib', 'seaborn', 'sklearn', 'nltk', 'wordcloud', 'gensim', 'multiprocessing']

# Check and install libraries if needed
print("Checking if all the required packages are installed. Any missing packages will be automatically installed\n")
for lib in required_libraries:
    check_and_install(lib)
```

A Kernel restart is recommended if the output contains any module that is not installed. 

## Custom Functions specified within the code (non-module/builtin functions) 

### Pre-processing function - preprocess_doc(df)

The preprocess_doc function takes a pd.DataFrame as input and the DF should contain "title" and "tags" as column headings. The code will not run if the DF does not have these two column values. 

This function does the following:
<ul>
<li>Removes special characters and punctuation</li>
<li>Changes to lower case</li>
<li>Removes stop words</li>
</ul>

### Classify sentiment for each "title" - classify_sentiment(likes, dislikes, threshold=0.2)

Range of values for threshold - [0,1] -> 0.2 has been used in the actual code

The classify_sentiment function takes the number of likes and dislikes (both int64 dtype) as input along with the variable threshold and returns the sentiment classification as 3 possible classes i.e. 'positive', 'negative' and 'neutral'. 

The input variable threshold specifies the degree by which the likes exceed dislikes in order for the video to be deemed as 'positive'

Anything in the range 0-threshold is deemed as 'neutral'

If the dislikes > likes, the sentiment is 'negative'

### Function to plot a heatmap from classification report for each method - plot_confusion_matrix_log_scale(y_true, y_pred, title):

The above function takes the actual value, predicted value and the feature as input and generates a confusion matrix, followed by a seaborn heatmap plot. 

## Parallelisation 

The parallelisation process has been defined only for the Topic Modelling section for LDA. Gensim module contains the function LdaMulticore which lets us define the number of workers or nodes among which the work would be split. This can be retrieved using in-built module function multiprocessing.cpu_count() which gives us 4 workers. 

This simply shows the number of cores available. As the desktop this was run under has 4 cores, all 4 cores were used. 

The memory is then efficiently allocated per gensim algorithm to the active cores or workers. - System memory was 16GB 

### System Specifications that the code was run on

<ul>
<li>Processor	Intel(R) Core(TM) i5-4570 CPU @ 3.20GHz   3.20 GHz</li>
<li>Installed RAM	16.0 GB</li>
<li>System type	64-bit operating system, x64-based processor</li>
</ul>

## Runtime 

<ul>
<li>Data Load and Preprocess - 24.43s</li>
<li>EDA (The full section) - 24.655s (Majority of the runtime was due to Word Cloud section)</li>
<li>Sentiment Analysis SVM - 4m 37s</li>
<li>Sentiment Analysis Naive Bayes - 2.43s</li>
<li>LDA Topic Modelling - 54.3s</li>
</ul>
