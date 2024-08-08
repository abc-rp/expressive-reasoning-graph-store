This Readme contains instructions and step to reproduce experiments number. Here we have explained steps for getting LUBM 1000 university dataset, similar steps can be followed for getting experiments number for other datasets also.
### Machine Configuration:
Experiments are performed on RHEL machine with 128 GB RAM, 16 cores and 1TB HDD.

### Steps:
1. Download LUBM data generator from http://swat.cse.lehigh.edu/projects/lubm/uba1.7.zip and generate 1000 university data.
2. Setup ERGS system using mavan build as provided in main [README](https://github.com/IBM/expressive-reasoning-graph-store/blob/master/README.md) file and create new ERGS repository
3. Modify [BulkLoader.java](https://github.com/IBM/expressive-reasoning-graph-store/blob/master/IngestionPipeline/GraphIngestion/src/main/java/com/ibm/research/ergs/ingestion/loader/BulkLoader.java) on [Line 74 and 75](https://github.com/IBM/expressive-reasoning-graph-store/blob/eee973c353cd05de842c376a0dda95dcc2396f76/IngestionPipeline/GraphIngestion/src/main/java/com/ibm/research/ergs/ingestion/loader/BulkLoader.java#L74) with appropriate storage backend parameters:
    ```
    configuration.addProperty("storage.backend", "hbase");
    configuration.addProperty("storage.hostname", "127.0.0.1");

    for hbase backend
    ```
    ```
    configuration.addProperty("storage.backend", "cql");
    configuration.addProperty("storage.hostname", "127.0.0.1");

    for cassandra backend
    ```

4. Load RDF data into JanusGraph with modified BulkLoader.java with following arguments:
    ```
    <repository_id> <dataset_directory> <dataset_format> <base_uri> <properties_file>
    ```
    Where:
    * repository_id: `<ID of repository created through GUI.>`
    * dataset_directory: `<directory of LUBM 1000 university data files.>`
    * dataset_format: "rdf"
    * base_uri: "http://swat.cse.lehigh.edu/onto/univ-bench.owl"
    * properties_file: [docker/bulkloading/bulkload.properties](https://github.com/IBM/expressive-reasoning-graph-store/blob/master/docker/bulkloading/bulkload.properties)


4. After data loading use [ComputeTime.java](https://github.com/IBM/expressive-reasoning-graph-store/blob/master/RDF4J/rdf4j-repository/src/test/java/com/ibm/research/ergs/rdf4j/repository/ComputeTime.java) file to generate execution time for each [query](https://github.com/IBM/expressive-reasoning-graph-store/blob/master/RDF4J/rdf4j-repository/src/test/resources/lubm/queries.txt) with following arguments.
    ```
     <repository_id> <query>
    ```
     Where:
    * repository_id: `<ID of repository created through GUI.>`
    * query: `<SPARQL query within " ".>`
