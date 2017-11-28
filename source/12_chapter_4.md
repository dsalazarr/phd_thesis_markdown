# Análisis

## Metodología de desarrollo

Para la realización del proyecto y su documentación se ha utilizado el Rational Unified Process (RUP), junto con el Lenguaje Unificado de Modelado (UML). Se ha elegido este sistema ya que es la metodología estándar más utilizada, además de ser un grupo de metodologías que se adaptan muy bien a las necesidades de un producto.

## Especificación de requisitos del sistema

A continuación se enumeran los requisitos funcionales que se consideran fundamentales para el sistema. Éstos serán detallados utilizando casos de uso, describiendo tanto su escenario principal como sus posibles flujos alternativos. Además se detallará cada caso de uso con su diagrama de secuencia correspondiente.

### Gestión de instalación


<!--
For details on setting attributes like width and height, see:

-->

Una vez finalizada la instalación de la aplicación el administrador de la empresa debe terminar la configuración del sistema.

![Diagrama de casos de uso de Gestión de instalación \label{gestion_instalacion}](source/figures/gestion-instalacion.png){ width=50% }

#### Caso de uso: Finalizar instalación

* **Descripción**: Caso de uso para la primera vez que se accede al sistema.
* **Actores**: Administrador de la empresa.
* **Precondiciones**: La aplicación ha sido instalada.
* **Postcondiciones**: La aplicación queda configurada para su uso.
* **Escenario principal**:
    * El sistema muestra un formulario al usuario para que introduzca los datos.
    * El administrador introduce el nombre de la empresa, su email y su contraseña, que será la contraseña del administrador del sistema.
    * El sistema valida los datos y muestra al usuario un formulario para introducir usuarios junto con su rol de acceso.
    * El administrador repite el paso anterior hasta que termine de introducir usuarios
    * El sistema manda un mail a todos los usuarios introducidos para terminar su configuración
    * El sistema queda configurado.
* **Escenarios alternativos**:
    * Alguno de los datos introducidos es inválido.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * El administrador decide dejar el proceso de introducción de usuarios para más tarde.
        * El caso de uso termina.
    * El administrador introduce un email que ya ha introducido previamente.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.

### Gestión de usuarios

![Diagrama de casos de uso de Gestión de usuarios \label{gestion_usuarios}](source/figures/gestion-usuarios.png){ width=50% }

#### Caso de uso: Añadir usuario

* **Descripción**: Caso de uso para la creación de un usuario.
* **Actores**: Administrador de empresa.
* **Precondiciones**: El administrador se ha identificado correctamente en el sistema.
* **Postcondiciones**: Se crea un usuario con el perfil correspondiente.
* **Escenario principal**:
    * El administrador selecciona una empresa existente para añadir un usuario en ella.
    * El administrador introduce los datos del usuario y el nivel de privilegios.
    * El sistema valida que los datos son correctos y no hay ningún usuario con el mismo email.
    * El sistema crea el usuario y envía un mail al usuario para terminar la configuración
* **Escenarios alternativos**:
    * Alguno de los datos no es correcto.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * Ya existe algún usuario con el mismo email.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.

#### Caso de uso: Terminar configuración de usuario

* **Descripción**: El usuario registrado termina su configuración.
* **Actores**: Usuario
* **Precondiciones**: El usuario ha sido creado previamente por un administrador y el usuario ha recibido un email con un enlace.
* **Postcondiciones**: El usuario queda configurado y con acceso al sistema.
* **Escenario principal**:
    * El usuario abre el link que ha recibido por email.
    * El sistema muestra un formulario para configurar la contraseña y el resto de datos necesarios.
    * El usuario introduce los datos.
    * El sistema valida los datos.
    * El sistema guarda los datos y da acceso al usuario.
* **Escenarios alternativos**:
    * Alguno de los datos es incorrecto
        * El sistema lo indica y vuelve al paso anterior.


#### Caso de uso: Editar usuario

