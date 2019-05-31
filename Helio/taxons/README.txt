Q1 : numero de instancias por tipo

SELECT DISTINCT ?Concept (count(distinct ?subject) as ?count){
 ?subject a ?Concept .
} GROUP BY ?Concept

Q2 : taxons in wikidata that have a different name from the one given in the csv, thus not found and enhanced

SELECT DISTINCT ?subject ?name {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
  filter not exists { 
    ?subject <http://www.w3.org/2002/07/owl#sameAs> ?link .
  }
}


Q3.1 : Nombre de especie y lugar donde puede encontrarse en gps


SELECT DISTINCT ?name ?lat ?long {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .
 ?measure <http://www.oeg.com/ontology#latitude> ?lat .
 ?measure <http://www.oeg.com/ontology#longitude> ?long .
 ?measure ?p ?o .
}

Q3.2 : Medidas por especie encontradas en fuentes de datos (muestra la falta de homogeindad de los datos)


SELECT DISTINCT ?p (count(distinct ?subject) as ?count) {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .

 ?measure ?p ?countries .
} GROUP BY ?p ORDER BY ?count



Q4 : Body mass de los animales + el tamaño (g)

SELECT DISTINCT ?name ?mass ?tamano {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#tamaño_adulto_g> ?tamano .
 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .

 ?measure <http://www.oeg.com/ontology#body_mass> ?mass .
}

Q5 : Camadas 


SELECT DISTINCT ?name ?tamaño_camada ?camada_x_año ?litters_x_year ?litter_size {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#tamaño_camada> ?tamaño_camada .
 ?subject <http://www.oeg.com/ontology#camada_por_año> ?camada_x_año .

 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .
 ?measure <http://www.oeg.com/ontology#litters_per_year> ?litters_x_year .
 ?measure <http://www.oeg.com/ontology#clutch/brood/litter_size> ?litter_size .
}


Q6 : Abitats 

SELECT DISTINCT ?name ?habitat {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .
 ?measure <http://www.oeg.com/ontology#habitat_includes> ?habitat .
}


Q6 : Places where they can be found 

SELECT DISTINCT ?name ?place {
 ?subject a <http://dbpedia.org/ontology/Species> .
 ?subject <http://www.oeg.com/ontology#nombre_especie> ?name .
 ?subject <http://www.oeg.com/ontology#hasMeasure> ?measure .
 ?measure <http://www.oeg.com/ontology#geographic_distribution_includes> ?place .
}


