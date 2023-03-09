# Extract Transform and Load (ETL)


## File reformatting

file reformatting to interpret the accented characters

``` bash
iconv -f WINDOWS-1252 -t UTF-8 cisCip.txt > cisCip2.txt
```

## Databricks Link
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/5675750426854688/1634914331591828/3731440783393868/latest.html

## Script
```scala
import java.io.{BufferedWriter, File, FileOutputStream, FileWriter, OutputStreamWriter}
import java.text.Normalizer
import scala.io.Source

object Application {

  def main(args: Array[String]): Unit = {


    val inputFilePath = "/Users/gogo/Downloads/cisCip.txt"
    val outputFilePath = "/Users/gogo/Downloads/cisCip2.txt"

    val source = Source.fromFile(inputFilePath, "ISO-8859-1")
    val lines = source.getLines().toArray

    val writer = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(outputFilePath), "UTF-8"))

    for (line <- lines) {
      val normalizedLine = Normalizer.normalize(line, Normalizer.Form.NFD)
        .replaceAll("\\p{M}", "").replaceAll("[éèêë]", "e")
      writer.write(normalizedLine)
      writer.newLine()
    }
    writer.close()
  }
}
```
