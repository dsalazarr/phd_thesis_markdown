# Diseño

## Introducción

La fase de diseño consiste en aplicar una serie de técnicas para transformar los requisitos elicitados en la fase de análisis en una estructura detallada para el sistema de forma que se pueda implementar fácilmente a partir de ese diseño.

Este diseño ha de realizarse a varios niveles de detalle, por un lado a nivel de arquitectura interna de la aplicación. Por otro lado el diseño del modelo de datos y finalmente a más alto nivel detallar todos los componentes del sistema y como interactúan entre si, componiendo un sistema escalable.

Siguiendo los requisitos de la fase anterior, esta tarea es relativamente sencilla y debe resultar en una serie de diagramas y especificaciones que sirvan como guión y documentación a las personas que vayan a participar, ahora o en un futuro en el desarrollo del sistema.

Este documento debe contener también un modelo de despliegue incluyendo infraestructura, aplicación y configuración.

## Arquitectura de aplicación

La aplicación se rige por el patrón arquitectónico MVC. Este patrón permite separar la lógica de negocio de la presentación, así como de los datos.

* El modelo representa a las estructuras de datos. Las clases del modelo contienen funciones para modificar los datos, insertar y actualizar la base de datos.
* La vista es la información presentada al usuario. Una vista normalmente es una página web que puede contener datos del modelo para mostrarlos al usuario.
* El controlador es un intermediario entre las dos capas anteriores y otros recursos que puedan ser necesarios. Se encarga de procesar las peticiones y generar la página web que será presentada al usuario.

Este patrón favorece la reutilización de código y la claridad.

### Elección del framework

Dado que en el lenguaje elegido para implementar la aplicación es Python, se ha elegido el framework Django, debido a la experiencia previa con esta tecnología. Este framework permite empezar a tener un software funcionando en muy poco tiempo, dedicando el trabajo casi exclusivamente a implementar el modelo y la lógica de negocio. Dado que el framework ya es conocido, la curva de aprendizaje es mínima.

### Endpoints

#### Empresas

##### Visualización

GET: /v1/companies

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Response (Lista de):
    * **"id"**: 123,
    * **"name"**: "Company",
    * **"code"**: "company_1"
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

GET: /v1/companies/\<company-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response (JSON Object):
    * **"id"**: 123,
    * **"name"**: "Company",
    * **"code"**: "company_1"
* Response Codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)
    * 404 NOT FOUND (*company-id* no encontrado)

##### Creación

POST: /v1/companies

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "Company",
    * **"code"**: "company_1"
* Response codes:
    * 201 CREATED (Empresa creada)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)


##### Actualización

PUT: /v1/companies/\<company-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "Company",
    * **"code"**: "company_id"
* Response codes:
    * 200 OK (Empresa actualizada)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Borrado

DELETE: /v1/companies/\<company-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response codes:
    * 204 NO CONTENT (Empresa borrada)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)


#### Usuarios

##### Visualización

GET: /v1/companies/\<company-id\>/users

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Response (Lista de):
    * **"id"**: 123,
    * **"name"**: "User",
    * **"email"**: "user@company.com"
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

GET: /v1/companies/\<company-id\>/users/\<user-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response (JSON Object):
    * **"id"**: 123,
    * **"name"**: "User",
    * **"email"**: "email@company.com"
* Response Codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)
    * 404 NOT FOUND (*company-id* o *user-id* no encontrado)

##### Creación

POST: /v1/companies/\<company-id\>/users

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "Nombre",
    * **"email"**: "user@company.com",
    * **"password"**: "password"
* Response codes:
    * 201 CREATED (Usuario creado)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)


##### Actualización

PUT: /v1/companies/\<company-id\>/users/\<user-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "User",
    * **"email"**: "email@company.com"
    * **"password"**: "password"
* Response codes:
    * 200 OK (Usuario actualizada)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Borrado

DELETE: /v1/companies/\<company-id\>/users/\<user-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response codes:
    * 204 NO CONTENT (Usuario borrado)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

#### Aplicaciones

##### Visualización

