# morph-GraphQL
Translate OBDA mappings (R2RML/RML) into GraphQL Resolvers

## EXAMPLE Starwars: Translating mappings online for Javascript and a set of CSV files (assuming that you have npm and node or docker installed)
### The mappings used in the examples
- url: https://raw.githubusercontent.com/fpriyatna/mapping-translator/master/examples/starwars/mappings6.ttl


1. ```mkdir output```
2. ```cd output```
3. Translate the corresponding RML: 
   ```curl -X POST http://mappingtranslator.mappingpedia.linkeddata.es/transform -H 'Content-Type: application/json' -d '{ "prog_lang": "javascript", "dataset_type":"csv", "mapping_url":"https://raw.githubusercontent.com/fpriyatna/mapping-translator/master/examples/starwars/mappings6.ttl", "db_name":"starwars6.sqlite", "mapping_language":"rml", "queryplanner":"joinmonster" }' > output.zip```
5. ```unzip output.zip```
6. ```npm install```
7. ```npm start```
9. Go to http://localhost:4321/graphql from your browser, use some of the queries below

### To query the hero in every episode
```
{
  listHeroes {
    episode {
      identifier
      code
    }
    hero {
      identifier
      name
    }
  }
}
```

### to query for the ID and friends of R2-D2
```
{
  listCharacter(name: "R2 D2") {
    identifier
    name
    friends {
      identifier
      charid
      friendId
    }
  }
}
```

### to query for Luke Skywalker directly, using his ID
```
{
  listCharacter(identifier: "http://starwars.mappingpedia.linkeddata.es/character/1000") {
    name
  }
}
```

### to query for both Luke and Leia
```
query FetchLukeAndLeiaAliased {
  luke: listCharacter(identifier: "http://starwars.mappingpedia.linkeddata.es/character/1000") {
    name
  }
  leia: listCharacter(identifier: "http://starwars.mappingpedia.linkeddata.es/character/1003") {
    name
  }
}
```

### to verify that R2-D2 is a droid
```
{
  listCharacter(name: "R2 D2") {
    identifier
    name
    type(name: "Droid") {
      identifier
      name
    }
  }
}
```

### to verify that the hero of episode Empire is a human
```
{
  listHeroes {
    identifier
    hero {
      identifier
      name
      type(name: "Human") {
        identifier
        name
      }
    }
    episode(code: "Empire") {
      identifier
      code
    }
  }
}
```
