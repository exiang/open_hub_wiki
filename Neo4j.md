# Open Hub Neo4j Database
Created as an extension on top of `graphaware/neo4j-php-ogm`

## Documentation

Please refer the documentation available on [ReadTheDocs](http://neo4j-php-ogm.readthedocs.io/en/latest/).


## Generate Neo4j OGM Class

OGM class can be generated under `Gii - Model Generator`
and make sure to create the none code meta data files at `\data` or `\modules\{modelName}\data` and have the following object.

```

'neo4j' => array (
        'attributes' => array(
            'name' => 'type', // name should match with created model name and type can be 'string', 'int', 'boolean' or 'float'
            'id' => 'string', // example
        )
    )

```



