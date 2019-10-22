# Setting UP ElasticSearch


Elasticsearch provides the functionality for Indexing and Search of the text documents. More information can be obtained from [official website for Elasticsearch](https://www.elastic.co/products/elasticsearch).


### Downloading and Installing

- Make sure that you have proper version of Java installed in your device

- Download extract the elasticsearch and kibana from Elasticsearch website

- Go to the bin folder and run ```./elasticsearch```

- Elasticsearch functionality can be used from Python environment which need to use package ```elasticsearch``` and ```elasticsearch_dsl```

- Create the indexing of the documents

- Run kibana by running ```./kibana``` from bin folder of extracted kibana.

- If index is successfully created, the new index can be seen at `localhost/IP:5601```

- To run kibana reotely setup ```0.0.0.0``` in the configuration file.


### Sample Application with Python

- Install packages in environment

```
pip install elasticsearch
pip install elasticsearch_dsl
```

- Setting up Indexing

```python
 request_body = {
        "settings": {
            "number_of_shards": 1,
            "number_of_replicas":0
        },
        "mappings": {
            pubmed: {
                "properties": {
                    "pmid": {
                        "type": "keyword"
                    },
                    "mesh_heading": {
                        "type": "text",
                        "similarity": "BM25"
                    },
                    "abstract":{
                        "type": "text"
                    }
                }
            }
        }
    }

es = Elasticsearch()
res = es.indices.create(index = pubmed, body = request_body)

```
- Setup search

```python
entity = "insulin"

s = Search(using=es, index="pubmed")\
                    .params(request_timeout=300)\
                    .query("match_phrase", abstract=entity)
                    
for hit in s.scan():
    print(hit.abstract)
    
```


---------------------


### Read more

1. [Elasticsearch Documentation](https://elasticsearch-py.readthedocs.io/en/master/#)

























