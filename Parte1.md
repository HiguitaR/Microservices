# **_Parte_**

# Introducción al Desarrollo de Microservicios Usando Spring Boot

En esta primera parte del libro, aprenderás a usar algunas de las características más importantes de Spring Boot para desarrollar microservicios.

# Esta parte del libro incluye los siguientes capítulos:

-Capítulo 1, Introducción a los Microservicios

- Capítulo 2, Introducción a Spring Boot
- Capítulo 3, Creación de un Conjunto de Microservicios Cooperativos
- Capítulo 4, Despliegue de Nuestros Microservicios Usando Docker
- Capítulo 5, Adición de una Descripción de API Usando OpenAPI
- Capítulo 6, Adición de Persistencia
- Capítulo 7, Desarrollo de Microservicios Reactivos

## Capítulo 1: Introducción a los Microservicios

Este libro no alaba ciegamente los microservicios. En cambio, trata sobre cómo podemos usar sus beneficios mientras somos capaces de manejar los desafíos de construir microservicios escalables, resilientes y manejables.
Como introducción a este libro, los siguientes temas serán cubiertos en este capítulo:

- Mi camino hacia los microservicios
- ¿Qué es una arquitectura basada en microservicios?
- Desafíos con los microservicios
- Patrones de diseño para manejar desafíos
- Habilitadores de software que pueden ayudarnos a manejar estos desafíos
- Otras consideraciones importantes que no se cubren en este libro

### Requisitos técnicos

No se requieren instalaciones para este capítulo. Sin embargo, puede que te interese echar un vistazo a las convenciones del modelo C4, https://c4model.com, ya que las ilustraciones de este capítulo están inspiradas en el modelo C4.
Este capítulo no contiene ningún código fuente.

### Aprovechando al máximo este libro – conoce tus beneficios gratuitos

Desbloquea beneficios gratuitos exclusivos que vienen con tu compra, cuidadosamente diseñados para potenciar tu viaje de aprendizaje y ayudarte a aprender sin límites.

Aquí tienes una visión general rápida de lo que obtienes con este libro:

### Lector de última generación

Nuestro lector basado en web, diseñado para ayudarte a aprender efectivamente, viene con las siguientes características:

- Sincronización de progreso multidispositivo: Aprende desde cualquier dispositivo con sincronización de progreso sin interrupciones.
- Resaltado y toma de notas: Convierte tu lectura en conocimiento duradero.
- Marcadores: Revisita tus aprendizajes más importantes en cualquier momento.
- Modo oscuro: Concéntrate con mínima fatiga visual cambiando al modo oscuro o sepia.

### Asistente de IA interactivo (beta)

Nuestro asistente de IA interactivo ha sido entrenado con el contenido de este libro, para maximizar tu experiencia de aprendizaje. Viene con las siguientes características:

- Resumirlo: Resume secciones clave o un capítulo completo.
- Explicadores de código con IA: En el Packt Reader de última generación, haz clic en el botón "Explain" sobre cada bloque de código para obtener explicaciones de código impulsadas por IA.

`Nota: El asistente de IA es parte del Packt Reader de última generación y aún está en beta.`

### Versión PDF o ePub sin DRM

Aprende sin límites con los siguientes beneficios incluidos con tu compra:

- Aprende desde cualquier lugar con una copia en PDF sin DRM de este libro.
- Usa tu lector electrónico favorito para aprender usando una versión ePub sin DRM de este libro.

### Mi camino hacia los microservicios

Cuando conocí por primera vez el concepto de microservicios en 2014, me di cuenta de que había estado desarrollando microservicios (bueno, más o menos) durante varios años sin saber que eran microservicios con los que estaba tratando.
Estuve involucrado en un proyecto que comenzó en 2009 donde desarrollamos una plataforma basada en un conjunto de funcionalidades separadas. La plataforma se entregaba a varios clientes que la desplegaban on-premises. Para facilitar que los clientes pudieran elegir qué funcionalidades querían usar de la plataforma, cada funcionalidad se desarrolló como un componente de software autónomo; es decir, tenía sus propios datos persistentes y solo se comunicaba con otros componentes usando APIs bien definidas.

Como no puedo discutir características específicas de la plataforma de este proyecto, he generalizado los nombres de los componentes, los cuales están etiquetados desde el Componente A hasta el Componente F. La composición de la plataforma como un conjunto de componentes se ilustra de la siguiente manera:

![Figura 1.4](~/Documents/Microservices/images/figure1.4.png)