GET: /v1/applications

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Response (Lista de):
    * **"id"**: 123,
    * **"name"**: "application",
    * **"client_id"**: "123123123",
    * **"client_secret"**: "123123123",
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

GET: /v1/applications/\<client-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response (JSON Object):
    * **"id"**: 123,
    * **"name"**: "User",
    * **"client_id"**: "123123123",
    * **"client_secret"**: "123123123",
* Response Codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)
    * 404 NOT FOUND (*client-id* no encontrado)

##### Creación

POST: /v1/applications/\<client-id\>

* Headers:
    * Content-Type: applicacion/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "Nombre",
* Response (JSON Object):
    * **"id"**: 123,
    * **"name"**: "User",
    * **"client_id"**: "123123123",
    * **"client_secret"**: "123123123",
* Response codes:
    * 201 CREATED (Aplicación creada)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)


##### Actualización

PUT: /v1/applications/\<client-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Request body (JSON Object):
    * **"name"**: "User",
* Response codes:
    * 200 OK (Aplicación actualizada)
    * 400 BAD REQUEST (Dato incorrecto en el body)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Borrado

DELETE: /v1/applications/\<client-id\>

* Headers:
    * Content-Type: application/json
    * Authorization: Bearer \<token\>
* Response codes:
    * 204 NO CONTENT (Aplicación borrada)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

#### Access Tokens

##### Creación (authorization code)

POST /v1/oauth2/authorization

* Headers:
    * Content-Type: application/json
* Request body (JSON Object)
    * **"username"**: "\<user_email\>",
    * **"password"**: "\<user_password\>",
    * **"client_id"**: "\<client_id\>"
    * **"client_secret"**: "\<client_secret\>"
    * **"redirect_uri"**: "http://auth_server"
* Response body:
    * Redirect to http://auth_server?code=code
* Response codes:
    * 302 Redirect (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Creación (password grant)

POST /v1/oauth2/access-tokens

* Headers:
    * Content-Type: application/json
* Request body (JSON Object)
    * **"grant_type"**: "password",
    * **"username"**: "\<user_email\>",
    * **"password"**: "\<user_password\>",
    * **"client_id"**: "\<client_id\>"
    * **"client_secret"**: "\<client_secret\>"
* Response body:
    * **"token"**: "token",
    * **"refresh_token"**: "refresh_token",
    * **"expires_in"**: 123123  (seconds)
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Creación (client_credentials grant)

POST /v1/oauth2/access-tokens

* Headers:
    * Content-Type: application/json
* Request body (JSON Object)
    * **"grant_type"**: "client_credentials",
    * **"client_id"**: "\<client_id\>"
    * **"client_secret"**: "\<client_secret\>"
* Response body:
    * **"token"**: "token",
    * **"refresh_token"**: "refresh_token",
    * **"expires_in"**: 123123  (seconds)
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Creación (authorization_code grant)

POST /v1/oauth2/access-tokens

* Headers:
    * Content-Type: application/json
* Request body (JSON Object)
    * **"grant_type"**: "authorization_code",
    * **"client_id"**: "\<client_id\>"
    * **"client_secret"**: "\<client_secret\>"
    * **"code"**: "\<code\>"
* Response body:
    * **"token"**: "token",
    * **"refresh_token"**: "refresh_token",
    * **"expires_in"**: 123123  (seconds)
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)

##### Actualización (refresh_token grant)

POST /v1/oauth2/access-tokens

* Headers:
    * Content-Type: application/json
* Request body (JSON Object)
    * **"grant_type"**: "refresh_token",
    * **"client_id"**: "\<client_id\>"
    * **"client_secret"**: "\<client_secret\>"
    * **"refresh_token"**: "\<refresh_token\>"
* Response body:
    * **"token"**: "token",
    * **"refresh_token"**: "refresh_token",
    * **"expires_in"**: 123123  (seconds)
* Response codes:
    * 200 OK (Success)
    * 401 UNAUTHORIZED (Token de acceso inválido o no enviado)
    * 403 FORBIDDEN (Sin permisos)


## Base de datos
Para el diseño de la base de datos en la que se guardarán los datos manejados por la aplicación se usará un modelo relacional. Se usará MySQL como sistema de gestión de base de datos.

