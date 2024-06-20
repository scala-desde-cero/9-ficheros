# 9-ficheros


1. [Leer Ficheros de texto](#schema1)
2. [Escribir ficheros de texto](#schema2)
3. [Escribir y leer ficheros binarios](#schema3)

<hr>

<a name="schema1"></a>

## 1. Leer Ficheros de texto

### **1. Usando scala.io.Source**
La forma más sencilla de leer un archivo de texto en Scala es utilizando `scala.io.Source`. Este método proporciona funciones para leer líneas de un archivo de texto de manera eficiente.

```scala
import scala.io.Source

object LeerArchivo {
  def main(args: Array[String]): Unit = {
    // Ruta al archivo de texto
    val rutaArchivo = "ruta/a/tu/archivo.txt"

    // Intentar abrir el archivo usando Source.fromFile
    val archivo = Source.fromFile(rutaArchivo)

    try {
      // Leer todas las líneas del archivo
      val lineas = archivo.getLines().toList

      // Imprimir cada línea del archivo
      lineas.foreach(println)
    } catch {
      case e: Exception => println(s"Error al leer el archivo: ${e.getMessage}")
    } finally {
      // Cerrar el archivo
      archivo.close()
    }
  }
}

```
En este ejemplo:

- `Source.fromFile(rutaArchivo)`: Abre el archivo en la ruta especificada.
- `archivo.getLines().toList`: Lee todas las líneas del archivo y las convierte en una lista de cadenas.
- `foreach(println)`: Itera sobre cada línea y la imprime en la consola.
### **2. Manejo de Excepciones**
Es importante manejar adecuadamente las excepciones al trabajar con archivos, ya que pueden surgir errores como archivos no encontrados o problemas de acceso.

- El bloque `try {...} catch {...} finally {...}` se utiliza para manejar posibles excepciones que puedan ocurrir al leer o cerrar el archivo.
### **3. Otras Consideraciones**
- Recursos Cerrados: Es crucial cerrar el recurso `(archivo.close())` después de terminar de usarlo para liberar recursos del sistema.
- Rutas Relativas y Absolutas: Asegúrate de proporcionar la ruta correcta al archivo, ya sea relativa al directorio de trabajo actual o absoluta.
### **Alternativas Avanzadas**
- APIs de Librerías Externas: Scala también es compatible con diversas bibliotecas de terceros que proporcionan funciones avanzadas para leer y manipular archivos, como Apache Commons IO.
- Streams y Características de Scala: Para operaciones más complejas o manipulación avanzada de archivos, Scala ofrece características potentes como `java.io.InputStream y java.io.Reader`.

Al seguir estos principios básicos, podrás leer archivos de texto de manera efectiva y segura en Scala para tus aplicaciones.

<hr>

<a name="schema2"></a>

## 2. Escribir ficheros de texto

### **1. Usando java.io.FileWriter**
La forma más directa de escribir en un archivo de texto en Scala es utilizando `java.io.FileWriter`. Este método te permite escribir texto en un archivo de manera secuencial.


```scala
import java.io.FileWriter
import java.io.BufferedWriter
import java.io.File
import java.io.IOException

object EscribirArchivo {
  def main(args: Array[String]): Unit = {
    val rutaArchivo = "ruta/a/tu/archivo.txt"
    val contenido = "Hola, mundo!\nEste es un ejemplo de escritura en archivo."

    // Crear un FileWriter y un BufferedWriter
    var archivoEscritor: FileWriter = null
    var bufferEscritor: BufferedWriter = null

    try {
      // Crear el archivo si no existe
      val archivo = new File(rutaArchivo)
      if (!archivo.exists()) {
        archivo.createNewFile()
      }

      // Inicializar el FileWriter y BufferedWriter
      archivoEscritor = new FileWriter(archivo)
      bufferEscritor = new BufferedWriter(archivoEscritor)

      // Escribir el contenido en el archivo
      bufferEscritor.write(contenido)

      println("Archivo escrito exitosamente.")
    } catch {
      case e: IOException => println(s"Error al escribir en el archivo: ${e.getMessage}")
    } finally {
      // Cerrar el BufferedWriter y FileWriter
      if (bufferEscritor != null) bufferEscritor.close()
      if (archivoEscritor != null) archivoEscritor.close()
    }
  }
}
```
En este ejemplo:

- Se define la ruta del archivo (rutaArchivo) y el contenido que se desea escribir (contenido).
- Se utiliza java.io.FileWriter y java.io.BufferedWriter para escribir el contenido en el archivo.
- Se manejan excepciones usando try {...} catch {...} finally {...} para asegurarse de cerrar correctamente los recursos y manejar posibles errores de E/S.
### **2. Consideraciones Adicionales**
- Flujo de Escritura: El flujo de escritura (FileWriter y BufferedWriter) debe ser cerrado explícitamente después de usarlo para liberar recursos del sistema.
- Manejo de Excepciones: Es crucial manejar adecuadamente las excepciones que puedan ocurrir durante la escritura del archivo para evitar problemas de pérdida de datos o recursos no liberados.
- Codificación de Caracteres: Si necesitas manejar caracteres especiales o diferentes codificaciones, considera utilizar java.nio.charset.Charset para especificar la codificación adecuada al escribir.
### **Alternativas Avanzadas**
- APIs de Librerías Externas: Scala también es compatible con diversas bibliotecas de terceros que proporcionan funciones avanzadas para la escritura y manipulación de archivos, como Apache Commons IO.
- Streams y Características de Scala: Para operaciones más complejas o manipulación avanzada de archivos, Scala ofrece características potentes como java.io.OutputStream y java.io.Writer.


<hr>

<a name="schema3"></a>

## 3. Escribir y leer ficheros binarios

Para escribir datos binarios en un archivo en Scala, generalmente se sigue un enfoque similar al de los archivos de texto, pero se utilizan flujos de salida específicos para datos binarios como `java.io.FileOutputStream` y `java.io.DataOutputStream`.


```scala
import java.io.FileOutputStream
import java.io.DataOutputStream
import java.io.File
import java.io.IOException

object EscribirArchivoBinario {
  def main(args: Array[String]): Unit = {
    val rutaArchivo = "ruta/a/tu/archivo.bin"

    // Datos binarios de ejemplo (números enteros)
    val datosBinarios = Array(1, 2, 3, 4, 5)

    var archivoEscritor: FileOutputStream = null
    var dataEscritor: DataOutputStream = null

    try {
      // Crear el archivo binario si no existe
      val archivo = new File(rutaArchivo)
      if (!archivo.exists()) {
        archivo.createNewFile()
      }

      // Inicializar el FileOutputStream y DataOutputStream
      archivoEscritor = new FileOutputStream(archivo)
      dataEscritor = new DataOutputStream(archivoEscritor)

      // Escribir los datos binarios en el archivo
      datosBinarios.foreach(dataEscritor.writeInt)

      println("Archivo binario escrito exitosamente.")
    } catch {
      case e: IOException => println(s"Error al escribir en el archivo binario: ${e.getMessage}")
    } finally {
      // Cerrar el DataOutputStream y FileOutputStream
      if (dataEscritor != null) dataEscritor.close()
      if (archivoEscritor != null) archivoEscritor.close()
    }
  }
}
```

- Se define la ruta del archivo binario (rutaArchivo) y los datos binarios de ejemplo (datosBinarios).
- Se utiliza `java.io.FileOutputStream y java.io.DataOutputStream` para escribir datos binarios en el archivo.
- Se manejan excepciones usando try {...} catch {...} finally {...} para asegurarse de cerrar correctamente los recursos y manejar posibles errores de E/S.
### **Lectura de Archivos Binarios en Scala**
Para leer datos binarios desde un archivo en Scala, se utiliza `java.io.FileInputStream y java.io.DataInputStream` para manejar flujos de entrada de datos binarios.

```scala
import java.io.FileInputStream
import java.io.DataInputStream
import java.io.File
import java.io.IOException

object LeerArchivoBinario {
  def main(args: Array[String]): Unit = {
    val rutaArchivo = "ruta/a/tu/archivo.bin"

    var archivoLector: FileInputStream = null
    var dataLector: DataInputStream = null

    try {
      // Abrir el archivo binario
      archivoLector = new FileInputStream(rutaArchivo)
      dataLector = new DataInputStream(archivoLector)

      // Leer datos binarios del archivo
      while (dataLector.available() > 0) {
        println(dataLector.readInt())
      }
    } catch {
      case e: IOException => println(s"Error al leer el archivo binario: ${e.getMessage}")
    } finally {
      // Cerrar el DataInputStream y FileInputStream
      if (dataLector != null) dataLector.close()
      if (archivoLector != null) archivoLector.close()
    }
  }
}
```

- Se define la ruta del archivo binario (rutaArchivo) desde donde se leerán los datos binarios.
- Se utiliza java.io.FileInputStream y java.io.DataInputStream para leer datos binarios del archivo.
- Se manejan excepciones usando try {...} catch {...} finally {...} para asegurarse de cerrar correctamente los recursos y manejar posibles errores de E/S.
### **Consideraciones Adicionales**
- Manejo de Excepciones: Es fundamental manejar adecuadamente las excepciones que puedan ocurrir durante la lectura o escritura de archivos binarios para evitar problemas de pérdida de datos o recursos no liberados.
- Codificación de Datos: A diferencia de los archivos de texto que usan codificaciones como UTF-8, los archivos binarios manejan datos en su representación binaria original, por lo que no hay preocupaciones por codificaciones de caracteres.
- Performance: Los archivos binarios son ideales para almacenar datos estructurados y complejos de manera eficiente en términos de espacio y rendimiento.