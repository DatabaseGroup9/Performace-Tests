
#### N1

```PROFILE MATCH (a:Author)-[ra:AUTHORED]->(b:Book)-[r:MENTIONS ]-(c:City) WHERE LOWER(c.name) = LOWER("Bonao") RETURN a,b```

[![https://gyazo.com/b529b0411e495805d1ffc6ba309f2a69](https://i.gyazo.com/b529b0411e495805d1ffc6ba309f2a69.png)](https://gyazo.com/b529b0411e495805d1ffc6ba309f2a69)


```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 21713 total db hits in 174 ms.```

Adding index 

```CREATE INDEX ON :City(name)```
```CREATE CONSTRAINT ON (c:City) ASSERT c.name IS UNIQUE```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 21727 total db hits in 47 ms.```


#### N2

Query in Cypher:

```PROFILE MATCH (b:Book)-[r:MENTIONS]->(c:City) WHERE LOWER(b.bookTitle) = LOWER("Mother: A Story") RETURN c```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 66712 total db hits in 139 ms.```


```CREATE INDEX ON :Book(bookTitle)``` 

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 66712 total db hits in 100 ms.```


#### N3

```PROFILE MATCH (a:Author)-[ra:AUTHORED]->(b:Book)-[r:MENTIONS ]->(c:City) WHERE LOWER(a.fullName) = LOWER("Norris, Kathleen Thompson") RETURN a,b,collect(c)```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 85593 total db hits in 86 ms.```

```CREATE INDEX ON :Author(fulllName)```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 85593 total db hits in 152 ms.```

*strange*

#### N4

```PROFILE MATCH (a:Author)-[ra:AUTHORED]->(b:Book)-[r:MENTIONS ]->(c:City) WHERE distance(point({ x: c.lat, y: c.lon}), 
    point({x: 14.6042 ,y: 120.9822 })) < 0.35 RETURN a,b,collect(c) ORDER BY a.fullName```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 27121 total db hits in 37 ms.```

```CREATE INDEX ON :City(lan)
CREATE INDEX ON :City(lon)```

```Cypher version: CYPHER 3.4, planner: COST, runtime: INTERPRETED. 5432 total db hits in 44 ms.
```



