# Costes y monetización

La estimación de costes de desarrollo de un proyecto software es un factor muy importante en el análisis de un proyecto, es parte fundamental encontrar métricas para cuantificar el coste del mismo, garantizando calidad y eficiencia. Por tanto podemos definir el análisis de coste como el proceso de identificación de los recursos necesarios para llevar a cabo el trabajo eficientemente.

## Estimación

Estimar el coste o tiempo que vamos a necesitar para desarrollar una funcionalidad no es fácil. Hay diversos factores a tener en cuenta, entre ellos el riesgo, la incertidumbre y la dificultad del trabajo en sí. Cuanta más incertidumbre más complicado será dar una estimación con precisión sobre el trabajo a realizar, ya que en muchas ocasiones se comprende mejor el problema a medida que se avanza.

Para ello en este proyecto se ha decidido dar una estimación por días de trabajo para cada funcionalidad, y ante la imposibilidad de dar una estimación exacta se ha seguido la aproximación de dar un rango de tiempo, siendo el rango de tiempo más amplio cuanta más incertidumbre hay.

En cuanto a los costes, no tiene sentido hablar aquí de costes a facturar cuando la aplicación está pensada para ser monetizada ofreciéndola como servicio, no como un producto a medida. Pero de este tema hablaremos en una sección posterior.

### Estimación por Caso de Uso

En el caso de este proyecto se han decidido dar estimaciones por caso de uso, ya que es la unidad más pequeña que tenemos para trabajar en el software. Por tanto a continuación se darán estimaciones por caso de uso en valor de días/persona de trabajo.

* Caso de uso Finalizar Instalación: 3-5 días
	* Crear formulario.
* Caso de uso Añadir usuario: 5-8 días
	* Requiere crear los modelos y toda la base del usuario.
* Caso de uso Terminar configuración de usuario: 2-3 días.
	* Misma base de añadir usuario.
* Caso de uso Editar usuario: 2-3 días
	* Misma base de añadir usuario.
* Caso de uso Ver usuario: 2-3 días
	* Solo interfaz.
* Caso de uso Borrar usuario: 1-2 días
	* Interfaz sencilla.
* Caso de uso Añadir empresa: 5-8 días
	* Crear modelos y base de empresa.
* Caso de uso Editar empresa: 2-3 días.
	* Misma base de añadir empresa.
* Caso de uso Ver empresa: 2-3 días.
	* Misma base.
* Caso de uso Borrar empresa: 1-2 días
	* Interfaz sencilla.
* Caso de uso Registrar aplicación: 3-5 días
	* Crear modelos.
* Caso de uso Editar aplicación: 2-3 días
	* Misma base.
* Caso de uso Borrar aplicación: 1-2 días
	* Interfaz sencilla.
* Caso de uso Ver detalle de aplicación: 2-3 días.
	* Solo interfaz.
* Caso de uso Editar usuarios de la empresa con acceso a la aplicación: 5-8 días
	* Interfaz y modelado complejo.
* Caso de uso Registrar aplicación externa: 3-5 días
	* Modelar
* Caso de uso Obtener token de usuario desde aplicación interna: 5-8 días
	* Montar el OAuth provider de base.
* Caso de uso Obtener token de usuario desde aplicación externa: 3-5 días
	* Nuevo flujo de OAuth.
* Caso de uso Importar usuarios desde módulo externo: 8-13 días
	* Hace falta investigación.
* Caso de uso Sincronizar usuarios con módulo externo: 8-13 días
	* Hace falta investigación.

Teniendo en cuenta estas estimaciones, finalmente obtenemos una estimación total para el periodo de desarrollo del proyecto de entre 65 y 105 días de trabajo, o lo que es lo mismo entre 2 y 3.5 meses.

## Monetización

Otro aspecto relacionado con la estimación de costes es el modelo de monetización del software una vez que este esté desarrollado. Tener claro este modelo antes de empezar a desarrollar es importante para asegurar el retorno de la inversión. Este es un proyecto pequeño de tipo académico pero viene bien para analizar este tipo de cuestiones. Es obvio que con sin un modelo de negocio pensado de antemano asumimos el riesgo de no recuperar la inversión inicial.

Este proyecto, al estar pensado para ser ofrecido como servicio, necesita tener un modelo de negocio para ser monetizado, ya que no se venderá a medida a un cliente.

Al ser un proyecto abierto a modificaciones y a estar en contacto con el cliente, el mejor modelo para este tipo de software responde al de licencias, es decir, vender una licencia de uso al cliente con ciertos límites, siendo esta licencia pagada de forma periódica. La licencia a vender responderá a las necesidades del cliente, pero básicamente hay dos variables que se modificarán dependiendo de las necesidades y del tamaño del cliente: número de aplicaciones y número de empresas cliente.

Así, en función de estas dos variables se cobrará más o menos al cliente final, por tanto podríamos partir de paquetes predefinidos para tamaños específicos de cliente y tener una opción a medida para un cliente, siendo ofrecido un precio negociado en este caso.
