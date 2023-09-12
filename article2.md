# Patient Knowledge Graph | Medium

Knowledge graphs are a graph database, consisting of a large network of entities, their semantic types, properties, and the relations between those entities. With the help of a knowledge graph we can transform unstructured data into structured ways and extract knowledge from it to solve some important problems.

Here's an example
![Image](https://i.imgur.com/JHSOLcu.png)

A triple is a tuple of subject, predicate and object where subject and object are entities and predicate is the relationship between entities. (Mary Kom, is, Amateur Boxer) is a triple present in the above figure.

We built the knowledge graph by using the text of discharge summaries from the hospital database of [MIMIC Physionet Org](https://github.com/MIT-lcp/mimic-code). 

## Text Extraction
All discharge summaries are divided into four sections
- family history
- social history
- medical history 
- discharge diagnosis 

Since section has a fixed pattern in the discharge summary, therefore we extracted text from them using regular expressions.

## Entity and Relation Extraction
### Family History
Contains disease information of the patient’s family members from maternal and paternal side (if they had any disease) and whether family members are alive or dead. 

We defined two relations as “hasDisease” for all diseases and “is” for all death related terms. To extract disease and death term entities we used scispacy (python package containing spaCy models for processing biomedical, scientific or clinical text) to create a training file of triples, which is used to build a knowledge embedding using TransR model of OpenKE. Using this, we can train the ERNIE relation classification model to predict relation between family relationships and disease entities.

### Social History
Provides information about patient’s social life, such as if he/she is married or single, his/her profession, his/her habits such as smoking or drinking etc. As the social history section contains the general information of the patient, we have used Stanford Open Information Extraction 5.1 to extract triple from text. Here, for segmentation we have used spaCy again.

### Medical History
Provides information of disease the patient had in the past and sometimes the reason of his/her illness.

### Discharge Diagnosis
Contains information of diagnoses or procedures done on the patient during the hospitalization.

The medical terms of our interest in both Medical History and Discharge Diagnosis sections are the name of the diseases, procedures. Hence, here one entity is disease name or procedure name, where the disease is extracted using scispacy and the procedure is extracted using BioBERT. And the other entity is patient itself. The relation is “hasDisease” and “hasProcedure” which are defined manually.

## Build Knowledge Graph 
After extracting the entities and relations from text, all of this information is stored in the JSON file of their respective sections. In our project, we have used the OrientDB graph database to copy information from all these JSON files by using database schemas.

![Image](https://i.imgur.com/FCsuji1.png)

## Information Retrieval and Visualization
Using the OrientDB query language, we can retrieve the large scale knowledge from the graph. 

Here are some examples
- What are the diseases associated with patients who smoke?
- How many patients with disease `<disease name>` AND family history of disease `<disease name>`?
- What are the diseases for patients with a family history of disease `<disease name>`?