# **_Capitulo 2_**

## Introducción a Spring Boot

En este capítulo, se te presentará cómo construir un conjunto de microservicios cooperativos usando Spring Boot, centrándote en cómo desarrollar funcionalidad que entregue valor de negocio. Los desafíos con los microservicios que señalamos en el capítulo anterior se considerarán solo hasta cierto punto, pero se abordarán en su totalidad en capítulos posteriores.

Desarrollaremos microservicios que contengan lógica de negocio basada en beans simples de Spring y expondrán APIs REST usando Spring WebFlux. Las APIs se documentarán basándose en la Especificación OpenAPI usando springdoc-openapi. Para hacer persistentes los datos procesados por los microservicios, usaremos Spring Data para almacenar datos tanto en bases de datos SQL como NoSQL.

Desde que Spring Boot v2.0 fue lanzado en marzo de 2018, se ha vuelto mucho más fácil desarrollar microservicios reactivos, incluyendo APIs REST síncronas no bloqueantes. Para desarrollar servicios asíncronos basados en mensajes, usaremos Spring Cloud Stream. Consulta el Capítulo 1, Introducción a los Microservicios, la sección Microservicios Reactivos, para más información.

Spring Boot 3.0 fue lanzado en noviembre de 2022. Está basado en Spring Framework 6.0 y Jakarta EE 9 y es compatible con Jakarta EE 10. Java 17, la versión de soporte a largo plazo (LTS) actual, se requiere como versión mínima de Java. Se han lanzado cinco versiones menores, 3.1–3.5, desde Spring Boot 3.0. Este libro está basado en Spring Boot 3.5, que fue lanzado en mayo de 2025.

Finalmente, usaremos Docker para ejecutar nuestros microservicios como contenedores. Esto nos permitirá iniciar y detener nuestro paisaje de microservicios, incluyendo servidores de base de datos y un broker de mensajes, con un solo comando.

Son muchas tecnologías y frameworks, así que repasemos cada uno brevemente para ver de qué se tratan. En este capítulo, presentaremos los siguientes proyectos de código abierto:

- Spring Boot – esta sección también incluye una visión general de lo nuevo en las versiones 3.0–3.5 y cómo migrar aplicaciones v2
- Spring WebFlux
- springdoc-openapi
- Spring Data
- Spring Cloud Stream
- Docker

### Requisitos técnicos

Este capítulo no contiene ningún código fuente que pueda descargarse, ni requiere que se instale ninguna herramienta.

## Spring Boot

Spring Boot, y el Spring Framework en el que se basa Spring Boot, es un gran framework para desarrollar microservicios en Java.
Cuando Spring Framework v1.0 fue lanzado en 2004, uno de sus objetivos principales era abordar el estándar J2EE excesivamente complejo (abreviatura de Java 2 Platform, Enterprise Edition) con sus infames y pesados descriptores de despliegue. Spring Framework proporcionó un modelo de desarrollo mucho más ligero basado en el concepto de inyección de dependencias (dependency injection). Spring Framework también usaba archivos de configuración XML mucho más ligeros en comparación con los descriptores de despliegue en J2EE. En 2006, J2EE fue renombrado a Java EE, abreviatura de Java Platform, Enterprise Edition. En 2017, Oracle entregó Java EE a la Eclipse Foundation. En febrero de 2018, Java EE fue renombrado a Jakarta EE.

El nuevo nombre, Jakarta EE, también afecta los nombres de los paquetes Java definidos por el estándar, requiriendo que los desarrolladores realicen un renombrado de paquetes al actualizar a Jakarta EE, como se describe en la sección Migración de una aplicación Spring Boot 2 más adelante en este capítulo. A lo largo de los años, mientras Spring Framework ganaba creciente popularidad, la funcionalidad en Spring Framework creció significativamente. Lentamente, la carga de configurar una aplicación Spring usando el archivo de configuración XML ya no tan ligero se convirtió en un problema.

En 2014, Spring Boot v1.0 fue lanzado, ¡abordando estos problemas!

### Convención sobre configuración y archivos JAR gordos (fat JAR)

