    
# Indexing of ICD 11 Documents
    
Indexing of documents is done in two steps


### Import the required libraries

```python
from elasticsearch import Elasticsearch
from elasticsearch_dsl import Search, Q
```

### Initialization of Index
    
- Prepare Name and number of shards, clusters etc.

```
INDEX_NAME = "icd11"
NUMBER_SHARDS = 1 
NUMBER_REPLICAS = 0
```
- Prepare request body

```python
request_body = {
        "settings": {
            "number_of_shards": NUMBER_SHARDS,
            "number_of_replicas": NUMBER_REPLICAS
        },
        "mappings": {
                "properties": {
                    "id": {
                        "type": "keyword"
                    },
                    "tree":{
                        "type": "text"
                    },
                    "name":{
                        "type": "text"
                    },
                    "root":{
                        "type": "text"
                    },
                    "degree":{
                        "type": "integer"
                    },
                    "definition":{
                        "type": "text"
                    },
                    "synonym":{
                        "type": "text"
                    }
                }
            }
        }
    
```

- Call Elasticsearch

```python
es = Elasticsearch()
```



- Facilitate deleting old indexing if already exist


```python

if es.indices.exists(INDEX_NAME):
     res = es.indices.delete(index = INDEX_NAME)
     print("Deleting index %s , Response: %s" % (INDEX_NAME, res))
```  

- Create new Indexing 

```python
res = es.indices.create(index = INDEX_NAME, body = request_body)
print("Create index %s , Response: %s" % (INDEX_NAME, res))
```

### Populating the Index

- For each item in the data list, create the data dictionary and op dictionary

```python

'''create data dictionary'''

data_dict = {}
data_dict["id"] = item["id"]
data_dict["tree"] = item["tree"]
data_dict["root"] = item["root"]
data_dict["name"] = item["name"]
data_dict['parents']=item['parents']
data_dict['childs'] = item['childs']
data_dict["sibls"] = item["sibls"]
data_dict["degree"] = item["degree"]
data_dict["synonym"] = item["synonym"]
data_dict["definition"] = item["definition"]

'''create  op dictionary'''
op_dict = {
     "index": {
          "_index": INDEX_NAME,
          "_id": data_dict["id"]
                    }
                }
```


- Add each data dictionary into bulk data list

```python

'''Put current data into the bulk''' 

bulk_data.append(op_dict)
bulk_data.append(data_dict)
```
- Push the doc dictionary to Indexing 

```python
INDEX_NAME = "icd11"
bulk_size = 50
es = Elasticsearch()
es.bulk(index=INDEX_NAME, body=bulk_data, request_timeout = 500)

```



