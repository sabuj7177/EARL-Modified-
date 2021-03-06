# EARL
## Joint Entity and Relation Linking for Question Answering

EARL (Entity and Relation Linker), a system for jointly linking entities and relations in a question to a knowledge graph. EARL treats entity linking and relation linking as a single task and thus aims to reduce the error caused by the dependent steps. To realise this, EARL uses the knowledge graph to jointly disambiguate entity and relations. EARL obtains the context for entity disambiguation by observing the relations surrounding the entity. Similarly, it obtains the context for relation disambiguation by looking at the surrounding entities. We support multiple entities and relations occurring in complex questions by modelling the joint entity and relation linking task as an instance of the Generalised Travelling Salesman Problem (GTSP).

ISWC 2018 Research Paper: https://link.springer.com/chapter/10.1007/978-3-030-00671-6_7 or on arXiv https://arxiv.org/abs/1801.03825

**Note**: The code in this branch is not an exact representation of the paper above. In particular we use https://github.com/AskNowQA/erpredictor2 for entity and relation classification now. This allows us to handle questions in a case-independent fashion, the lack of which was a weakness of the original paper.

# SETUP INSTRUCTIONS

**The code in this branch is compatible with Python 3.8.**

Install all python dependencies required that are mentioned in requirements.txt. Download bloom files from https://drive.google.com/drive/folders/1lKu0tVA5APhZVOZqRQK2tCk0FDj82lvo?usp=sharing and store them at data/blooms/.  Download the archived elastic search dumps from the same google drive link and import them into a local running elasticsearch 6.8.5 instance. The mappings can be found in data/elasticsearchdump/ folder. Download https://www.dropbox.com/s/flh1fjynqvdsj4p/lexvec.commoncrawl.300d.W.pos.vectors.gz?dl=1, unzip it, and store it in data/ folder. Download https://drive.google.com/file/d/1IEkWGq89bc2UgWj--JkxOKztjXxxjkec/view?usp=sharing (unzip after download) and store it in data/ folder.

To import elasticsearch data one could install elasticdump https://www.npmjs.com/package/elasticdump

    npm install elasticdump -g

Then import the mapping:

    elasticdump --input=dbentityindex11mapping.json  --output=http://localhost:9200/dbentityindex11 --type=mapping

Then import the actual data:

    elasticdump --limit=10000 --input=dbentityindex11.json  --output=http://localhost:9200/dbentityindex11 --type=data

Now go to EARL/ code checkout folder:

    $cd scripts/

Download SENNA parser from https://ronan.collobert.com/senna/ and unzip in this folder, then:

    $python TextMatchServer.py 8888  (this takes several minutes to load)
    $python api.py 4999

This starts the EARL api server at port 4999.

You may need to add the following to the above elasticdump commands to make it work on some setups:

    --headers='{"Content-Type": "application/json"}'


To consume the API

    curl -XPOST 'localhost:4999/processQuery' -H 'Content-Type: application/json' -d"{\"nlquery\":\"Who is the president of USA?\"}"

# LIVE API

  There is a live version of this api hosted by th LT Group, University of Hamburg. It may be accessed in the following manner:

    curl  -XPOST 'http://ltdemos.informatik.uni-hamburg.de/earl/processQuery' -H 'Content-Type:application/json' -d'{"nlquery":"Who is the president of Russia?"}'


# Additional info:
Necessary software: 

JDK 11 with JAVA_HOME

visual studio with python and c development 

node.js(for elastic dump)

elasticsearch-6.8.5

Need pytorch 1.4.0:

pip install torch==1.4.0+cpu torchvision==0.5.0+cpu -f https://download.pytorch.org/whl/torch_stable.html

For running elastic search: Go to 'C:\elasticsearch-6.8.5\bin' with command prompt and type 'elasticsearch' and press enter.
# Acknowledgement:
This repo is from https://github.com/AskNowQA/EARL

It has been changed for running in windows 10. 