### Modelo entidad-relación
<!--
TODO
-->
![Modelo Entidad Relación \label{modelo_er}](source/figures/er-model-pfc.png)
\ref{modelo_er}

<!--
Table \ref{ref_a_table} shows us how to add a table. Integer tincidunt sed nisl eget pellentesque. Mauris eleifend, nisl non lobortis fringilla, sapien eros aliquet orci, vitae pretium massa neque eu turpis. Pellentesque tincidunt aliquet volutpat. Ut ornare dui id ex sodales laoreet.
-->
<!--

\newpage

---------------------------------------------------------------------------
Column 1            Column 2                Column 3
--------------      -------------------     -------------------
Row 1               0.1                     0.2

Row 2               0.3                     0.3

Row 3               0.4                     0.4

Row 4               0.5                     0.6

---------------------------------------------------------------------------

Table: This is the table caption. Suspendisse blandit dolor sed tellus venenatis, venenatis fringilla turpis pretium. \label{ref_a_table}

-->

### Tablas y atributos
<!--
TODO
-->

#### Usuarios

--------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ---------            ---------        -----------------    -----------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

email                  VARCHAR(50)          NO               UNIQUE               Email de usuario, sirve para login

password               VARCHAR(255)         NO               NO                   Password cifrado de usuario

nombre                 VARCHAR(255)         NO               NO                   Nombre del usuario

apellido               VARCHAR(255)         NO               NO                   Apellidos del usuario

company_id             INTEGER              NO               FOREIGN_KEY          Id de empresa

-----------------------------------------------------------------------------------

##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.


#### Aplicaciones

---------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ------------         -----------      --------------       ------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

client_id              VARCHAR(50)          NO               UNIQUE               client id

client_secret          VARCHAR(50)          NO               NO                   client secret

name                   VARCHAR(50)          NO               NO                   Nombre de aplicación

-----------------------------------------------------------------------------------------


##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.


#### Empresas

---------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ------------         -----------      --------------       ------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

code                   VARCHAR(50)          NO               UNIQUE               Código identificador de la empresa

name                   VARCHAR(50)          NO               NO                   Nombre de empresa

-----------------------------------------------------------------------------------------

##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.

#### Tokens de acceso

---------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ------------         -----------      --------------       ------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

token                  VARCHAR(50)          NO               UNIQUE               Token de acceso de usuario

user_id                INTEGER              NO               FOREIGN_KEY          Id de usuario

application_id         INTEGER              NO               FOREIGN_KEY          Id de aplicación

permisos               VARCHAR(255)         NO               NO                   Permisos de token de usuario

expires                INTEGER              NO               NO                   Segundos de expiración de token

-----------------------------------------------------------------------------------------

##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.

#### Refresh tokens

---------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ------------         -----------      --------------       ------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

token                  VARCHAR(50)          NO               UNIQUE               Token de refresco de usuario

access_token_id        INTEGER              NO               FOREIGN_KEY          Id de usuario

-----------------------------------------------------------------------------------------

##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.

#### Authorization codes

---------------------------------------------------------------------------------
Atributos              Tipo                 Nulo             Index                Descripción
-------------          ------------         -----------      --------------       ------------
id                     INTEGER              NO               PRIMARY_KEY          Identificador autoincremental

code                   VARCHAR(50)          NO               UNIQUE               Código de autorización

application_id         INTEGER              NO               FOREIGN_KEY          Id de usuario

user_id                INTEGER              NO               FOREIGN_KEY          Id de usuario

expires                INTEGER              NO               NO                   Segundos de expiración de token

-----------------------------------------------------------------------------------------

##### Normalización

* La tabla está en primera forma normal ya que:
    * Todos los atributos son atómicos.
    * Tiene clave primaria única (id).
    * La CP no puede ser nula.
* La tabla está en segunda forma normal ya que:
    * Está en 1FN.
    * Al ser la clave única no puede haber dependencias parciales.
* La tabla está en tercera forma normal ya que:
    * Está en 2FN.
    * No hay dependencias funcionales transitivas.
* La tabla está en forma normal de Boyce-Codd ya que:
    * Para toda dependencia funcional X->A X es superllave.

