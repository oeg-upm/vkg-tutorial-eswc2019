# Morph-CSV
## How to enable OBDA query-translation over real CSVs?

Use CSVW annotations and RML FnO mappings to generate R2RML mappings and an enriched RDB to enable OBDA query translation over real CSV files. This framework can be embedded in the top of any R2RML-compliant engine.

- Morph-CSV source code: https://github.com/oeg-upm/morph-csv
- Docker image: https://hub.docker.com/r/dchaves1/morph-csv


## How to run it?

```bash
git clone https://github.com/oeg-upm/vkg-tutorial-eswc2019
cd vkg-tutorial-eswc2019/morph-csv
docker-compose up -d
cd run-scripts
./run-XXX
```


## Examples:

- Comments and persons: https://github.com/oeg-upm/morph-csv/tree/master/evaluation/motivating-example
- Linking Open City data: https://github.com/oeg-upm/morph-csv/tree/master/evaluation/open-city-data-validation
- Virtual Bio2RDF: https://github.com/oeg-upm/morph-csv/tree/master/evaluation/bio2rdf
- Performance over transport data (GTFS): https://github.com/oeg-upm/morph-csv/tree/master/evaluation/transport-performance
