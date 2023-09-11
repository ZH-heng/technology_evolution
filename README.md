### Revealing the Technology Evolution of Natural Language Processing: A Scientific Entity-Centric Perspective

### 1. Overview

**Dataset and source code for paper "Revealing the Technology Evolution of Natural Language Processing: A Scientific Entity-Centric Perspective".**

This study analyzes the technology evolution of the NLP field from a scientific entity perspective. This project includes relevant codes for entity recognition and datasets, along with programs for entity normalization and calculating entity influence.

### 2. Directory structure

<pre>
technology_evolution                               Root directory
├── Code                                           Source code folder
│   ├── entity-normalization.ipynb                 Source code for entity normalization
│   ├── ner-base.py                                Source code for baseline models
│   ├── ner-cascade.py                             Source code for the SciBERT+BiLSTM(cascade) model
│   └── z-score_calculation .ipynb                 Source code for calculating impact of entities
├── Dataset                                        Dataset folder
│   ├── mapping-list.txt                           "abbreviation-full name" mapping dictionary for entities
│   ├── mdmt.parquet                               Our annotated dataset
│   ├── paper-ents.parquet                         Extracted entities of papers
│   ├── pid2conf.txt                               Dictionary of paper_id to its conference
│   ├── remove-words.txt                           Words with little semantic contribution
│   ├── scierc.parquet                             Open dataset SciERC
│   ├── tdm.parquet                                Open dataset TDM
│   └── top-ents.csv                               Full list of high-impact entities
└── README.md
</pre>

### 3. Technology-related entity recognition

- **Dataset**

"*./Dataset/mdmt.parquet*": Parquet format. Our self-annotated dataset. As further research is required, we make available a portion of the data which includes 500 training samples, 100 validation samples, and 100 test samples. This dataset comprises three fields, namely 'word', 'label', and 'type'. The field 'type' is used to differentiate the trainset, validset, and testset. The entities we annotated consist of four types: method, dataset, metric, and tool.

*word': ['According', 'to', 'Chen', 'et', 'al', '.', '(', '2016', ')', ',', 'Bilinear', 'outperforms', 'multi-layer', 'forward', 'neural', 'networks', 'in', 'relevance', 'measurement', '.']*

*'label': ['O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'S-Method', 'O', 'B-Method', 'I-Method', 'I-Method', 'E-Method', 'O', 'O', 'O', 'O']*

*'type': 'train'*

"*./Dataset/scierc.parquet*": The open dataset SciERC, consists of six entity types, namely, Task, Method, Metric, Material, Other-ScientificTerm, and Generic.

"*./Dataset/tdm.parquet*": The open dataset TDM, defining three types of entities: Task, Dataset, and Metric.

- **Code**

"*./Code/ner-base.py*": Code for baseline models.

"*./Code/ner-cascade.py*": Code for the SciBERT+BiLSTM(cascade) model.

- **Model evaluation**

Evaluation of models on our annotated dataset

|Model|P|R|F1|
|-|-|-|-|
|BERT|83.64|85.92|84.77|
|BERT+CRF|84.10|84.45|84.28|
|RoBERTa|85.84|81.51|83.62|
|RoBERTa+CRF|83.86|82.98|83.42|
|T5|84.41|85.29|84.85|
|T5+ CRF|85.08|85.08|85.08|
|SciBERT|85.95|86.13|86.04|
|SciBERT+CRF|83.53|87.39|85.42|
|SciBERT+BiLSTM (cascade)|86.22|86.76|86.49|
|SciBERT+BiLSTM (cascade)+data_aug|86.82|87.18|**87.00**|

Evaluation on SciERC and TDM dataset

|**Dataset**|**Authors**|**Model**|**P**|**R**|**F1**|
|-|-|-|-|-|-|
|SciERC|Luan et al. (2018)|SCIIE| 67.20 | 61.50 | 64.20 |
||Zhong and Chen (2021)|PURE||| 68.90 |
||Eberts and Ulges (2021)|SpERT| 70.87 | 69.79 | 70.33 |
||Zaratiana et al. (2022)|Hierarchical Transformer| 67.99 | 74.11 | **70.91** |
||Our|SciBERT+BiLSTM (cascade)+data_aug| 66.95 | 71.49 | 69.14 |
|TDM|Hou et al. (2021)|SCIIE| 67.17 | 58.27 | 62.40 |
||Zaratiana et al. (2022)|Hierarchical Transformer| 65.56 | 70.21 | 67.81 |
||Our|SciBERT+BiLSTM (cascade)+data_aug| 68.84 | 70.73 | **69.77** |

### 4. Entity normalization

"*./Code/entity-normalization.ipynb*": Normalizing entities based on edit distance similarity and hierarchical clustering. The relevant resources can be found in the "data" folder.

A total of 534,500 entities were extracted, and the number of entities after normalization was 268,392.  Subsequently, we filtered out entities with an annual frequency of less than 5, and ultimately obtained 37,624 valid technology-related entities. Each valid entity corresponds to a cluster in the clustering result. To verify the effectiveness of entity normalization, we randomly selected 1000 pairs of entities from these entity clusters and manually judged whether they belonged to the same entity. The precision metric, calculated based on the human reviews and the normalization results, was 91.30.

### 5. z-score calculation

"*./Code/z-score_calculation.ipynb*": After completing entity normalization, the co-occurrence networks are constructed based on papers for each year, and the z-scores of entities are calculated to measure their impact.The relevant resources can be found in the "Dataset" folder.

Entities with z-scores exceeding 2.5 are defined as high-impact technology-related entities. Since 2001, 179 high-impac new entities have emerged in the NLP field, and their complete list can be found in the file "*./Dataset/top-ents.csv*".

The top 5 entities for each type are as follows:

|**Type**|**Entity**|***z-score***|**Type**|**Entity**|***z-score***|
|-|-|-|-|-|-|
|Method|BERT|43.3138|Metric|BLEU|15.9303|
||Transformer|34.6696||Cross-Entropy|13.1292|
||LSTM|28.8231||ROUGE|7.8905|
||Attention Mechanism|26.2604||Fluency|6.9009|
||Adam|20.3561||Standard Deviation|6.1762|
|Dataset|Wikipedia|17.4187|Tool|PyTorch|6.1565|
||MNLI|6.7163||MOSES|5.3327|
||SQuAD|5.7830||GIZA++|5.2089|
||Twitter|5.3056||TensorFlow|3.5630|
||SST-2|5.2605||Stanford Parser|3.2967|

### 6. Dependency packages



