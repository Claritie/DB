# Stash Queries

<p align="center">
    <img src="http://travisfont.com/images/stash-queries-carbon.png" width="990" height="405" />
</p>

Stash Queries is a PHP SQL library providing an easy, eloquent, and fluent yet expressive way to select, create, update, and delete SQL records. Overall, it's a simple thin wrapper around PHP's PDO database abstraction layer. The layer fluency provides a direct way to interact with SQL using actual SQL files.

This library is originally part of Skyfire's PHP framework database layer known as *DB* service.

_Note:_ Stash Queries isn't an ORM in any way or shape, but rather a wrapper replacement for native PDO.

_Currently Stash Queries works only with MySQL and PostgreSQL databases. Support for other platforms is planned for later releases._

## Requirements

- PHP >=5.6+
- PDO_MYSQL PHP extensions

*Due to security and feature reasons, PHP 5.6 is required.*
<br/>*At most by standards, you should be using a PHP version above or at 5.6 by now.*

## Code Examples

```php
// setting the DB display encoding type (if needed)
FixCollation::charset('utf-8', FixCollation::TEXT_HTML);

// setting Database credentials
DB::define('stash_dir',   getcwd()); // or dirname(__FILE__)
DB::define('host',       'localhost');
DB::define('dbname',     'test_db1');
DB::define('dbuser',     'root');
DB::define('dbpassword', '');


// SQL select query (with prepare variables)
$prepare = array
(
    'label' => 'test'
);
$data = DB::select('get.HomeTextByLabel')->prepare($prepare);
var_dump($data);


// SQL simple select query
$data = DB::select('get.AllHomeTextData')->execute();
var_dump($data);


// raw SQL query (with prepare variables)
$data = DB::query('SELECT * FROM test WHERE data IS NOT NULL AND id > :count AND data != :text', array
(
    ':id'   => 10,
    ':text' => 'test'
))->execute();
var_dump($data);


// displays the prepare update statement in plain text (ideal for debugging queries)
$query = DB::update('PostfromTestById')->text($prepare);
echo $query;
```

## Injections:

To assign dynamic variables outside queries without binding (secured injections) such as table names and fields which are not possible by PDO binding:
```php
$data = DB::select('get.fieldData.byId')->inject(array
(
    'field' => $data->field,
    'table' => $table_name
))->prepare(array('id' => (int) $record->id));
```

## Persistent Connections:

To have persistent connections for all queries, this can be done by defining it in the DB configurations - as such:
```php
DB::define('persistent', TRUE);
// DB::define('persistent', 'yes');
```
Both examples above work perfectly, only TRUE or 'yes' are allowed.


## Creating Query Folders

If query don't exist (or uncertainity of their existance), calling the following function will create the query folders if they do not exist. The function will return back FALSE, if no directories were created, or the count of (e.g. 4) directories created.
```php
DB::createQueryDirectories();
```
Note: It's important to place this function after all DB::define() calls.


## Creating a database if not existing (shortcut)
```php
DB::createNotExist('database_name');
```
Note: It's important to place this function after all DB::define() calls.


## Installation:
Injecting the code mamually, you only have to include 'StashQueries.php' as such:
```php
require_once 'DB/StashQueries.php';
```
Also, externally through composer by adding 'SkyfirePHP/DB' to composer dependencies in composer.json:
```json
{
    "require": {
        "skyfirephp/db": "dev-master"
    }
}
```

## License

Stash Queries is licensed under the [MIT License](http://opensource.org/licenses/MIT).

Copyright 2015-2017 [Travis van der Font](http://travisfont.com)
