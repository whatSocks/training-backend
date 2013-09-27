# Neo4j Training Backend
    
Heroku App: http://neo4j-training-backend.herokuapp.com


### Usage:

We use a session id generated by the front-end to control user sessions, it will usually be an UUID.

````
// execute query, reads or updates
curl -H X-Session:239739847 -XPOST http://neo4j-training-backend.herokuapp.com/backend/cypher -d'MATCH (n) RETURN n'

// initialize, is optional
curl -H X-Session:239739847 -XPOST http://neo4j-training-backend.herokuapp.com/backend/init \
 -d'{"init":"create (:User {name:\"Andreas\"}),(:User {name:\"Michael\"})","query":"MATCH (n:User) return n"}'

// delete, cleanup session
curl -H X-Session:239739847 -XDELETE http://neo4j-training-backend.herokuapp.com/backend

````

With session handling:
````
// store database setup with provided id using /backend/save
curl -H X-Session:239739847 -XPOST http://neo4j-training-backend.herokuapp.com/backend/graph \
 -d'{"id":"users-graph","init":"create (:User {name:\"Andreas\"}),(:User {name:\"Michael\"})"}'


curl -H X-Session:239739847 -XDELETE http://neo4j-training-backend.herokuapp.com/backend/graph/:id

curl -H X-Session:239739847 -XDELETE http://neo4j-training-backend.herokuapp.com/backend/graph/users-graph

// initialize with session-id, fallback to the provided setup-id
// returns also previously stored history
curl -H X-Session:239739847 -XPOST http://neo4j-training-backend.herokuapp.com/backend/init \
 -d'{"id":"users-graph"}'

// on shutdown/timeout changed database state and input history is saved to storage
// so on next init the modified state will be used as initial state and stored history will be returned
````


### Run locally:

````
mvn exec:java
````
