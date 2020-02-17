# Neo4J Functions Breakdown

Notes: All Neo4j model class will have the naming convention as following `Neo4jModelName`. The `ModelName` should match with your primary model name.
## Usage

Neo4j will sync the data from your primary database by using this function under `afterSave()` function on your Model
```php
Neo4jModelName::model($this)->sync();

// Example

Neo4jUser::model($this)->sync();
```

## Examples
Find all Users

```php
Neo4jUser::model()->findAll();
```

Find one User by Attributes

```php
Neo4jUser::model()->findOneByAttributes(array('username' => 'me@domain.tld'));
```

Find one User by ID

```php
Neo4jUser::model()->findOneByPk('1');
```

Delete one User by ID

```php
Neo4jUser::model()->deleteOneByPk('1');
```

## Sync all data from primary database to Neo4j

If there is any case of database failure on Neo4j, you can sync back all the unsync data from your primary database using this function.

```php
Neo4jModelName::model()->dbSync();

// Example

Neo4jUser::model()->dbSync();
```