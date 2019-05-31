Enseñar el recurso http://localhost:8080/stars4all/photometers/stars231, luego ir al log y dejar ver que el tiempo para generar el RDF se debe al tiempo de respuesta del servidor:

2019-05-30 11:45:18.592  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 2429 ms
2019-05-30 11:45:18.598  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 2435 ms
2019-05-30 11:45:19.750  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 1149 ms
2019-05-30 11:45:19.755  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 1154 ms
2019-05-30 11:45:20.215  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 453 ms
2019-05-30 11:45:20.215  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 453 ms
2019-05-30 11:45:21.742  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 1307 ms
2019-05-30 11:45:21.743  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 1309 ms
2019-05-30 11:45:22.254  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 310 ms
2019-05-30 11:45:22.255  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 311 ms
2019-05-30 11:45:23.062  INFO 726 --- [nio-8080-exec-8] helio.components.connector.URLConnector  : Data retrieved trough URLConnector took: 580 ms
2019-05-30 11:45:23.063  INFO 726 --- [nio-8080-exec-8] h.components.datasource.JsonDatasource   : Data reading trough JsonDatasource took: 581 ms


Enseñar el recurso http://localhost:8080/stars4all/locations/cities/wittingen, explicar que este tarda super poco porque esta con refresh y no hace falta acceder al recurso remotamente




Queries:

- Enseñar que podemos combinar los valores de los tres enpoints (alertas, valor de los sensores, y su localizacion) para hacer queries como "dame los sensores con las ciudades donde estan". Ademas podríamos jugar preguntando por aquellos que solo estan activos

# Sensores con ciudades
SELECT DISTINCT ?sensorName ?value ?tstamp ?property ?locationName {

         ?sensor <https://w3id.org/saref#hasName> ?sensorName  .
        ?sensor <https://w3id.org/saref#makesMeasurement> ?measurement .
        ?measurement <https://w3id.org/saref#hasValue> ?value .
        ?measurement <https://w3id.org/saref#hasTimeStamp> ?tstamp .
  		?measurement <https://w3id.org/saref#relatesToProperty> ?property .
       
  		?sensor <http://schema.org/location> ?location .
     ?location <http://schema.org/name> ?locationName .
     ?location a <http://schema.org/City> .
  
} 

# SOLO Sensores activos con ciudades
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT DISTINCT ?sensorName ?value ?tstamp ?property ?locationName {

         ?sensor <https://w3id.org/saref#hasName> ?sensorName  .
        ?sensor <https://w3id.org/saref#makesMeasurement> ?measurement .
        ?measurement <https://w3id.org/saref#hasValue> ?value .
        ?measurement <https://w3id.org/saref#hasTimeStamp> ?tstamp .
  		?measurement <https://w3id.org/saref#relatesToProperty> ?property .
       
  		?sensor <http://schema.org/location> ?location .
     ?location <http://schema.org/name> ?locationName .
     ?location a <http://schema.org/City> .
      filter( xsd:double(?value) > 0) .
  
} 
    
- Enseñar que se combina el GDPNominal de wikidata y dbpedia (que por otro lado se podrían usar los distintos valorees para ajustes matematicos), y los sensores con sus valores

SELECT DISTINCT ?sensorName ?value ?tstamp ?property ?locationName ?gdpNominal ?gdpNominalPercapita {

         ?sensor <https://w3id.org/saref#hasName> ?sensorName  .
        ?sensor <https://w3id.org/saref#makesMeasurement> ?measurement .
        ?measurement <https://w3id.org/saref#hasValue> ?value .
        ?measurement <https://w3id.org/saref#hasTimeStamp> ?tstamp .
  		?measurement <https://w3id.org/saref#relatesToProperty> ?property .
       
  		?sensor <http://schema.org/location> ?location .
     ?location <http://dbpedia.org/property/gdpNominal> ?gdpNominal.
     ?location <http://dbpedia.org/property/gdpNominalPerCapita> ?gdpNominalPercapita .
     ?location <http://schema.org/name> ?locationName .
  
} 
    