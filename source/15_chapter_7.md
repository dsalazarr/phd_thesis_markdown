# Seguridad

Cuando comercializamos una aplicación que contendrá datos sensibles de usuarios y empresas como esta un aspecto importante es asegurar que todos estos datos son accedidos solo por la gente que debe, además de asegurar la persistencia e integridad de estos datos.

## Seguridad para el software

Es importante poder tracear de que forma se ha accedido al sistema y quién ha modificado cada cosa. Para ello es importante mantener un registro de auditoría del sistema.

### Subsistema de administración

Para el subsistema de administración se mantiene un log de eventos en forma de tabla en la base de datos, este log indica cada acción realizada, sobre que tabla y qué usuario, además se almacenan también los intentos de acceso al sistema de administración.

### Subsistema de integración

Al igual que en el otro subsistema, en este caso se hace necesario saber qué aplicación está accediendo y en nombre de quién, para ello en la misma tabla anterior añadimos un campo application_id, para saber desde qué aplicación se está realizando la modificación, siendo nulo en caso de haberse realizado directamente desde el panel de administración.

### Logs

Además de los datos anteriores, es necesario guardar logs de aplicación en el servidor de logs correspondiente, para ello utilizamos la herramienta *syslog-ng* para redirigir los logs generados por la aplicación al servidor correspondiente. La aplicación debe generar logs útiles de todas las acciones que se realicen, de forma que sean trazables.

## Seguridad de los datos

Al ser una aplicación multi-tenant los datos no deben ser accesibles de un cliente a otro, ya que en principio no estarían físicamente separados, la forma de hacerlo será por software, en forma de esquemas diferentes de base de datos, asegurando así que no serán accedidos de un tenant a otro.

### Copias de seguridad

Al no ser datos extremadamente críticos se decide hacer una copia de seguridad al día, esta copia está gestionada por AWS, de forma que no tenemos que preocuparnos mucho más que de configurarla.

### Mecanismos de integridad

La base de datos ayudada por el ORM mantiene en todo momento integridad referencial, al ser una base de datos SQL con sus relaciones definidas.

## Seguridad para el usuario

También se debe asegurar que cada usuario y cada aplicación solo accede a los datos que debe, para ello se configuran permisos para cada usuario y luego se añade la capa de OAuth, para la cual solo se otorgan permisos limitados por token. A continuación se explicará el protocolo OAuth 2.0 en detalle.

### OAuth

OAuth 2.0 es el protocolo estándar de la industria para autorización. OAuth 2.0 se enfoca en la simplicidad para proveer flujos de autorizaciones para aplicaciones, ya sean de tipo web, escritorio, móvil, etc. OAuth es una especificación desarrollada en el marco de la IETF.

#### Flujos de OAuth

OAuth dispone de varios flujos de funcionamiento, en función de si la aplicación es pública o privada. A continuación se explicarán todos los flujos disponibles.

##### Flujo de autorización en 3 pasos

Este flujo está pensado para aplicaciones públicas de terceros, en las cuales no queremos que la aplicación tenga acceso a las credenciales de usuario, para ello en primer lugar se envía al usuario al servidor de autorización, mostrando una interfaz para introducir sus credenciales, estas credenciales son introducidas directamente en el servidor, por lo que la aplicación no tiene acceso a ellas.

Una vez validadas el servidor devuelve un código temporal de autorización a la aplicación, que debe disponer de un servidor con una url para recoger este código.

Una vez obtenido el código este debe ser intercambiado por un token de acceso definitivo, para ello con el código y las credenciales de aplicación, ésta realiza una petición al servidor de autorización para intercambiar el código por un token, el servidor devuelve el token y el código deja de ser válido.


##### Flujo de autorización en 2 pasos

Este flujo existe para aplicaciones públicas confiables, en principio aplicaciones desarrolladas por el mismo proveedor de autorización pero que funcionan de forma pública, tipo aplicaciones móvil, etc. En estas aplicaciones conocemos que no almacenarán las credenciales del usuario por lo que son confiables.

En primer lugar la aplicación realiza una petición al servidor con las credenciales de aplicación y de usuario, el servidor responde directamente con el token de acceso y la aplicación ya puede empezar a funcionar.

##### Flujo de autorización para aplicaciones

Este flujo existe para aplicaciones privadas desarrolladas por el proveedor de autorización, que son totalmente confiables, estas aplicaciones utilizan solamente sus credenciales de aplicación, sin necesitar las de usuario, y en principio tendrían más privilegios que las normales, ya que no están disponibles de forma pública.

En primer lugar la aplicación realiza una petición al servidor enviando sus credenciales, el servidor responde con un token de acceso.

### Permisos

Cada usuario tiene unos permisos definidos con lo que puede ver en el sistema, además de esto se pueden configurar permisos personalizados por aplicación que serán otorgados luego al usuario.

### Scopes

Los scopes son permisos que la aplicación obtiene sobre un usuario para actuar en su nombre.

## Seguridad del hardware

Esta capa a pesar de ser importante está gestionada por AWS, al haber configurado los sistemas de forma que sean fácilmente replicables no tenemos que preocuparnos por este aspecto.