* **Descripción**: Caso de uso para la edición de un usuario.
* **Actores**: Usuario.
* **Precondiciones**: El usuario que se intenta editar coincide con el identificado en el sistema o bien el usuario identificado es un administrador.
* **Postcondiciones**: Se actualizan los datos del usuario.
* **Escenario principal**:
    * El sistema muestra los datos actuales del usuario.
    * El usuario modifica sus datos.
    * El sistema valida que los datos introducidos son correctos y no hay ningún otro usuario con el mismo email.
    * El usuario elige guardar los datos.
    * El sistema modifica el usuario.
* **Escenarios alternativos**:
    * Alguno de los datos no es correcto.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * Ya existe algún usuario con el mismo email.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.

#### Caso de uso: Ver usuario.

* **Descripción**: Caso de uso para seleccionar una empresa.
* **Actores**: Usuario del sistema.
* **Precondiciones**: El usuario tiene privilegios de usuario del sistema.
* **Postcondiciones**: Se muestran los datos del usuario.
* **Escenario principal**:
    * El usuario elige un usuario para ver sus datos.
    * El sistema muestra los datos del usuario.
    * El sistema muestra las aplicaciones a las que tiene acceso actualmente.


#### Caso de uso: Borrar usuario

* **Descripción**: Caso de uso para el borrado de un usuario
* **Actores**: Administrador.
* **Precondiciones**: El usuario identificado en el sistema tiene permisos para borrar usuarios o bien es administrador de la empresa del usuario a borrar.
* **Postcondiciones**: El usuario queda borrado
* **Escenario principal**:
    * El usuario selecciona un usuario para borrarlo.
    * El sistema muestra los datos actuales del usuario.
    * El usuario confirma que quiere borrar al usuario.
    * El sistema confirma el borrado.
* **Escenarios alternativos**:
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.


### Gestión de empresas

A continuación se especifican los casos de uso necesarios para llevar a cabo la gestión de las empresas clientes del sistema.

![Diagrama de casos de uso de Gestión de empresas \label{gestion_empresas}](source/figures/gestion-empresas.png){ width=50% }

#### Caso de uso: Añadir empresa

* **Descripción**: Caso de uso para añadir una empresa
* **Actores**: Administrador del sistema.
* **Precondiciones**: El usuario tiene nivel de administrador del sistema.
* **Postcondiciones**: La empresa queda registrada en el sistema
* **Escenario principal**:
    * El administrador introduce los datos de la empresa.
    * El sistema valida que los datos son correctos y no hay ninguna empresa con el mismo nombre.
    * El administrador selecciona los productos a los que tendrá acceso la empresa.
    * El sistema crea la empresa.
* **Escenarios alternativos**:
    * Alguno de los datos no es correcto.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * Ya existe alguna empresa con el mismo nombre.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.

#### Caso de uso: Editar empresa

* **Descripción**: Caso de uso para la edición de una empresa.
* **Actores**: Administrador del sistema.
* **Precondiciones**: El usuario tiene privilegios de administrador del sistema.
* **Postcondiciones**: Se actualizan los datos de la empresa
* **Escenario principal**:
    * El sistema muestra los datos actuales de la empresa
    * El usuario modifica sus datos.
    * El sistema valida que los datos introducidos son correctos y no hay ningún otro usuario con el mismo email.
    * El usuario modifica los productos a los que tiene acceso la empresa.
    * El usuario elige guardar los datos.
    * El sistema modifica la empresa.
* **Escenarios alternativos**:
    * Alguno de los datos no es correcto.
        * El sistema indica el error y el caso de uso vuelve al paso anterior.
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.

#### Caso de uso: Ver empresa.

* **Descripción**: Caso de uso para la edición de una empresa.
* **Actores**: Usuario del sistema.
* **Precondiciones**: El usuario tiene privilegios de usuario del sistema.
* **Postcondiciones**: Se muestran los datos de la empresa
* **Escenario principal**:
    * El usuario elige una empresa de las que tiene acceso.
    * El sistema muestra los datos actuales de la empresa.
    * El sistema muestra los usuarios de la empresa.
    * El sistema muestra los productos contratados por la empresa.

#### Caso de uso: Borrar empresa.

* **Descripción**: Caso de uso para el borrado de una empresa.
* **Actores**: Administrador.
* **Precondiciones**: El usuario identificado en el sistema tiene permisos para borrar empresas.
* **Postcondiciones**: La empresa queda borrada.
* **Escenario principal**:
    * El usuario selecciona una empresa para borrarla.
    * El sistema muestra los datos actuales de la empresa.
    * El usuario confirma que quiere borrar a la empresa.
    * El sistema confirma el borrado.
