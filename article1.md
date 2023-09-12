# Knowledge Graphs Introdution
Knowledge Graph is a knowledge base of entities and the relationships between them

# Understanding Knowledge Graphs
Knowledge Graphs are a way of storing data that allows for more complex queries than a traditional database.
Here Graph means a collection of nodes and edges. Nodes are entities and edges are relationships between them. It contains facts in the form of "SPO" triples (subject, predicate, object) 

It aquires and integrates information into an ontology and applies a reasoner to derive new knowledge. 

Ontology: a set of concepts and categories in a subject area or domain that shows their properties and the relations between them


# Description of Entities of Knowledge Graphs

Descriptions have formal semantics which allow people and machines to process them in a way that is unambiguous and consistent.

Entity Description contributes to the one another in the forming a network where each entity represents part of description of another entity.


# Creating Knowledge Graphs

## Data Acquisition:
Data can be acquired  or can be created manually.

## Generating RFD triples and quads:
RML(RDF Mapping language) rules where the YARRRML rules are translated along with the execution of the RMLMapper with the corresponding RML rules to generate RDF(Resource Description Framework) triples and quads.

(YARRRML : A human readable text-based representation for declarative Linked Data generation rules.)

(RML is a generic scalable mapping language defined to express rules that map data in heterogeneous structures and serializations to the RDF data model.)

## Storing Triples in Graph Database:

Storing the generated triples and quads in graph databases for future uses.

Some Available Graph Databases:

Neo4j
ArangoDB etc

## Mapping of Data to Knowledge Graph:

We can use RDFlib for mapping of RDF data into Knowledge graphs.

OR,

We can directly visualize the Knowledge Graph from the Graph Database.

# Applications of Knowledge Graph:
## Question Answering System
## Recomender System
## Information Retrieval
## Domain Specific
