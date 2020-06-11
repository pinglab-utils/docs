
### ICD 11 Graph
 
NetworkX and Neo4j has been implemented for creation of ICD 11 graph. The parsed document for each of the node in ICD 11 graph has the following propreties:

```python
{
"id": "496418968",
"code": "EB13.2",
"title": "Stevens-Johnson and toxic epidermal necrolysis overlap syndrome",
"defn": "A severe reactive skin disorder with features of both toxic epidermal...",
"syns": "NA",
"childs": ["1077457356"],
"parents": ["195467267"]
}
```

### Implementation of NetworkX

- Install networkx 

```bash
pip install networkx
```
- Import networkx in the current environment

```python
import networkx as nx
```

- Initiate vacant graph
```python
G = nx.Graph()
```
- Introduce node id and properties from each item of parsed Data

```python
            G.add_node(item_id,\
                   title = item['title'],\
                   code = item['code'],\
                   defn = item['defn'],\
                   childs = item['childs'],\
                   parents = item['parents'])
         
```
- Introduce child relationship as edges of the graph from each item of parsed Data

```python
        childs = item['childs']
        for c_id in childs:
             G.add_edge(item_id,c_id)
```
                
### Implementation of Neo4j (EPOCH)

- Install neo4j python driver

```bash
pip install neo4j
```
- Import neo4j to current environment

```python
from neo4j import GraphDatabase
```             
- Create a python driver

```python
driver = GraphDatabase.driver(uri = "bolt://localhost:7687",\
                      auth = ("user","password"))
```                
 - Clean the graph database, if using with pre-existing data
 
```python
with driver.session() as session:
        session.run("MATCH (n:icd11_code) DETACH DELETE n")
```
- Create nodes in the graph

```python
with driver.session() as session:
     session.run("call apoc.load.json('file:///clean_mms_graph_data.json')"
                "yield value create (node:icd11_code) set node = value.props;")
```
                
- Create an index on each node's id property, otherwise the next command will take too long

```python

with driver.session() as session:
    session.run("create index on :icd11_code(id)")
```


- Create edge with relationship 'child_of'

```python
with driver.session() as session:
     sesh.run("""match (n:icd11_code) with n , n.child_nodes as child_nodes
     		       match (m:icd11_code) where m.id in child_nodes  with n, m
        	       create (n)<-[:child_of]-(m)""")
```

### Implementation of Neo4j (Individual)

- Install neo4j python driver

```bash
pip install neo4j
```
- Import neo4j to current environment

```python
from neo4j import GraphDatabase
```             
- Create a python driver

```python
driver = GraphDatabase.driver(uri = "bolt://localhost:7687",\
                      auth = ("user","password"))
```                
 - Clean the graph database, if using with pre-existing data
 
```python
with driver.session() as session:
        session.run("MATCH (a) DETACH DELETE a")
```
- Create nodes in the graph

```python
for item in Data:
    with driver.session() as session:
        session.run("MERGE(a:Disease{ID: $ID}) "
                        "ON CREATE SET a.code = $code, \
                        a.title = $title, a.defn = $defn,\
                        a.syns = $syns, a.childs = $childs,\
                        a.parents = $parents",
            ID=item['id'], code=item['code'], title=item['title'],\
            defn=item['defn'], syns=item['syns'], childs=item['childs'],\
            parents=item['parents'])
```
                
- Create edge with relationship "parents"

```python

with driver.session() as session:
    session.run("MATCH (a:Disease),(b:Disease) "
            "WHERE a.ID in b.parents "
            "MERGE (a)<-[r:Parent]-(b)")
```


- Create edge with relationship "children"

```python

with driver.session() as session:
    session.run("MATCH (a:Disease),(b:Disease) "
            "WHERE a.ID in b.childs "
            "MERGE (a)<-[r:Child]-(b)")
```


                
                
                