* **Escenarios alternativos**:
    * En cualquier momento el administrador decide cancelar el proceso.
        * El caso de uso finaliza.

### Gestión de productos

![Diagrama de casos de uso de Gestión de productos \label{gestion_productos}](source/figures/gestion-productos.png){ width=50% }

#### Caso de uso: Seleccionar producto

* **Descripción**: Caso de uso abstracto incluído en otros casos de uso para seleccionar una aplicación de una lista de disponibles
* **Actores**: Gestor de aplicaciones del sistema
* **Precondiciones**: El usuario identificado en el sistema es un gestor de aplicaciones.
* **Postcondiciones**: Se selecciona una aplicación para su uso en otra finalidad.
* **Escenario principal**:
    * El sistema muestra un listado de las aplicaciones disponibles.
    * El usuario selecciona la aplicación deseada.
* **Escenarios alternativos**:
    * No hay ninguna aplicación registrada.
        * El sistema indica el error y el caso de uso finaliza.

#### Caso de uso: Registrar aplicación

* **Descripción**: Registra una nueva aplicación en el sistema.
* **Actores**: Gestor de aplicaciones del sistema
* **Precondiciones**: El usuario identificado en el sistema es un gestor de aplicaciones.
* **Postcondiciones**: La aplicación queda registrada.
* **Escenario principal**:
    * El gestor introduce el código, el nombre y todos los demás datos de la aplicación.
    * El sistema comprueba que los datos cumplen el formato.
    * El sistema confirma el alta de la aplicación mostrando un mensaje.
* **Escenarios alternativos**:
    * Alguno de los datos introducidos tiene un formato incorrecto.
        * El sistema indica el error y se vuelve al paso anterior.
    * Falta algún campo obligatorio.
        * El sistema indica el error y se vuelve al paso anterior.
    * Ya existe alguna aplicación con ese código o nombre,
        * El sistema indica el error y se vuelve al paso anterior.
    * El gestor decide cancelar el registro en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Editar aplicación

* **Descripción**: Edita una aplicación existente en el sistema modificando sus datos
* **Actores**: Gestor de aplicaciones del sistema
* **Precondiciones**: El usuario identificado en el sistema es un gestor de aplicaciones.
* **Postcondiciones**: La aplicación queda modificada.
* **Escenario principal**:
    * Se realiza el caso de uso *seleccionar aplicación*
    * El sistema muestra sus datos actuales, permitiendo su edición.
    * El usuario modifica los datos.
    * El sistema comprueba que los datos son correctos.
    * El sistema confirma la modificación de la aplicación mostrando un mensaje.
* **Escenarios alternativos**:
    * Alguno de los datos introducidos tiene un formato incorrecto.
        * El sistema indica el error y se vuelve al paso anterior.
    * Falta algún campo obligatorio.
        * El sistema indica el error y se vuelve al paso anterior.
    * Ya existe alguna aplicación con ese código o nombre,
        * El sistema indica el error y se vuelve al paso anterior.
    * El gestor decide cancelar el registro en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Borrar aplicación

* **Descripción**: Borra una aplicación del sistema.
* **Actores**: Gestor de aplicaciones del sistema
* **Precondiciones**: El usuario identificado en el sistema es un gestor de aplicaciones.
* **Postcondiciones**: La aplicación queda eliminada.
* **Escenario principal**:
    * Se realiza el caso de uso *seleccionar aplicación*
    * El sistema muestra un diálogo de confirmación.
    * El usuario confirma que quiere eliminar la aplicación.
    * El sistema elimina la aplicación.
    * El sistema confirma la eliminación de la aplicación mostrando un mensaje.
* **Escenarios alternativos**:
    * El usuario selecciona que no desea eliminar la aplicación
        * El caso de uso se reinicia.
    * El gestor decide cancelar la eliminación en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Ver detalle de aplicación

