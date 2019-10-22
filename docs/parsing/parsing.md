## Parsing and Mapping

Parsing Different domponent of the MMS data and creation of mapping


#### Parsing ID 

```python 

    idterm = str(item['@id'].split("/")[-1])
    
    if idterm.encode('utf-8') in ["other","unspecified"]:
        ID = str(item['@id'].split("/")[-2]) + "/" + idterm
    else:
        ID = idterm
```

#### Parsing Code and ID to Code mapping

```python

     try:
        code = str(item['code'])
        id2code.update({ID:code})
    except:
        code = "NA"
        id2code.update({ID:code})

```

#### Parsing Title and ID to title mapping

```python
title = item['title']["@value"]
'''id to title mapping'''
id2title.update({ID:title})
```


#### Parsing Definition

```
try:
        defn = str(item['definition']['@value'])
    except:
        defn = "NA"
        
```


#### Parsing Foundation term

```python

    try:
        index_term =item['indexTerm'] 
        INDEX_TERM = []
        for it in index_term:
            try:
                index_term_title = str(it['label']['@value'])
            except:
                index_term_title = "NA"
            try:
                index_term_foundation_id = str(it['foundationReference'].split("/")[-1])
            except:
                index_term_foundation_id  = "NA"
                
            INDEX_TERM.append({"indexTerm_title":index_term_title,\
                              "indexTerm_foundation_id":index_term_foundation_id})
        '''update mapping table'''
        id2index.update({ID:INDEX_TERM})
    except:
        index_term = []
        '''update mapping table'''
        id2index.update({ID:INDEX_TERM})
        
```

#### Parsing Childrens

```python
    try:   
        childs = item['child']
        if len(childs) > 0:
            CHILDS = []
            for c in childs:
                child_idterm = str(c.split("/")[-1])
                if child_idterm in ["other","unspecified"]:
                    CHILDS.append(str(c.split("/")[-2]) + "/" + str(c.split("/")[-1]))
                else:
                    CHILDS.append(str(c.split("/")[-1]))
            has_children = len(CHILDS)
        else:
            CHILDS = []
            has_children = 0
            is_leaf_node = True
    except:
        CHILDS = []
        is_leaf_node = True
        has_children = 0
```


#### Parsing Parents

```python    
    try:
        parents = item['parent']
        if len(parents)>0:
            PARENTS = []
            for p in parents:
                PARENTS.append(str(p.split("/")[-1]))
        
            has_parents = len(parents)
        else:
            PARENTS =[]
            has_parents = 0
    except:
        PARENTS = []
        has_parents = 0
        
```       

#### Parsing Post Coordination Term

```python
    try: 
        POST_COORDINATION_SCALE = []
        postcoordinationScale = item['postcoordinationScale']
        for pcs in postcoordinationScale:
            try:
                allowMultipleValues = str(pcs['allowMultipleValues'])
            except:
                allowMultipleValues = "NA"
            try:
                axisName = str(pcs['axisName'].split("/")[-1])
            except:
                axisName = "NA"
            try:
                requiredPostcoordination = str(pcs['requiredPostcoordination'])
            except:
                requiredPostcoordination = "NA"
            try:
                scaleEntity = pcs['scaleEntity']
                SCALE_ENTITY = []
                if len(scaleEntity)>0:
                    for e in scaleEntity:
                        idterm = str(e.split("/")[-1])
                        if scale_idterm in ["other","unspecified"]:
                            SCALE_ENTITY.append(str(e.split("/")[-2]) + "/" + scale_idterm)
                        else:
                            SCALE_ENTITY.append(scale_idterm)
            except:
                SCALE_ENTITY = []
                
            POST_COORDINATION_SCALE.append({"allowMultipleValues":allowMultipleValues,\
                                           "axisName":axisName,\
                                           "requiredPostcoordination":requiredPostcoordination,\
                                           "scaleEntity": SCALE_ENTITY})
        '''update mapping table'''    
        id2pcs.update({ID:POST_COORDINATION_SCALE})
            
            
    except:
        POST_COORDINATION_SCALE = []
        '''update mapping table'''
        id2pcs.update({ID:POST_COORDINATION_SCALE})
    
```



