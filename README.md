# Robustness of Neural Rankers to Typos: A Comparative Study

This is the repo for the experiment of the effect of typos on the SOTA sparse retriever uniCOIL, including the original juypter notebook and the tree structure of the files for reproducibility. This research has been published in the paper: [*Robustness of Neural Rankers to Typos: A Comparative Study*](https://ielab.io/publications/adcs2022-typos/adcs2022-comparative-study.pdf)

# Environment
The original experiment was conducted on **Google Colab**, with default Python version 3.7, however the main library we used **Pyserini** requires for a higher version such as **Python 3.8**, we have tuned the platform to meet that requirement. The corresponding code for tuning the environment can be found on the **Python3.8 env** and the **Conda** (elective) part of the *typos_unicoil.ipynb* file.

# Directory structure
The directory structure of the experiment is as follows. 

**Notice**: some files (such as collections and indexes) are not provided in this repo for storage limitation, but the code for generating coressponding files are given in the jupyter notebook.
```
|-- typos_sparse_models
    |-- collections
    |-- indexes
    |-- runs
    |   |-- qrels.dl2019.txt
    |   |-- run.msmarco-passage.unicoil.tsv
    |   |-- run.queries-dev-small-typo1.unicoil.tsv
    |   |-- ...
    |   |-- run.queries-dev-small-typo10.unicoil.tsv
    |-- queries.dev.small.typo1.tsv
    |-- ...
    |-- queries.dev.small.typo10.tsv
```

# Libraries, Collections, Indexes and Models used 
The main python library used is **Pyserini** (version 0.15.0) https://github.com/castorini/pyserini, dependencies such as *huggingface-hub*, *torch* are installed as in the notebook **Python3.8 Env** part, we followed the instructions as suggested by the original author.

The collection used for this experiment is TREC 2019 DL (Passage). For uniCOIL we follow the instructions given in https://github.com/castorini/anserini/blob/master/docs/regressions-dl19-passage-unicoil.md to get the collection and indexing it, corresponding to the **Data** part of the notebook. 

The uniCOIL model is then initialised and tested to ensure it's correctly implemented, the correctness can be seen from the result of evaluation in the **uniCOIL** part of the notebook.

For our task, simply replace the typo queries in *--topics* used for searching in the following command, in the **Queries.dev.small.typo1.tsv** part of the notebook:
```
! python -m pyserini.search \
  --index msmarco-passage-unicoil-d2q \
  --topics queries.dev.small.typo1.tsv \
  --encoder castorini/unicoil-msmarco-passage \
  --output runs/run.queries-dev-small-typo1.unicoil.tsv \
  --output-format msmarco \
  --batch 36 --threads 12 \
  --hits 1000 \
  --impact
 ```
 
 A looped implementation for searching and evaluating can be found in the **A loop from query2 to query10** part of the notebook.
 
 **Notice**:
 For other models and collections, following similar processes and change codes in corresponding command lines.
 links for other models in Pyserini can be found in https://github.com/castorini/pyserini.



# Acknowledgements
Shengyao Zhuang is supported by a UQ Earmarked PhD Scholarship. Xinyu Mao is supported by the Australian Research Council Discovery Project DP210104043 (CI: Zuccon).
