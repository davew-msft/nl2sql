## Using a Search Index

How?  

Consider indexing all of these things:  

* All tbl names
* col names: include metadata like PK and FKs and datatypes
* sample values: we can use this instead of few-shot

Something like this:  

```json
{ 
    "index": "database_schema", 
    "fields": [ 
        { 
            "name": "table_name", 
            "type": "Edm.String", 
            "searchable": true 
        }, 
        { 
            "name": "column_name", 
            "type": "Edm.String", 
            "searchable": true 
        }, 
        { 
            "name": "data_type", 
            "type": "Edm.String", 
            "searchable": false 
        }, 
        { 
            "name": "column_description", 
            "type": "Edm.String", 
            "searchable": true 
        }, 
        { 
            "name": "table_relationships", 
            "type": "Collection(Edm.String)" 
        } ,
        {
            "name": "vector_embedding", 
            "type": "Collection(Edm.Single)"
        }
    ] 
}

```

Then, enable _semantic search_ so the LLM understands the _meanings_ behind queries and doesn't just try to find columns and conditions that map to the natural language.  Example:  _total sales_ in a question MIGHT also roughly equate to _sales amount_ or _revenue_.  ie, they are synonyms.  

This is how you do it with the python SDKs:

```python
results = search_client.search(search_text="total sales", semantic_configuration_name="default" ) 
for result in results: 
    print(result["table_name"], result["column_name"]) 
    # Use Vector Indexing for Schema Embeddings


```

Then, do a vector index for disambiguation.  

Example:  a generic question like _list all clients from NYC_ is asked by the user.  We need the LLM to understand that client is a synonym for customer, NYC is a city and we need to find the address table for the customers and not the address table for the employees...etc etc etc.  

```python

query_vector = generate_embedding("list all clients from NYC") 
search_results = search_client.search( search_text=None, vectors={"vector_embedding": query_vector}, top=5 ) 
for result in search_results: 
    print(f"Table: {result['table_name']}, Column: {result['column_name']}")
```


How to Prioritize a given schema element? ie, I have 2 sales tables, `sales` and `sales_archive`...which do I use?  

custom scoring profile

```python
{ "scoringProfiles": [ 
    { 
        "name": "importanceScoring", 
        "text": { "weights": { "column_name": 1.5, "table_relationships": 2.0 } } 
    } ] 
}

```


What if I want to limit results to just schema based on line-of-business (I'm not talking about security).  Example, I'm logged in as a salesperson so I limit on "sales" queries to that schema and not finance.  

This is basic search "faceting" and "filtering"

```python

results = search_client.search( 
    search_text="sales by region", 
    filter="table_name eq 'SalesData' or column_name eq 'Region'", 
    facets=["table_name"] ) for result in search_results: print(result["table_name"], result["column_name"])

```