* **Descripción**: Muestra los datos de una aplicación en detalle, así como sus credenciales.
* **Actores**: Gestor de aplicaciones del sistema
* **Precondiciones**: El usuario identificado en el sistema es un gestor de aplicaciones.
* **Postcondiciones**: Los datos de la aplicación se muestran por pantqalla
* **Escenario principal**:
    * Se realiza el caso de uso *seleccionar aplicación*
    * El sistema muestra los datos de la aplicación y sus credenciales.
* **Escenarios alternativos**:
    * El gestor decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza


#### Caso de uso: Editar usuarios de la empresa con acceso a la aplicación

* **Descripción**: Cambia los usuarios de una empresa que tienen acceso a la aplicación.
* **Actores**: Administrador de empresa.
* **Precondiciones**: El usuario identificado en el sistema es un administrador de una empresa.
* **Postcondiciones**: Los usuarios con acceso a la aplicación quedan modificados.
* **Escenario principal**:
    * Se realiza el caso de uso *seleccionar aplicación*
    * El usuario selecciona el rol que quiere editar.
    * El usuario selecciona los usuarios de su empresa a añadir a la  aplicación.
    * El usuario vuelve al paso 2 para seleccionar otro rol hasta que haya acabado con todos los roles.
    * El usuario selecciona guardar.
    * El sistema modifica los datos
* **Escenarios alternativos**:
    * El gestor decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

## Integraciones

![Diagrama de casos de uso de integraciones \label{gestion_integraciones}](source/figures/gestion-integraciones.png){ width=50% }

#### Caso de uso: Registrar aplicación externa

* **Descripción**: Registra una nueva aplicación externa en el sistema.
* **Actores**: Administrador de aplicaciones.
* **Precondiciones**: El usuario identificado en el sistema es un administrador de aplicaciones.
* **Postcondiciones**: La aplicación externa queda registrada.
* **Escenario principal**:
    * El usuario introduce los datos de la aplicación.
    * El sistema comprueba que son correctos.
    * El usuario introduce la url externa para hacer la integración.
    * El sistema prueba la conexión.
    * El sistema guarda los datos.
* **Escenarios alternativos**:
    * Los datos son incorrectos.
        * El sistema lo indica y vuelve al paso anterior.
    * La conexión con la url externa no se puede realizar.
        * El sistema lo indica y vuelve al paso anterior.
    * El gestor decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Obtener token de usuario desde aplicación interna

* **Descripción**: Un usuario obtiene un token a través de una aplicación interna.
* **Actores**: Usuario
* **Precondiciones**: El usuario existe en el sistema y tiene acceso a la aplicación.
* **Postcondiciones**: La aplicación recibe el token de usuario.
* **Escenario principal**:
    * La aplicación redirige al usuario a la web del sistema para identificarse.
    * El usuario se identifica en el sistema.
    * El sistema comprueba los datos son correctos.
    * El sistema comprueba que el usuario tiene acceso a la aplicación.
    * El sistema devuelve el token a la aplicación.
* **Escenarios alternativos**:
    * Las credenciales son incorrectas.
        * El sistema lo indica y vuelve al paso anterior.
    * El usuario identificado no tiene acceso a la aplicación.
        * El sistema lo indica y el caso de uso termina
    * El usuario decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Obtener token de usuario desde apliación externa

* **Descripción**: Un usuario obtiene un token a través de una aplicación externa.
* **Actores**: Usuario
* **Precondiciones**: El usuario existe en el sistema y tiene acceso a la aplicación.
* **Postcondiciones**: La aplicación recibe el token de usuario.
* **Escenario principal**:
    * La aplicación redirige al usuario a la web del sistema para identificarse.
    * El usuario se identifica en la aplicación.
    * El sistema redirige a la web de la aplicación externa.
    * El usuario se identifica en la aplicación externa.
    * El sistema devuelve el token a la aplicación.
* **Escenarios alternativos**:
    * Las credenciales son incorrectas.
        * El sistema lo indica y vuelve al paso anterior.
    * El usuario identificado no tiene acceso a la aplicación.
        * El sistema lo indica y el caso de uso termina
    * El usuario decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Crear regla de acceso de usuario

