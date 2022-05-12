# Meetup Dashboard Queries (12/5/2022)
`demo.neo4jlabs.com` --> stackoverflow database.


## Queries

### Page 1

#### Start with a schema
```
CALL db.schema.visualization()
```

#### Fast stats:
```
CALL db.labels() YIELD label
CALL apoc.cypher.run('MATCH (:'+label+') RETURN count(*) as count',{}) YIELD value
RETURN label, value.count
```

```
CALL db.relationshipTypes() YIELD relationshipType as type
CALL apoc.cypher.run('MATCH ()-[:'+type+']->() RETURN count(*) as count',{}) YIELD value
RETURN type, value.count
```



#### Top 100 users:

MATCH (u:User)
RETURN u.name as Name, u.reputation as Rep
ORDER BY Rep DESC
LIMIT 100



#### Tag similarity tree:
```
MATCH path=(n:Tag{name:"html"})-[:SIMILAR*1..3]->(x)
WITH  nodes(path) as no
WITH no, last(no) as leaf
WITH  [n IN no[..-1] | n.name] AS result, sum(leaf.count) as val
UNWIND result as res
RETURN collect(DISTINCT res) as result, val
```

### PAGE 2 


#### a question and its answers:

MATCH path=(q:Post)--(a:Post)--()
WHERE q.id = “129178”
RETURN path

#### Posts by a user

MATCH path=(u:User)-[:POSTED]-(:Post)-[:ACCEPTED]->(:Post)<-[:POSTED]-(u2:User)
WHERE u.name = "max54"
RETURN path


