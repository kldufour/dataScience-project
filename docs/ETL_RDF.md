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
val df = spark.read.textFile("/FileStore/tables/cipUCDfev23.nt").rdd
//val dataframe = df.map(l => l.split(" ")).map(l => (l(0), (l(1), l(2)))).toDF("subject", "propeties")
//val dataframe = df.map(l => l.split(" ")).map(l => (l(0), (l(1), l(2)))).reduce

//df.map(l => l.split(" ")).map(l => (l(0), (l(1), l(2)))).reduceByKey{ (m1, m2) => println(m2) }


/*
val dataframeProps = df.map(l => l.split(" ")).map(l => (l(1))).distinct.toDF("properties")
val dataframeSubjects = df.map(l => l.split(" ")).map(l => (l(0))).distinct.toDF("subjects")

dataframe.createOrReplaceTempView("db")
dataframeProps.createOrReplaceTempView("dbProps")
dataframeSubjects.createOrReplaceTempView("dbSubjects")

val res = spark.sql("SELECT subjects as CIP13, properties as PROPS, value as o FROM dbProps, dbSubjects LEFT OUTER JOIN db WHERE db.subject = dbSubjects.subjects AND db.property = dbProps.properties")
*/

```