Spring Boot apunta al desarrollo rápido de aplicaciones Spring listas para producción al ser fuertemente opinativo sobre cómo configurar tanto los módulos principales de Spring Framework como productos de terceros, tales como librerías que se usan para logging o para conectarse a una base de datos. Spring Boot hace eso aplicando una serie de convenciones por defecto, minimizando la necesidad de configuración.

Siempre que sea necesario, cada convención puede ser anulada escribiendo algo de configuración, caso por caso. Este patrón de diseño se conoce como convención sobre configuración (convention over configuration) y minimiza la necesidad de configuración inicial.

`La configuración, cuando es necesaria, en mi opinión, se escribe mejor usando Java y anotaciones. Los buenos y viejos archivos de configuración basados en XML todavía se pueden usar, aunque son significativamente más pequeños que antes de que se introdujera Spring Boot.`

Sumado al uso de convención sobre configuración, Spring Boot también favorece un modelo de ejecución basado en un archivo JAR independiente (standalone JAR), también conocido como archivo JAR gordo (fat JAR). Antes de Spring Boot, la forma más común de ejecutar una aplicación Spring era desplegarla como un archivo WAR en un servidor web Java EE, como Apache Tomcat. El despliegue de archivos WAR todavía es compatible con Spring Boot.

`Un archivo JAR gordo (fat JAR) contiene no solo las clases y archivos de recursos de la aplicación en sí, sino también todos los archivos JAR de los que depende la aplicación. Esto significa que el archivo JAR gordo es el único archivo JAR requerido para ejecutar la aplicación; es decir, solo necesitamos transferir un archivo JAR a un entorno donde queramos ejecutar la aplicación, en lugar de transferir el archivo JAR de la aplicación junto con todos los archivos JAR de los que la aplicación depende.`

Iniciar un JAR gordo no requiere un servidor web Java EE instalado por separado, como Apache Tomcat. En su lugar, se puede iniciar con un comando simple como java -jar app.jar, lo que lo convierte en una opción perfecta para ejecutar en un contenedor Docker. Si la aplicación Spring Boot, por ejemplo, usa HTTP para exponer una API REST, también contendrá un servidor web embebido.
Ejemplos de código para configurar una aplicación Spring Boot
Para entender mejor lo que esto significa, veamos algunos ejemplos de código fuente.

`Solo veremos algunos pequeños fragmentos de código aquí para señalar las características principales. Para un ejemplo completamente funcional, ¡tendrás que esperar hasta el próximo capítulo!`

### La mágica anotación @SpringBootApplication

El mecanismo de autoconfiguración basado en convenciones se puede iniciar anotando la clase de la aplicación (es decir, la clase que contiene el método main estático) con la anotación @SpringBootApplication.
El siguiente código muestra esto:

```java@SpringBootApplication
public class MyApplication {
  public static void main(String[] args) {
    SpringApplication.run(MyApplication.class, args);
  }
}
```

`Consejo rápido: Mejora tu experiencia de codificación con las funciones de Explicador de Código con IA y Copiado Rápido. Abre este libro en el Packt Reader de última generación. Haz clic en el botón Copiar (1) para copiar rápidamente el código en tu entorno de codificación, o haz clic en el botón Explicar (2) para que el asistente de IA te explique un bloque de código.`

La siguiente funcionalidad será proporcionada por esta anotación:

- Habilita el escaneo de componentes (component scanning), es decir, buscar componentes Spring y clases de configuración en el paquete de la clase de la aplicación y todos sus subpaquetes.
- La clase de la aplicación en sí misma se convierte en una clase de configuración.
- Habilita la autoconfiguración, donde Spring Boot busca archivos JAR en el classpath que pueda configurar automáticamente. Por ejemplo, si tienes Tomcat en el classpath, Spring Boot configurará automáticamente Tomcat como un servidor web embebido.

### Escaneo de componentes (Component scanning)

Supongamos que tenemos el siguiente componente Spring en el paquete de la clase de la aplicación (o en uno de sus subpaquetes):

```java
@Component
public class MyComponentImpl implements MyComponent {}
```

Otro componente en la aplicación puede obtener este componente inyectado automáticamente, también conocido como auto-wiring (cableado automático), usando la anotación @Autowired:

```javapublic class AnotherComponent {
private final MyComponent myComponent;
@Autowired
public AnotherComponent(MyComponent myComponent) {
this.myComponent = myComponent;
}

```

```

```
