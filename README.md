# Exploring-Roles-of-Abnormal-Metabolites-in-Kynurenine-Pathway-by-Using-Knowledge-Graph-Approach
KyPath
This repository provides the resources and protocols for constructing the Kynurenine Pathway (KyPath) knowledge graph, as presented in the associated research. KyPath is a biomedical knowledge graph designed to explore the role of the kynurenine pathway (KP) in depression, integrating its complex relationships with neurotransmitters, inflammatory molecules, and associated biological pathways. This project is a collaboration between researchers at Weifang University of Science and Technology, the Chinese Academy of Agricultural Sciences, and the Knowledge Representation & Reasoning (KR&R) Group at Vrije Universiteit Amsterdam.

Datasets
The following datasets were integrated to construct the KyPath knowledge graph and are available via the releases page:

KyFact - Entities and relations manually curated from pathway images and text descriptions in the literature.

KyCept - Named entities automatically recognized and semantically annotated from biomedical literature abstracts and titles using the CI-er tool, linked to UMLS and SNOMED CT.

KyRela - Relations between entities extracted using our fine-tuned BioKyBERT model, trained specifically on kynurenine pathway literature.

MiKG - A previously constructed knowledge graph focusing on neurotransmitter interactions, from the MiKG paper.

PPKG - A previously constructed knowledge graph documenting pre-/probiotic and microbiota-gut-brain axis disease relationships, from the PPKG paper.

WikiPathways - Biological pathway data for Homo sapiens.

Gene Ontology - Attributes and functions of genes and gene products.

KEGG Pathways - Pathway profiles related to neurotransmitters and metabolism.

DrugBank - Information on drug targets, pharmacology, and metabolism.

UMLS (Unified Medical Language System) - A comprehensive biomedical vocabulary and ontology.

SNOMED CT - A standardized clinical healthcare terminology.

The pre-trained language model used as the foundation for our relation extraction task is BioBERT-Base v1.1 (repository).

Workflow
1. Retrieving Articles
Relevant PubMed IDs (PMIDs) were retrieved using the following query focusing on the kynurenine pathway and depression:
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term="Kynurenine+Depression"&RetMax=10000
The resulting PMID list was saved locally as a text file in the pmid2html/data/pmidlist/ directory.

2. Downloading Articles
The run-PubmedDownload.bat script in the pmid2html directory was executed to download the title and abstract of each article in HTML format, using the local PMID list.

3. Manual Fact Curation from Pathway Images (KyFact)
Hand-drawn pathway images and descriptive text from publications were manually collected.

Key biological entities (e.g., metabolites, enzymes) and their relationships (e.g., catalyzes, inhibits) were extracted from these sources.

These facts were structured as RDF triples (subject-predicate-object).

Entities were mapped to standardized identifiers from databases like KEGG using owl:sameAs statements.
The output is the KyFact.ttl file.

4. Named Entity Recognition and Semantic Annotation (KyCept)
The CI-er (Concept Identifier) tool was used to perform Named Entity Recognition (NER) on the downloaded article titles and abstracts. Identified entities were linked via URIs to concepts in the UMLS and SNOMED CT terminologies to ensure semantic consistency. The resulting annotations are stored in the KyCept dataset.

5. Knowledge Graph Construction in GraphDB
5.1. Downloading and Starting GraphDB
Download the free edition of GraphDB from the official site. Ensure Java 8 is installed. Start the GraphDB server and access the Workbench interface at http://localhost:7200.

5.2. Creating a Repository
In the Workbench, navigate to Setup -> Repositories. Create a new repository with the ID KyPath.

5.3. Importing Data
Import all RDF dataset files (.ttl, .rdf) via Import -> RDF -> User data. For large files, place them in the GraphDB import directory and use the Server files import option.

5.4. Accessing and Querying Data
The integrated knowledge graph can be queried using the SPARQL language from the SPARQL menu in the Workbench. Query templates and case studies are provided in the sparql-codes directory.

6. Relation Extraction with BioKyBERT
6.1. Preparing Training Data
Sentences containing co-occurrences of 'kynurenine pathway' entities and 'neurotransmitter' entities were retrieved from the knowledge graph (see Case 1 query). Entity mentions in these sentences were normalized with special tags (e.g., @KYN$). Each sentence was labeled (1 for relation present, 0 for absent) based on biological knowledge to create a supervised dataset.

6.2. Fine-tuning and Evaluation
The BioBERT model was further pre-trained on a corpus of kynurenine-related abstracts (domain adaptation) and then fine-tuned on the labeled relation extraction dataset. The resulting model is termed BioKyBERT. Model performance was evaluated using five-fold cross-validation. The extracted relations form the KyRela.ttl dataset, which is subsequently imported into the KyPath GraphDB repository.

Biological Query Cases
To demonstrate the utility of KyPath, four biological questions were formulated and answered using SPARQL queries:

Which neurotransmitters does KP affect? (Retrieves associated neurotransmitters and evidence)

How does KP affect neurotransmitters? (Investigates metabolic pathways and mechanisms)

What are the pathway relations for kynurenine in the literature? (Synthesizes relationships from curated images and literature)

How are neurotransmitters, inflammatory molecules and kynurenine related? (Explores the integrated neuro-immune role of KP)

Query templates and results for these cases are available in the supplementary materials and the sparql-codes directory.

More Information
Please contact the corresponding author, Pengfei Huang (huangpengfei@caas.cn), for questions regarding this knowledge graph, the construction process, datasets, or query protocols.

Citation
If you use KyPath in your research, please cite the associated work:

text
@article{cong2025kyPath,
    author = {Peifei Cong and Pengfei Huang and Zhisheng Huang},
    title = {Exploring Roles of Abnormal Metabolites in Kynurenine Pathway by Using Knowledge Graph Approach},
    journal = {[Journal Name]},
    year = {2025},
    volume = {},
    number = {},
    pages = {},
    publisher = {},
    doi = {[DOI]}
}
