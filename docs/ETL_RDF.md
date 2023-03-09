# Extract Transform and Load (ETL)

## Convert RDF to NT

We use RDF2RDF to generate NT file.
You can found installation gide on [link](https://github.com/architolk/rdf2rdf)

After cloning and building you can excute the next command as root of project to generate `.nt` file

```bash
java -jar rdf2rdf/target/rdf2rdf-1.1.0-jar-with-dependencies.jar data/cipUCDfev23.rdf.xml data/cipUCDfev23.nt
```

Convert `.nt` file to dataframe
```scala

val dataframe = spark.read.textFile("/FileStore/tables/cipUCDfev23.nt").map(l => l.split(" ")).map(l => (l(0), l(1), l(2))).toDF("subject", "property", "value")
dataframe.createOrReplaceTempView("database")

val nom = spark.sql("SELECT subject, value as nom FROM database WHERE property LIKE '%rdf-schema#label%'").show(1)

val labo = spark.sql("SELECT subject, value as labo FROM database WHERE property LIKE '%titulaire%'").show(1)

val ephMRE = spark.sql("SELECT subject, value as ephMRE FROM database WHERE property LIKE '%ephMRA%'").show(1)

val ucd13 = spark.sql("""
SELECT d2.subject, d2.value as ucd13

FROM database as d1, database as d2

WHERE 
    d1.property LIKE '%type%' AND d1.value='%UCD%'
""").show(1)

```
