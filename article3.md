# Relation Extraction and Entity Extraction in Text using NLP | Medium 

## Codebase
- [Github](https://github.com/NikhilSrihari/entities-and-relationsintext/)

## Assumptions/Restrictions:
1. Only direct references to entities are captured
2. Description of entities isn't captured
3. Limiting relations to exact substrings
4. Only direct relations are captured

# Architecture Overview
![Image](https://i.imgur.com/I1aTHI4.png)

The input is a text string. We use it to first perform **entity detection** to obtain entities $1,2,...n$, and then we perform **relation detection** to generate relational pairs $1,2,...m$.

## Dataset
We use the MSCOCO dataset, with a focus on the 3 super categories
- Humans 
- Animals 
- Vehicles

We use the captions from the dataset as input to the pipeline, followed by running a custom rule-based parser on the captions to obtain the ground truth labels

![Image](https://i.imgur.com/jlE3adf.png)

# Entity Detection
The entity detection model in our architecture is a token classification model, obtained by fine-tuning a pre-trained BERT-base model. This model classifies each token in the input string as either "O" (not an entity), "B-ENT" or "I-ENT" (entities).

## NVIDIA NeMo
We will be using NVIDIA NeMo, an open-source toolkit with PyTorch and PyTorch Lightning backend. It provides an extra layer of abstraction composed of reusable and modular components, which helps in training complex neural network architectures quickly.


## Data Preprocessing
To use NeMo, the dataset will be divided into 4 files - `text_train.txt`, `labels_train.txt`, `text_dev.txt`, `labels_dev.txt`. Each row in each file denotes a data point. Here's an example.
![Image](https://i.imgur.com/E1fEupF.png)

## NeMo YAML Configuration
![Image](https://i.imgur.com/kEq1FMD.png)

Here's a [sample config](https://github.com/NVIDIA/NeMo/blob/v1.0.0/examples/nlp/token_classification/conf/token_classification_config.yaml).

## Inference
In the `entities_extraction/token_classification.py`, first, we call the `load_model` function, followed by the `run_inference` function with the dataset as input, which also postprocesses the output for readability

# Relation Detection
It involves 2 steps:
1. Preprocessing data to generate pairs
2. Relationship extraction with the Relation Detection model

The input is the list of all entities identified by the Entity Detection model, alongwith the input text. 

The Relation Detection model is a token-classification model, which takes in as input a text string and entities `i,j`, separated by the `[SEP]` token. The model classifies every token in the input concatenated string as either “O” (no relation), “B-REL” or “I-REL” (token captures the relation between the given entities `i,j`).

## Data Preprocessing
The dataset will be divided into 4 files - `text_train.txt`, `labels_train.txt`, `text_dev.txt`, `labels_dev.txt`. Each row in each file denotes a data point. Here's an example.

For example, for the user input text “That boy is next to the girl”, and for the entity pair (“boy”, “girl”), the entry in the text_*.txt file is “That boy is next to the girl [SEP] boy [SEP] girl” and the corresponding row in the labels_*.txt file is “O O O B-REL I-REL O O O O O O”.

![Image](https://i.imgur.com/jcpviyu.png)

## Inference
In the `relation_extraction/token_classification.py`, first, we call the `load_model` function, followed by the `run_inference` function with the dataset as input, which preprocesses the input as well as postprocesses the output for readability

In the `inferencepipeline_interactive.py` file, the `main` function executes the `inference_interactive_loop()` function, which executes the pipeline in a loop.  