De la ilustración, también podemos ver que cada componente tiene su propio almacenamiento para datos persistentes y no comparte bases de datos con otros componentes.
Cada componente se desarrolla usando Java y el Spring Framework, se empaqueta como un archivo WAR y se despliega como una aplicación web en un contenedor web Java EE, por ejemplo, Apache Tomcat. Dependiendo de los requisitos específicos del cliente, la plataforma puede desplegarse en uno o varios servidores.
Un despliegue de dos nodos podría verse de la siguiente manera:

## ![Figura 1.5](~/Documents/Microservices/images/figure1.5.png)

### Beneficios de los componentes de software autónomos

De este proyecto, aprendí que descomponer la funcionalidad de la plataforma en un conjunto de componentes de software autónomos proporciona varios beneficios:

- Un cliente puede desplegar partes de la plataforma en su propio paisaje de sistemas, integrándola con sus sistemas existentes usando sus APIs bien definidas.
- El siguiente es un ejemplo donde un cliente decidió desplegar el Componente A, el Componente B, el Componente D y el Componente E de la plataforma e integrarlos con dos sistemas existentes, el Sistema A y el Sistema B, en el paisaje de sistemas del cliente:

![Figura 1.6](~/Documents/Microservices/images/figure1.6.png)

- Otro cliente podría optar por reemplazar partes de la funcionalidad de la plataforma con implementaciones que ya existen en el paisaje de sistemas del cliente, requiriendo potencialmente alguna adaptación de la funcionalidad existente a las APIs de la plataforma. El siguiente es un ejemplo donde un cliente ha reemplazado el Componente C y el Componente F de la plataforma con su propia implementación:

![Figura 1.7](~/Documents/Microservices/images/figure1.7.png)

- Cada componente en la plataforma puede ser entregado y actualizado por separado. Gracias al uso de APIs bien definidas, un componente puede ser actualizado a una nueva versión sin depender del ciclo de vida de los otros componentes.
  El siguiente es un ejemplo donde el Componente A ha sido actualizado de la versión v1.1 a v1.2. El Componente B, que llama al Componente A, no necesita ser actualizado ya que usa una API bien definida; es decir, sigue siendo la misma después de la actualización (o al menos es compatible hacia atrás):

![Figura 1.8](~/Documents/Microservices/images/figure1.8.png)

- Gracias al uso de APIs bien definidas, cada componente en la plataforma también puede escalarse horizontalmente a múltiples servidores de forma independiente de los otros componentes. El escalado puede hacerse ya sea para cumplir con requisitos de alta disponibilidad o para manejar volúmenes más altos de solicitudes. En este proyecto específico, se logró configurando manualmente balanceadores de carga frente a varios servidores, cada uno ejecutando un contenedor web Java EE. Un ejemplo donde el Componente A ha sido escalado a tres instancias se ve de la siguiente manera:

![Figura 1.9](~/Documents/Microservices/images/figura1.9.png)

### Desafíos con los componentes de software autónomos

Mi equipo también aprendió que descomponer la plataforma introdujo varios desafíos nuevos a los que no estábamos expuestos (al menos no en el mismo grado) al desarrollar aplicaciones monolíticas más tradicionales:

- Agregar nuevas instancias a un componente requería configurar manualmente balanceadores de carga y configurar manualmente nuevos nodos. Este trabajo consumía mucho tiempo y era propenso a errores.
- La plataforma era inicialmente propensa a errores causados por otros sistemas con los que se comunicaba. Si un sistema dejaba de responder a las solicitudes enviadas desde la plataforma de manera oportuna, la plataforma se quedaba rápidamente sin recursos cruciales, por ejemplo, hilos del SO, especialmente cuando estaba expuesta a un gran número de solicitudes concurrentes. Esto causaba que los componentes en la plataforma se colgaran o incluso se bloquearan. Dado que la mayor parte de la comunicación en la plataforma se basaba en comunicación síncrona, un componente que fallara podía llevar a fallos en cascada; es decir, los clientes de los componentes que fallaban también podían fallar después de un tiempo. Esto se conoce como una cadena de fallos.
- Mantener la configuración en todas las instancias de los componentes consistente y actualizada rápidamente se convirtió en un problema, causando mucho trabajo manual y repetitivo. Esto llevó a problemas de calidad de vez en cuando.
- Monitorear el estado de la plataforma en términos de problemas de latencia y uso de hardware (por ejemplo, uso de CPU, memoria, discos y red) era más complicado en comparación con monitorear una sola instancia de una aplicación monolítica.
- Recopilar archivos de registro de varios componentes distribuidos y correlacionar eventos de registro relacionados de los componentes también era difícil, pero factible ya que el número de componentes era fijo y conocido de antemano.

