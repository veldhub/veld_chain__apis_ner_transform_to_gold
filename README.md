# ![veld chain](https://raw.githubusercontent.com/veldhub/.github/refs/heads/main/images/symbol_V_letter.png) veld_chain__apis_ner_transform_to_gold

This repo contains [chain velds](https://zenodo.org/records/13322913) encapsulating extraction and 
conversion of all gold data from https://gitlab.oeaw.ac.at/acdh-ch/apis/spacy-ner

## requirements

- git
- docker compose (note: older docker compose versions require running `docker-compose` instead of 
  `docker compose`)

Clone this repo with all its submodules
```
git clone --recurse-submodules https://github.com/veldhub/veld_chain__apis_ner_transform_to_gold.git
```

## how to reproduce

The following chain velds were used. Open the respective veld yaml file for more information.

**[./veld.yaml](./veld.yaml)** 

Reruns the transformation.

```
docker compose -f veld.yaml up
```

### notes on duplicated legacy code:

Since some of the gold data is persisted as python pickle and contains references to classes from
the spacy-ner repo, these classes and their code context was copied from 
https://gitlab.oeaw.ac.at/acdh-ch/apis/spacy-ner/-/tree/8e75d3561e617f1bd135d4c06fbb982285f6f544/notebooks/ner 
into here: [./src/ner/](./src/ner/)

### notes on skipped conversion of evalset.json

There is one file (`ner_apis_2020-04-30_11:24:09/corpus/evalset.json`), that is encoded as json
where the texts are tokenized and the entities attached to the tokens in the BILOU format.

This tokenized data structure would make conversion to the harmonized data output difficult,
since the full text needs to be restored from the tokens but that's not possible with certainty
(where to put what whitespaces in between what tokens?). And using the original text and
aligning the entities to it is also difficult since a correspondence between text and tokens
would need to be implemented, while also calcuating the offset indices of entity substrings. 
Possible, but quite some work.

Since I've observed plenty of redundancies among the various data sets from the spacy-ner repo,
I rather implemented an evaluation function to see if any of the json data is actually unique, and
hence worth the effort described above.

**As it turns out, there are no texts in the json file not found in the other datasets,
meaning the json data likely is the product of some processing of the other data.** 

This means that converting this json data would not create new unique data and given the effort
outlined above, it is simply not worth the effort.

The function implementing a simple comparison is `evaluate_json_data` in `./src/convert.py`. 