* **Descripción**: Se añade una regla para controlar las conexiones del usuario
* **Actores**: Administrador de empresa
* **Precondiciones**: El usuario existe en el sistema
* **Postcondiciones**: La regla queda creada y se aplica en cada acceso del usuario
* **Escenario principal**:
    * El administrador entra en el menú de crear reglas de usuario
    * El sistema muestra el formulario para crear reglas
    * El administrador añade nombre, selecciona operador y añade los parámetros
    * El sistema valida los datos
    * La regla queda aplicada.
* **Escenarios alternativos**:
    * Las credenciales son incorrectas.
        * El sistema lo indica y vuelve al paso anterior.
    * El usuario identificado no tiene acceso al usuario
        * El sistema lo indica y el caso de uso termina
    * Algún parámetro es incorrecto
        * El sistema lo indica y el caso de uso vuelve al paso anterior
    * El usuario decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Crear regla de acceso de empresa

* **Descripción**: Se añade una regla para controlar las conexiones de los usuarios de la empresa
* **Actores**: Administrador de empresa
* **Precondiciones**: El usuario existe en el sistema
* **Postcondiciones**: La regla queda creada y se aplica en cada acceso de los usuarios de la empresa
* **Escenario principal**:
    * El administrador entra en el menú de crear reglas de usuario
    * El sistema muestra el formulario para crear reglas
    * El administrador añade nombre, selecciona operador y añade los parámetros
    * El sistema valida los datos
    * La regla queda aplicada.
* **Escenarios alternativos**:
    * Las credenciales son incorrectas.
        * El sistema lo indica y vuelve al paso anterior.
    * El usuario identificado no tiene acceso al usuario
        * El sistema lo indica y el caso de uso termina
    * Algún parámetro es incorrecto
        * El sistema lo indica y el caso de uso vuelve al paso anterior
    * El usuario decide cancelar el proceso en cualquier momento
        * El caso de uso finaliza

#### Caso de uso: Importar usuarios desde módulo externo

* **Descripción**: Se importan usuarios desde módulo externo
* **Actores**: Administrador de empresa
* **Precondiciones**: El usuario existe en el sistema y es administrador de una empresa
* **Postcondiciones**: Los usuarios quedan cargados en la aplicación
* **Escenario principal**:
    * El usuario selecciona el método de importación.
    * El usuario carga los datos siguiendo el método adecuado.
    * El sistema registra los usuarios en el sistema.

#### Caso de uso: Sincronizar usuarios con módulo externo
* **Descripción**: Se sincroniza con una api externa para cargar los usuarios.
* **Actores**: Administrador de empresa
* **Precondiciones**: El usuario existe en el sistema y es administrador de una empresa
* **Postcondiciones**: Los usuarios quedan cargados en la aplicación
* **Escenario principal**:
    * El usuario selecciona el método de importación.
    * El usuario introduce las apis necesarias con las credenciales.
    * El sistema sincroniza con la api externa.

<!--
Figures can be added with the following syntax:
![my_caption \label{my_label}](source/figures/my_image.pdf){ width=50% }

For details on setting attributes like width and height, see:
http://pandoc.org/MANUAL.html#extension-link_attributes
-->

## Modelo Conceptual de datos

![Diagrama de clases conceptual \label{diagrama_clases_conceptual}](source/figures/diagrama-clases-conceptual.png)

## Modelo de comportamiento del sistema

Para el modelo de comportamiento del sistema se mostrarán diferentes diagramas de secuencia del sistema. El diagrama define las interacciones entre actores y sistema, también se detallarán los contratosde las operaciones del sistema, para describir en detalle qué hace cada operación.

Al existir muchos casos de uso similares, sólo se detallarán los más relevantes de cada subsistema.

### Caso de uso: Añadir usuario

![Caso de uso: Añadir usuario \label{secuencia_anadir_usuario}](source/figures/secuencia-anadir-usuario.png)

#### Contrato de la operación "introducir datos de usuario"

* **Responsabilidades**: Registrar usuario en el sistema
* **Referencias cruzadas**: Caso de uso *editar usuario*. Caso de uso *añadir usuario*
* **Precondiciones**: No existe un usuario con email = w_email
* **Postcondiciones**: Se crea una instancia de usuario U, se asignan w_email y datos.

### Caso de uso: Editar usuario