## Arquitectura del sistema

El sistema completo se compone de varias partes, si bien la aplicación es agnóstica en cuanto a los componentes de los que depende, en este documento se detallará la tencología elegida para cada una de ellas.

La aplicación principal con la lógica de negocio está desarrollada en Python, y depende de una base de datos, en este caso se ha elegido MySQL. Esta aplicación se descompone a su vez en una API que será la que esté expuesta públicamente y un worker para tareas que se ejecutan en segundo plano. Ambos servicios necesitan de la base de datos por lo que tienen acceso a ésta.

Las peticiones de la API más usadas deberán estar en una caché para evitar la sobrecarga de la aplicación y la Base de Datos.

Todos estos servicios estarán controlados por un servidor de aplicaciones, que expondrá un punto de entrada al cual dará acceso un servidor web.

### API

La api es un servicio desarrollado en Django, este servicio expone diferentes endpoints REST para realizar diversas labores en la aplicación, dentro de este servicio reside la lógica de negocio, que no está separada de la API en si misma. En caso de que otros servicios necesitaran de esta lógica de negocio la separación sería sencilla ya que el proyecto está dividido en äplicacionesÿ cada una de estas aplicaciones puede extraerse a una librería separada.

### Base de datos

Como sistema de gestión de bases de datos se ha elegido MySQL debido a la experiencia previa con este sistema. La base de datos es accesible con un usuario con los permisos que exclusivamente necesita la aplicación.

### Worker

Un worker es un servicio que trabaja ejecutando tareas asíncronas que va recibiendo en una cola, este worker descarga de trabajo a la API, de forma que esta pueda responder rápidamente a las peticiones que recibe, derivando los trabajos más pesados a esta cola. Este worker puede recibir tareas que ejecutará inmediatamente, o bien recibir tareas pospuestas para un momento concreto en el tiempo. Este sistema debe poder funcionar de forma distribuida, por lo tanto para funcionar necesita de un backend común para almacenar las tareas hasta que estas sean ejecutadas.

Como tecnología para el worker se ha elegido celery, ya que es la mejor opción existente actualmente en Python, permite funcionar con todos los requisitos expuestos anteriormente.

Como tecnología para el backend se ha elegido redis, que es un almacén de datos clave valor que se ejecuta de forma ligera.

### Servidor de aplicaciones

Un servidor de aplicaciones es una pieza de software necesaria para ejecutar cierto tipo de aplicaciones y servirlas a un cliente a través de un puerto. Django y en general Python utilizan el protocolo Wsgi para comunicarse con servidores de este tipo

WSGI es una especificación que actúa de interfaz entre servidores web y aplicaciones web, es el estándar adoptado por Python para este tipo de comunicaciones.

Entre la aplicación y el servidor puede existir un middleware para procesar la petición y enrutarla a la parte de código correspondiente. En nuestro modelo este middleware sería el router de Django, que se encarga de parsear la URL y enviarla a la vista correspondiente que se encargará de ejecutar la petición, una vez procesada, el middleware devuelve la respuesta al servidor de aplicaciones que se encarga de transmitirla al cliente.

Como implementación de servidor de aplicaciones hemos elegido uWSGI, que es una implementación del protocolo altamente configurable, que además proporciona mejor rendimiento que la mayoría de rivales.

### Servidor web

En ocasiones es necesario un servidor web en modo reverse proxy que se encargue de enrutar la petición al servidor de aplicaciones correspondiente, como pueden existir varios servicios para responder a la petición, delante del servidor de aplicaciones se pone este reverse proxy que se encarga de enroutar la petición al servicio adecuado.

En esta ocasión hemos elegido NGINX por su facilidad de configuración. Este servidor también nos sirve para servir ficheros estáticos como imágenes o plantillas.

### Infraestructura

Uno de los requisitos no funcionales de la aplicación es la alta disponibilidad, por ello necesitamos un hosting confiable del cual también podamos controlar los gastos.

Además de esto, el hosting no es lo único que necesitamos, también necesitamos poder asignar direcciones DNS, un servidor de email, entre otras cosas.

