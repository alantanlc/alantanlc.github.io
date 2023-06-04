---
layout: post
categories: blog
---

# Implementing A Service That Toggles Between Cache And Database

## Overview

1. Model
1. Data Access Object
1. Service

## Model

```python
from dataclasses import dataclass

@dataclass
class Person:
    """ Person class. """
    id: str
    first_name: str
    last_name: str

```

## Data Access Object

Data Access Object (DAO) is an abstract light object to provide connection to the datastore (e.g. cache, database).

We define an abstract `PersonDao` class which will be inherited by a database and cache class.

**PersonDao**

```python
from abc import ABC

class PersonDao(ABC):

    def connect(self):
        pass

    def get(self, id):
        pass

```

**PersonDbDao**

PersonDbDao connects to a database and performs SQL queries.

```python
import mysql.connector

class PersonDbDao(PersonDao):

    def __init__(self):
        self.cursor = self.connect()

    def connect(self):
        db = mysql.connector.connect(
            host = "localhost",
            user = "username",
            password = "passoword"
        )
        return db.cursor()

    def get(self, key):
        query = f"SELECT * FROM PERSON WHERE ID = '{key}'"
        self.cursor.execute(query)
        return self.cursor.fetchone()

```

**PersonCacheDao**

PersonCacheDao connects to a cache and performs cache operations.

```python
from pymemcache.client import base

class PersonCacheDao(PersonDao):

    def __init__(self):
        self.client = self.connect()

    def connect(self):
        client = base.Client(('localhost', 11211))
        return client

    def get(self, key):
        return self.client.get(key)

```

## Service

The server layer provides logic to operate on the data sent to and from the DAO and client. For security reasons, this layer should not have any relation to the database or cache.

We define a PersonService interface with an abtract method `get` which must be implemented by the implementing class.

**PersonServiceInterface**:

```python
from abc import ABC

class PersonServiceInterface(ABC):

    @abstractmethod
    def get(self, id):
        """ Get person by id. """
        pass

```

**PersonServiceImpl**:

PersonServiceImpl implements PersonServiceInterface:

1. A `use_db` flag is used to determine whether to perform the operation on the database or cache.
1. When using cache, we should also handle cache-miss scenarios by looking up the entry from database and putting it into cache.

```python
class PersonServiceImpl(PersonServiceInterface):

    def __init__(self):
        self.use_db = False
        self.db = PersonDbDao()
        self.cache = PersonCacheDao()

    def get(self, key):
        value = None
        if self.use_db:
            value = self.db.get(key)
        else:
            value = self.cache.get(key)
            if value is None:
                value = self.db.get(key)
                if not self.cache.put(key, value):
                    logging.error('Cache add failed.')
        return value

```

## Cache Miss

If entry is not found in cache then:

1. Fetch entry from database
1. Put entry into cache
1. Return result

```python
from google.appengine.api import memcache

def get(self, key):
    value = memcache.get(key)

    if value is None:
        value = get_value_from_db(key)
            if not memcache.add(key, value):
                logging.error('Memcache add failed.')

    return value
```
