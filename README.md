# TDSE_Parcial-practico

#### Enunciado

Diseñe, construya y desplegue una aplicación web para investigar el problema matemático asignado. El programa debe estar desplegado en AWS. Las tecnologías usadas en la solución deben ser Maven, Git, GitHub, Spring, HTML5 y js. No use liberías adicionales.

### Problema matematico asignado:

#### Fibonacci con ventana (suma móvil K=3)
Su servicio matemático debe incluir una función:

fibwin(n) retorna un JSON con la serie de Fibonacci desde F_0 hasta F_n y la suma móvil de ventana K=3 aplicada a esa serie. (Recibe enteros n ≥ 2).
Definición
Fibonacci: F_0=0, F_1=1, F_n=F_{n-1}+F_{n-2} para n ≥ 2.
Ventana K=3: para la lista [a0, a1, …, am], la suma móvil es [a0+a1+a2, a1+a2+a3, …].

Implementación requerida
(1) Genere Fibonacci de forma iterativa. (2) Calcule la suma móvil recorriendo la lista y sumando tramos contiguos de longitud 3. No use funciones de librería que implementen rolling windows.

Detalles de API

Responda a HTTP GET. Nombre del endpoint: /fibwin. Parámetro en query: value.

Ejemplo de llamado (EC2)

https://amazonxxx.x.xxx.x.xxx:{port}/fibwin?value=10

Salida (JSON)

{
"operation": "Fibonacci con ventana K=3",
"input": 10,
"output": "serie: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55 | ventana: 2, 4, 6, 10, 16, 26, 42, 68, 110"

Documentación autorizada:
* https://docs.oracle.com/javase/8/docs/api/
* https://spring.io/guides/gs/rest-service
* https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/amazon-linux-install.html
* AWS Academy
* AWS

### Problema:

Diseñe un prototipo de sistema de microservicios que tenga un servicio (En la figura se representa con el nombre Math Services) para computar las funciones numéricas.  El servicio de las funciones numéricas debe estar desplegado en al menos dos instancias virtuales de EC2. Adicionalmente, debe implementar un service proxy que reciba las solicitudes de llamado desde los clientes  y se las delegue a las dos instancias del servicio numérico usando un algoritmo de round-robin. El proxy deberá estar desplegado en otra máquina EC2. Asegúrese de poder configurar las direcciones y puertos de las instancias del servicio en el proxy usando variables de entorno del sistema operativo.  Finalmente, construya un cliente Web mínimo con un formulario que reciba el valor y de manera asíncrona invoke el servicio en el PROXY. Puede hacer un formulario para cada una de las funciones. El cliente debe ser escrito en HTML y JS.

![Screenshot 2026-03-24 091305.png](imag%2FScreenshot%202026-03-24%20091305.png)

#### Detalles adicionales de la arquitectura y del API
Construya una aplicación web para investigar este problema. La aplicación debe tener esta arquitectura:
* Cliente asíncrono que corra en el browser escrito en HTML5 y JS (No use librerías, solo html JS básico).
* El cliente NO COMPUTA LA SECUENCIA DIRECTAMENTE, sino que la delega a un servicio REST corriendo en AWS.
* El servicio REST puede ser GET o POST.
* Se debe construir la aplicación usando Spring Boot y desplegarla en un contenedor corriendo en AWS.
* Cree un solo repositorio en github para toda la aplicación
* Use los mejores estándares de diseño, arquitectura y programación.

### Entregable:
1. Proyecto actualizado en github (un solo repositorio)
2. Descripción del proyecto en el README con pantallazos que muestren el funcionamiento.
3. Descripción de cómo correrlo en EC2.
4. Video de menos de un minuto del funcionamiento (lo puede tomar con el celular una vez funcione)

En el campo de texto escriba la dirección de su repositorio GITHUB.

### Ayuda
Para invocar un servicios get desde java puede hacerlo de manera fácil con:

import java.io.BufferedReader;

import java.io.IOException;

import java.io.InputStreamReader;

import java.io.OutputStream;

import java.net.HttpURLConnection;

import java.net.URL;

public class HttpConnectionExample {

    private static final String USER_AGENT = "Mozilla/5.0";
    private static final String GET_URL = "https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=fb&apikey=Q1QZFVJQ21K7C6XM";

    public static void main(String[] args) throws IOException {

        URL obj = new URL(GET_URL);
        HttpURLConnection con = (HttpURLConnection) obj.openConnection();
        con.setRequestMethod("GET");
        con.setRequestProperty("User-Agent", USER_AGENT);
        
        //The following invocation perform the connection implicitly before getting the code
        int responseCode = con.getResponseCode();
        System.out.println("GET Response Code :: " + responseCode);
        
        if (responseCode == HttpURLConnection.HTTP_OK) { // success
            BufferedReader in = new BufferedReader(new InputStreamReader(
                    con.getInputStream()));
            String inputLine;
            StringBuffer response = new StringBuffer();

            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();

            // print result
            System.out.println(response.toString());
        } else {
            System.out.println("GET request not worked");
        }
        System.out.println("GET DONE");
    }

} 

2. Para invocar servicios rest de forma asíncrona desde un cliente JS

````dtd
<!DOCTYPE html>
<html>
<head>
<title>Form Example</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
<h1>Form with GET</h1>
<form action="/hello">
<label for="name">Name:</label><br>
<input type="text" id="name" name="name" value="John"><br><br>
<input type="button" value="Submit" onclick="loadGetMsg()">
</form>
<div id="getrespmsg"></div>
<script>
function loadGetMsg() {
let nameVar = document.getElementById("name").value;
const xhttp = new XMLHttpRequest();
xhttp.onload = function() {
document.getElementById("getrespmsg").innerHTML =
this.responseText;
}
xhttp.open("GET", "/hello?name="+nameVar);
xhttp.send();
}
</script>
<h1>Form with POST</h1>
<form action="/hellopost">
<label for="postname">Name:</label><br>
<input type="text" id="postname" name="name" value="John"><br><br>
<input type="button" value="Submit" onclick="loadPostMsg(postname)">
</form>
<div id="postrespmsg"></div>
<script>
function loadPostMsg(name){
let url = "/hellopost?name=" + name.value;
fetch (url, {method: 'POST'})
.then(x => x.text())
.then(y => document.getElementById("postrespmsg").innerHTML = y);
}
</script>
</body>
</html>
3. Aplicación Spring mínima

-------
package co.edu.eci.lambda.springrest;

import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

private static final String template = "Hello, %s!";
private final AtomicLong counter = new AtomicLong();

@GetMapping("/greeting")
public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
return new Greeting(counter.incrementAndGet(), String.format(template, name));
}
}
-------
package co.edu.eci.lambda.springrest;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestServiceApplication {

public static void main(String[] args) {
SpringApplication.run(RestServiceApplication.class, args);
}

}
---------
package co.edu.eci.lambda.springrest;


public record Greeting(long id, String content) { }

    4. Ejemplo pom.xml de Spring con plugins
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>3.3.0</version>
<relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>com.example</groupId>
<artifactId>rest-service-complete</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>rest-service-complete</name>
<description>Demo project for Spring Boot</description>
<properties>
<java.version>17</java.version>
</properties>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
</dependency>
</dependencies>

<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>

</project>
````