En los últimos tiempos han ido ploriferando plataformas en la nube como Amazon Web Services (AWS) o Google Cloud Platform. AWS nos ofrece de serie una capa gratuita durante un año, con lo cual nos decidimos por esta plataforma, ya que ofrece todos los servicios que necesitamos.

Otro de los requisitos no funcionales es la escalabilidad, con esto nos referimos a que el servicio pueda adecuarse a los cambios en el tráfico de la aplicación. AWS ofrece de serie servicios autoescalables, de forma que la aplicación se ofrezca en servidores más potentes o se incremente el número de servidores que la sirven automáticamente. Con lo cual en este aspecto también nos puede ser útil.

### Escalabilidad

Ya hemos hablado previamente de escalabilidad, pero en un software de estas características, es impor- tante tener claros los elementos que tienen que intervenir para que el servicio pueda responder a una carga alta de tráfico.

#### Balanceador de carga

Un balanceador de carga es un servicio que permite distribuir la carga de trabajo entre varios nodos, evitando así la sobrecarga de uno de estos nodos, lo que provocaría una caída del rendimiento del servicio. Este componente aumenta la fiabilidad del servicio a través de la redundancia, es decir el servicio está replicado en varios nodos y es el balanceador de carga el que se encarga de distribuir las peticiones a base de varios criterios, entre los que estarían la carga de cada nodo, si un nodo se identifica como no saludable, etc.

Este servicio divide la carga usando interfaces de red, por lo que actúa en la capa 4 del modelo OSI, esto quiere decir que utiliza las direcciones IP y el puerto TCP de origen y destino para enrutar la petición.

AWS ofrece Elastic Load Balancer como componente para balanceo de carga. ELB es fácilmente configurable, solo hay que añadir los hosts que servirán el tráfico y ELB se encargará de repartirlo. La pega es que ELB, al ser tan sencillo, también es bastante limitado, no ofrece autoescalabilidad, y reparte la carga por igual, por lo que si las diferentes máquinas configuradas tienen capacidad diferente, unas podrían llegar al límite de carga antes que otras.
Con el nivel de carga estimado inicialmente parece suficiente con este modelo, pero aun así hay que tener previsión para mover a un modelo diferente, autoescalable, que permita reducir costes.
Para ello, Amazon ofrece una alternativa llamada Application Load Balancer, que es un balanceador de carga pensado para aplicaciones. Este componente actúa en la capa de aplicación, basándose en el contenido de la petición para enrutarla al objetivo correspondiente. Está pensado para arquitecturas de aplicación modernas, como microservicios y aplicaciones basadas en contenedores tipo Docker o Kubernetes, de la cual nos podríamos beneficiar para mejorar la disponibilidad.

#### Servicios autoescalables

Amazon ofrece el servicio Elastic Beanstalk para ejecutar aplicaciones. Este servicio es una abstracción que ofrece Amazon para desplegar aplicaciones sin preocuparnos por el provisionamiento de la máquina, autoescalado y monitorización.

#### Alta disponibilidad

Es importante tener en cuenta las posibles pérdidas de servicio de alguna de nuestras instancias, ni si- quiera un proveedor como AWS es 100 % fiable y hay que estar preparado para posibles fallos.

Para ello AWS ofrece diferentes availability zones donde ejecutar nuestra aplicación, de forma que podemos poner instancias de nuestra aplicación en diferentes AZ y si una de estas AZ cae, tendremos disponibles otras sin que nuestro servicio se vea afectado. En estos casos es el load balancer quién se encarga de redirigir el tráfico en caso de caídas.

## Despliegue

Otra parte importante del diseño de la aplicación es definir claramente como será el proceso de despliegue, esto es, como se llevará el software al entorno de producción, como se cargará la configuración y como se preparará la infraestructura para que todo funcione.

### Despliegue de software

El paquete de software que se entregue tiene que ser en todo momento replicable en cualquier otro entorno, de forma que si queremos volver atrás tengamos la seguridad de que todo va a funcionar. Por tanto todas las tareas de compilación y preparación del software deben hacerse antes del despliegue. Lo que se entregará será un paquete listo para ser ejecutado en cualquier entorno.

#### Paquetizado