Con el tiempo, abordamos la mayoría de los desafíos mencionados en la lista anterior con una combinación de herramientas desarrolladas internamente e instrucciones bien documentadas para manejar estos desafíos manualmente. La escala de la operación estaba, en general, en un nivel donde los procedimientos manuales para lanzar nuevas versiones de los componentes y manejar problemas en tiempo de ejecución eran aceptables, aunque no fueran deseables.

### El surgimiento de los microservicios

Aprender sobre arquitecturas basadas en microservicios en 2014 me hizo darme cuenta de que otros proyectos también habían estado lidiando con desafíos similares (en parte por otras razones diferentes a las que describí anteriormente, por ejemplo, los grandes proveedores de servicios en la nube cumpliendo con requisitos de escala web). Muchos pioneros de los microservicios habían publicado detalles de las lecciones que aprendieron. Fue muy interesante aprender de estas lecciones.

Muchos de los pioneros desarrollaron inicialmente aplicaciones monolíticas que los hicieron muy exitosos desde una perspectiva empresarial. Pero con el tiempo, estas aplicaciones monolíticas se volvieron cada vez más difíciles de mantener y evolucionar. También se volvieron difíciles de escalar más allá de las capacidades de las máquinas más grandes disponibles (también conocido como escalado vertical). Eventualmente, los pioneros comenzaron a encontrar formas de dividir las aplicaciones monolíticas en componentes más pequeños que pudieran lanzarse y escalarse independientemente unos de otros. Escalar componentes pequeños puede hacerse usando escalado horizontal, es decir, desplegando un componente en varios servidores más pequeños y colocando un balanceador de carga frente a ellos. Si se hace en la nube, la capacidad de escalado es potencialmente infinita – es solo cuestión de cuántos servidores virtuales incorpores (asumiendo que tu componente pueda escalarse horizontalmente en una gran cantidad de instancias, pero más sobre eso más adelante).

En 2014, también aprendí sobre varios proyectos nuevos de código abierto que ofrecían herramientas y frameworks que simplificaban el desarrollo de microservicios y podían usarse para manejar los desafíos que conlleva una arquitectura basada en microservicios. Algunos de estos son los siguientes:

- Pivotal lanzó Spring Cloud, que envuelve partes de Netflix OSS para proporcionar capacidades como descubrimiento dinámico de servicios, gestión de configuración, trazado distribuido, circuit breaking, y más.
- También aprendí sobre Docker y la revolución de los contenedores, que es excelente para minimizar la brecha entre desarrollo y producción. Poder empaquetar un componente no solo como un artefacto de runtime desplegable (por ejemplo, un archivo .war o .jar de Java) sino como una imagen completa, lista para ser lanzada como un contenedor en un servidor ejecutando Docker, fue un gran avance para el desarrollo y las pruebas.

`Por ahora, piensa en un contenedor como un proceso aislado. Aprenderemos más sobre contenedores en el Capítulo 4, Despliegue de Nuestros Microservicios Usando Docker.`

- Un motor de contenedores, como Docker, no es suficiente para poder usar contenedores en un entorno de producción. Se necesita algo que pueda asegurar que todos los contenedores estén funcionando y que pueda escalar contenedores en varios servidores, proporcionando así alta disponibilidad y mayores recursos de cómputo.
- Estos tipos de productos se conocieron como orquestadores de contenedores. Varios productos han evolucionado en los últimos años, como Apache Mesos, Docker en modo swarm, Amazon ECS, HashiCorp Nomad y Kubernetes. Kubernetes fue desarrollado inicialmente por Google. Cuando Google lanzó la v1.0 en 2015, también donó Kubernetes a la CNCF (https://www.cncf.io/). Durante 2018, Kubernetes se convirtió en una especie de estándar de facto, disponible tanto preempaquetado para uso on-premises como como servicio de la mayoría de los principales proveedores de nube.

`Como se explica en https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/, Kubernetes es en realidad una reescritura de código abierto de un orquestador de contenedores interno, llamado Borg, utilizado por Google durante más de una década antes de que se fundara el proyecto Kubernetes.`

- En 2018, comencé a aprender sobre el concepto de una malla de servicios (service mesh) y cómo puede complementar a un orquestador de contenedores para descargar aún más responsabilidades de los microservicios con el fin de hacerlos manejables y resilientes.
