# HappyDB :)

HappyDB is a corpus of 100,000+ crowd-sourced happy moments. The goal of the corpus is to advance the state of the art of understanding the causes of happiness that can be gleaned from text. Please see also our [web site](http://rebrand.ly/happydb)


## Data Collection

We conducted a large scale collection of happy moments over 3 months on Amazon Mechanical Turk (MTurk.) For every task, we asked the MTurk workers to describe 3 happy moments in the past 24 hours (or past 3 months.)

Here are the instructions of the data collection task.

```
What made you happy today?  Reflect on the past {24 hours|3 months}, and recall three actual events 
that happened to you that made you happy.  Write down your happy moment in a complete sentence.
Write three such moments.

Examples of happy moments we are NOT looking for (e.g events in distant past, partial sentence):
    - The day I married my spouse
    - My Dog
```


## HappyDB Statistics

Basic statistics of HappyDB are shown in the table.


| Collection period | 3/28/2017 - 6/16/2017 |
| ------------- | ------------- |
| # happy moments | 100,922 |
| # distinct users | 10,843 |
| # distinct words | 38,188 |
| Avg. # happy moments / user | 9.31 |
| Avg. # words / happy moment | 19.66 |


## How to Download

You can download the dataset by using `git` command or simply downloading the file from the repository.

```
$ git clone <git-repository-path>
```

## Directory Structure

After you clone or download the repository, you will see the following file structure.

```
happydb
└── data
    ├── cleaned_hm.csv
    ├── demographic.csv
    ├── original_hm.csv
    ├── senselabel.csv
    ├── topic_dict
    │   ├── entertainment-dict.csv
    │   ├── exercise-dict.csv
    │   ├── family-dict.csv
    │   ├── food-dict.csv
    │   ├── people-dict.csv
    │   ├── pets-dict.csv
    │   ├── school-dict.csv
    │   ├── shopping-dict.csv
    │   └── work-dict.csv
    └── vad.csv
```

HappyDB consists of a set of CSV files. Here are schema descriptions of the files.


### cleaned_hm.csv

`cleaned_hm.csv` contains cleaned-up happy moments and some additional information in addition to original happy moments.

- **hmid (int)**: Happy moment ID
- **wid (int)**: Worker ID
- **reflection_period (str)**: Reflection period used in the instructions provided to the worker (3m or 24h)
- **original_hm (str)**: Original happy moment
- **cleaned_hm (str)**: Cleaned happy moment
- **modified (bool)**: If True, `original_hm` is "cleaned up" to generate `cleaned_hm` (True or False)
- **predicted_category (str)**: Happiness category label predicted by our classifier (7 categories. Please see the reference for details)
- **ground_truth_category (str)**: Ground truth category label. The value is `NaN` if the ground truth label is missing for the happy moment
- **num_sentence (int)**: Number of sentences in the happy moment


### original_hm.csv

`original_hm.csv` contains *unfiltered* version of happy moments. 

- **hmid (int)**: Happy moment ID
- **wid (int)**: Worker ID
- **hm (str)**: Original happy moment
- **reflection_period (str)**: Reflection period used in the instructions provided to the worker (3m or 24h)


### demographic.csv

`demographic.csv` contains demographic information of the workers who contributed to the happy moment collection.

- **wid (int)**: Worker ID
- **age (float)**: Age
- **country (str)**: Country of residence (follows the ISO 3166 Country Code)
- **gender (str)**: {Male (m), Female (f), Other (o)}
- **marital (str)**: Marital status {single, married, divorced, separated, or widowed}
- **parenthood (str)**: Parenthood status {yes (y) or no (n)}


### senselabel.csv

`senselabel.csv` contains multi-word expression and supsersense tags on "cleaned" happy moments. Thus, the number of rows are exactly same as that of `cleaned_hm.csv`.

- **hmid (int)**: Happy moment ID
- **tokenOffset (int)**: Position index of a token
- **word (str)**: Token in the original form
- **lowercaseLemma (str)**: Lemmatized token in lowercase
- **POS (str)**: Part-of-Speech tag
- **MWE (str)**: Multi-word expression (MWE) tag in the extended IOB style (See [REF] for further information)
- **offsetParent (int)**: The beginning position of a multi-word expression
- **supersenseLabel (str)**: Supersense classes defined in the WordNet (19 verb and 25 noun classes. See [REF] for further information.)

Here is an example of annotated happy moments. A sentence is tokenized into words. You may see that "got" in the 2nd position is lemmatized as "get" in the `lowercaseLemma` column. The `supersenseLabel` column shows the supersense labels of each word or multi-word expression.

```
    hmid  tokenOffset      word lowercaseLemma    POS MWE  offsetParent supersenseLabel
0  70027            1         I              i   PRON   O             0             NaN
1  70027            2       got            get   VERB   B             0        v.motion
2  70027            3        my             my   PRON   o             0             NaN
3  70027            4       car            car   NOUN   o             0      n.artifact
4  70027            5     waxed          waxed   NOUN   I             2             NaN
5  70027            6       and            and   CONJ   O             0             NaN
6  70027            7  polished         polish   VERB   O             0        v.motion
7  70027            8         .              .  PUNCT   O             0             NaN
```

### topic_dict/*-dict.csv

`happydb/data/topic_dict` contains the topic (e.g., entertainment, exercise etc.) dictionaries that are manually prepared for analyzing topics of happy moments. Each file contains keywords that belong to the topic.


## Getting started with the dataset

All files (except for *-dict.scv files) follow the CSV-format. Each programming language should have some library that has CSV loading function. In Python, `pandas` is a common library for handling data.

Here are some examples looking at data with `pandas` library.

```python
>>> import pandas as pd
>>> df = pd.read_csv("happydb/data/cleaned_hm.csv")
>>> df["cleaned_hm"].head()

0    I went on a successful date with someone I fel...
1    I was happy when my son got 90% marks in his e...
2         I went to the gym this morning and did yoga.
3    We had a serious talk with some friends of our...
4    I went with grandchildren to butterfly display...
Name: cleaned_hm, dtype: object

>>> df["predicted_cattegor"”].value_counts
affection           34206
achievement         34044
enjoy_the_moment    11202
bonding             10729
leisure              7505
nature               1846
exercise             1206
Name: predicted_category, dtype: int64
```

We plan to release sample scripts and iPython (Jupyter) notebooks for the HappyDB soon!


# References

Please cite the following publication if you use the dataset in your work.

```
Akari Asai, Sara Evensen, Behzad Golshan, Alon Halevy, Vivian Li, Andrei Lopatenko, 
Daniela Stepanov, Yoshihiko Suhara, Wang-Chiew Tan, Yinzhan Xu, 
``HappyDB: A Corpus of 100,000 Crowdsourced Happy Moments'', LREC '18, May 2018. (to appear)
```


# Contact

Please ask us questions at <happydb@recruit.ai>.