Existen diferentes alternativas para este método, en nuestro caso usaremos paquetes Debian, ya que el entorno de despliegue será una máquina con esta distribución. Para preparar un paquete debian primero necesitaremos preinstalar nuestra aplicación en un entorno virtual de Python (virtualenv).

El problema principal de virtualenv es que mantiene las rutas *shebang* de los ficheros que genera de forma absoluta, por tanto mantendrá los de la máquina en la que lo instalemos, pudiendo ser diferentes a las de la máquina en producción. Para solucionar esto existe una herramienta llamada *dh-virtualenv*.

Una vez preparado el virtualenv con la aplicación instalada será eso lo que empaquetemos en el Debian, y será el paquete Debian lo que se entregará a los servidores de producción, de forma que la versión sea replicable.

#### Gestión de configuración

Siguiendo las recomendaciones de buenas prácticas en diseño de software, la configuración no puede estar en el repositorio junto con el código. Hay diferentes razones por las que no hacer esto, algunas de ellas son que la configuración es dependiente del entorno, por tanto tendremos tantas configuraciones como entornos tengamos, lo cual hace imposible mantener todas en el repositorio, además de esto, muchos valores de configuración son secretos, mantenerlos en claro en el repositorio no es una opción.

Para ello, la configuración se cargará siempre desde variables de entorno, para evitar de ninguna forma mantener ficheros con configuración. Estas variables de entorno se cargarán desde el software de gestión de configuración, que en nuestro caso será Ansible.

### Gestión de infraestructura

La infraestructura debe estar preparada y configurada para soportar nuestra aplicación, para ello hay que provisionarla con ciertos valores y necesitaremos una herramienta adecuada que nos permita automatizar el proceso para que también pueda ser replicado, para ello usaremos Puppet, además de Terraform para prerparar el entorno de AWS.

## Métricas y monitorización

Para una aplicación de esta escala se hace imprescindible generar métricas de las cuales en el futuro se pueda extraer información, igualmente la aplicación tiene que estar monitorizada, para en caso de fallos, informar a los administradores del sistema para resolverlo lo antes posible.

### Métricas

Las métricas son puntos de datos generados por la aplicación que se envían a un servidor de métricas, estas métricas son configuradas de forma que se pueda extraer información útil de ellas.

Para un software comercial es imprescindible tener este tipo de información para conseguir una mayor monetización, además de poder añadir nuevas características en base a la información extraída.

Estas métricas se componen de un backend o base de datos y de un dashboard para configurarlas y visualizarlas, para el backend hemos elegido *InfluxDB* que es una de las bases de datos más utilizadas para este propósito.

*InfluxDB* es una base de datos de código abierto que almacena series basadas en el tiempo. Está escrita en *Go* y optimizada para ser usada en entornos de tiempo real y de alta disponibilidad.

Como dashboard hemos utilizado *Grafana*, es un dashboard configurable totalmente compatible con *InfluxDB*.

![Dashboard de ejemplo de Grafana \label{dashboard}](source/figures/dashboard.png)

### Monitorización

La monitorización es una parte importante para controlar la estabilidad del sistema, sin unas herramientas adecuadas no podemos asegurar la disponibilidad y el buen funcionamiento de nuestro software, para ello existen diferentes herramientas, como la anteriormente mencionada *Grafana* y a nivel de procesos *monit*, monit nos asegura que nuestros servicios estarán siempre levantados, si hay alguna caída, la herramienta los volverá a levantar.

### Alertas

En caso de caída de servicio necesitamos tener un sistema de alertas que nos comunique el fallo correspondiente, para ello se utilizará además de *monit*, *Sensu*, que es un sistema configurable de checks para nuestros servicios, *Sensu* es a su vez compatible con sistemas de alerta a equipos de operaciones, como por ejemplo *OpsGenie*.

[@SQLCookbook]

[@DBSConcepts]

[@GoF]

[@EntPatterns]

[@UMLDistilled]

[@AWSTut]

[@TDDExample]

[@2Scoops]

[@Python]

[@PythonPocket]

[@CodeComplete]

[@Pragmatic]

[@TDDPython]

[@STDPython]

[@STDDjango]