![Caso de uso: Editar usuario \label{secuencia_editar_usuario}](source/figures/secuencia-editar-usuario.png)

#### Contrato de la operación "seleccionar usuario"

* **Responsabilidades**: Abrir página de usuario
* **Referencias cruzadas**: Caso de uso *Ver usuario*. Caso de uso *editar usuario*
* **Precondiciones**: El usuario está creado en la empresa y el usuario logueado tiene permisos para verlo.
* **Postcondiciones**: Se muestra la página del usuario.

### Caso de uso: Añadir empresa

![Caso de uso: Añadir empresa \label{secuencia_anadir_empresa}](source/figures/secuencia-anadir-empresa.png)

#### Contrato de la operación "introducir datos de empresa"

* **Responsabilidades**: Registrar empresa en el sistema
* **Referencias cruzadas**: Caso de uso *añadir empresa*. Caso de uso *editar empresa*
* **Precondiciones**: No existe una empresa con code = w_code
* **Postcondiciones**: Se crea una instancia de empresa E, se asignan w_code y datos.

### Caso de uso: Editar empresa

![Caso de uso: Editar empresa \label{secuencia_editar_empresa}](source/figures/secuencia-editar-empresa.png)

#### Contrato de la operación "seleccionar empresa"

* **Responsabilidades**: Abrir página de empresa
* **Referencias cruzadas**: Caso de uso *Ver empresa*. Caso de uso *añadir usuario*. Caso de uso *editar usuario*. Caso de uso *editar empresa*. Caso de uso *editar usuarios con acceso a aplicación*.
* **Precondiciones**: La empresa existe en el sistema
* **Postcondiciones**: Se muestra la página de la empresa con el listado de usuarios.

### Caso de uso: Añadir aplicación

![Caso de uso: Añadir aplicacion \label{secuencia_anadir_aplicacion}](source/figures/secuencia-anadir-aplicacion.png)

#### Contrato de la operación "introducir datos de aplicación"

* **Responsabilidades**: Registrar aplicación en el sistema
* **Referencias cruzadas**: Caso de uso *añadir aplicación*. Caso de uso *editar aplicación*
* **Precondiciones**: No existe una aplicación con code = w_code
* **Postcondiciones**: Se crea una instancia de aplicación A, se asignan w_code y datos, se generan credenciales.

### Caso de uso: Editar aplicación

![Caso de uso: Editar aplicacion \label{secuencia_editar_aplicacion}](source/figures/secuencia-editar-aplicacion.png)

#### Contrato de la operación "seleccionar aplicación"

* **Responsabilidades**: Abrir página de empresa
* **Referencias cruzadas**: Caso de uso *Ver aplicación*. Caso de uso *editar aplicación*. Caso de uso *borrar aplicación*
* **Precondiciones**: La aplicación existe en el sistema
* **Postcondiciones**: Se muestra la página de la aplicación.

### Caso de uso: Borrar aplicación

![Caso de uso: Borrar aplicacion \label{secuencia_borrar_aplicacion}](source/figures/secuencia-borrar-aplicacion.png)

#### Contrato de la operación "borrar aplicación"

* **Responsabilidades**: Borra aplicación en el sistema
* **Referencias cruzadas**: Caso de uso *borrar aplicación*.
* **Precondiciones**: Existe una aplicación con code = w_code
* **Postcondiciones**: Se elimina la instancia de aplicación A. Se borran todos los accesos existentes para la aplicación.

### Caso de uso: Editar usuarios de empresa con acceso a aplicación

![Caso de uso: Editar acceso de usuarios de empresa \label{secuencia_editar_usuarios_empresa_aplicacion}](source/figures/secuencia-editar-usuarios-empresa-aplicacion.png)

### Caso de uso: Obtener token de aplicación interna

![Caso de uso: Obtener token de aplicación interna \label{secuencia_obtener_token}](source/figures/secuencia-obtener-token.png)

#### Contrato de la operación "obtener token de acceso"

* **Responsabilidades**: Genera un token para el usuario logueado.
* **Referencias cruzadas**: Caso de uso *Obtener token de aplicación interna*.
* **Precondiciones**: El usuario tiene acceso a la aplicación.
* **Postcondiciones**: Se genera un token de acceso para el usuario.

