# Extract Transform and Load (ETL)

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