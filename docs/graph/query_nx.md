
## Query and Algorithms
 
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

### Exploring single node

These steps are dedicated to explore the single node information

#### Finding root information

For the provided IDs of root nodes, we can extrace the root node information

```python
ROOT = [n for n in G.neighbors("ICD11")]
INFO = []
for node in ROOT:
        try:
            INFO.append({"ID": node,\
                     "title": G.nodes()[node]["title"],\
                     "defn":  G.nodes()[node]['defn'],\
                     "child": len([n for n in G.neighbors(node)])})
        except:
            
             INFO.append({"ID": node,\
                     "title": "NA",\
                     "defn":"NA",\
                     "child": "NA"})
```


#### Finding Root node 

For a specific node, one can find the corresponding root node

```python
'''Prepare root info'''
root_of_source_node = nx.shortest_path(G,source=ROOT,target=source_node)[1]
```

#### Finding neighborhood 

For a specific node, neighborhood node information can be collected.

```python
'''Collect Nrighbour info'''
source_neighbours = G.neighbors(source_node)
NBD = []
for item in source_neighbours:    
      NBD.append({"id":item,\
                 "title":G.nodes()[item]['title'],\
                  "code" : G.nodes()[item]['code']})
```

#### Finding path information 

For a given node, the path from root to the specific node path information can be collected.

```python

'''Collect Path Info'''
path = nx.shortest_path(G, ROOT, source_node)
        
PATH_NAMES = []
for item in path:
    PATH_NAMES.append({"id":item,\
                       "title":G.nodes()[item]['title'],\
                       "code" : G.nodes()[item]['code']})

```

#### Collect Subgraph

For a specific node, the subgraph can be created by extracting neighbourhood of the node.

```python
'''Collect Subgraph info'''
SG = nx.Graph()
for node in path:
    SG.add_node(node,title = G.nodes()[node]['title'],\
                          defn = G.nodes()[node]['defn'],\
                          code = G.nodes()[node]['code'])
            
    nbd = [n for n in G.neighbors(node)]
    for item in nbd:
         try:
           SG.add_node(item,title = G.nodes()[item]['title'],\
                          defn = G.nodes()[item]['defn'],\
                          code = G.nodes()[item]['code'])
         except:
           SG.add_node(item,title = "Key not found",\
                          defn = "key not found",\
                          code = "key not found")
               
    for item in nbd:
          SG.add_edge(node,item)
```

---------------------



### Exploring two node association

Following are the methods to explore node to node association

#### Source and target node

Collect source and target node information

```python
        '''identify source and target'''
        source_node = result["source"]
        target_node = result["target"]
        source_title = G.nodes()[source_node]['title']
        target_title = G.nodes()[target_node]['title']
        source_defn =  G.nodes()[source_node]['defn']
        target_defn =  G.nodes()[target_node]['defn']
```

#### Neighborhood of nodes

```python
        
        '''find neighbourhood info'''
        source_neighbours = G.neighbors(source_node)
        target_neighbours = G.neighbors(target_node)
        
        s_NBD = []
        for item in source_neighbours:
            s_NBD.append({"id":item,\
                        "title":G.nodes()[item]['title'],\
                        "code" : G.nodes()[item]['code']})
            
        t_NBD = []
        for item in target_neighbours:
            t_NBD.append({"id":item,\
                        "title":G.nodes()[item]['title'],\
                        "code" : G.nodes()[item]['code']})
```            
            
#### Root node information            

```python

        '''find root info'''
        source_root = nx.shortest_path(G,source=ROOT,target=source_node)[1]
        target_root = nx.shortest_path(G,source=ROOT,target=target_node)[1]
        source_root_title = G.nodes()[source_root]['title']
        target_root_title = G.nodes()[target_root]['title']
        source_root_defn =  G.nodes()[source_root]['defn']
        target_root_defn =  G.nodes()[target_root]['defn']
        
```

#### Path information

```python
        
        '''Collect path items'''
        path = nx.shortest_path(G, source_node, target_node)
        if "ICD11" in path:
            common_child == False
            
        PATH_NAMES = []
        for item in path:
            PATH_NAMES.append({"id":item,\
                               "title":G.nodes()[item]['title'],\
                               "code" : G.nodes()[item]['code']})
```

#### Subgraph Information

```python
            
        '''Find subgraph info'''
        SG = nx.Graph()
        for node in path:
            nbd = [n for n in G.neighbors(node)]
            SG.add_node(node)
            for item in nbd:
                SG.add_node(item)
            for item in nbd:
                SG.add_edge(node,item)
```







            
        
        
        
        
                
                
                
