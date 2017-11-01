boofw/mongo-pdo
=========================

使用操作 MongoDB 的方式来操作 PDO 支持的 SQL 数据库

Installation
--------------

With composer :

``` json
{
    ...
    "require": {
        "boofw/mongo-pdo": "~1.0"
    }
}
```

or

```
composer require boofw/mongo-pdo:~1.0
```

Usage
------

将 `MongoCollection` 改为 `Boofw\MongoPDO\Collection` 即可将数据库从 `MongoDB` 切换到 `MySQL` 等 `PDO` 支持的关系型数据库

例如将以下代码：

```php
<?php

$mongoClient = new MongoClient('mongodb://127.0.0.1');
$collection = $mongoClient->selectCollection('myDB', 'myTable');
```

修改为：

```php
<?php

$pdo = new PDO('mysql:host=localhost;dbname=myDB', 'username', 'password');
$collection = new \Boofw\MongoPDO\Collection($pdo, 'myTable');
```

如此就可以使用 `MongoCollection` 中的接口形式来操作 `MySQL` 等关系型数据库，例如以下代码：

```php
<?php

$pdo = new PDO('mysql:host=localhost;dbname=myDB', 'username', 'password');
$collection = new \Boofw\MongoPDO\Collection($pdo, 'myTable');

$cursor = $collection->find(['id' => ['$gt' => 100]])->sort(['updated_at' => 1])->limit(20)->skip(10);
var_dump(iterator_to_array($cursor));

$collection->insert(['firstname' => 'Bob', 'lastname' => 'Jones']);
var_dump($collection->findOne(['firstname' => 'Bob']));

$collection->update(['firstname' => 'Bob'], ['$set' => ['address' => '1 Smith Lane']]);
var_dump($collection->findOne(['firstname' => 'Bob']));
