# Relation Extraction and Entity Extraction in Text using NLP | Medium 

## Assumptions/Restrictions:
1. Only direct references to entities are captured
2. Description of entities isn't captured
3. Limiting relations to exact substrings
4. Only direct relations are captured

## Architecture Overview
![Image](https://i.imgur.com/I1aTHI4.png)

The input is a text string. We use it to first perform **entity detection** to obtain entities $1,2,...n$, and then we perform **relation detection** to generate relational pairs $1,2,...m$.

## Dataset
We use the MSCOCO dataset, with a focus on the 3 super categories
- Humans 
- Animals 
- Vehicles

We use the captions from the dataset as input to the pipeline, followed by running a custom rule-based parser on the captions to obtain the ground truth labels

![Image](https://i.imgur.com/jlE3adf.png)

## Entity Detection
The entity detection model in our architecture is a token classification model, obtained by fine-tuning a pre-trained BERT-base model. This model classifies each token in the input string as either "O" (not an entity), "B-ENT" or "I-ENT" (entities).

## Training
