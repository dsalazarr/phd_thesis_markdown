# Pruebas

Todo software que se precie requiere de una suite de pruebas que asegure la calidad del software. En muchas ocasiones se tiende a usar una metodología de desarrollo TDD (Test Driven Development), en la cual se escriben primero los tests para que fallen y a continuación se va implementando el software para hacer pasar esos tests fallidos. Esta metodología de desarrollo facilita pensar primero en la solución y el objetivo final a implementar, además de desarrollar una solución rápida, además permite tener una base sobre la que refactorizar luego el código de forma segura. TDD es una metodología útil, pero no debería de ser el único marco de desarrollo que utilizáramos para asegurar la calidad del software. Para ello se ha de hacer primero un plan de pruebas que asegure la calidad de todo el proceso de creación del mismo, desde el análisis hasta la implementación final.

Este documento pretende generar un plan de pruebas acorde a la documentación generada en las secciones anteriores, revisándo de esta forma los pasos anteriores y actualizando lo que no tuviera sentido o lo que faltara por contemplar. Para ello distinguiremos entre las diferentes categorías de pruebas, desde las *unitarias* hasta las de *aceptación*, entrando más en detalle en las últimas, ya que son las que asegurarán la calidad de los diferentes casos de uso. Estas pruebas deberán llevarse a cabo de forma automática, y lanzarse previamente a una release de software para asegurar que esta release cumple con los requisitos de aceptación previamente elicitados. El resultado de este informe debería ser revisado por el equipo de calidad (Quality Assurance) de forma que nos aseguremos que los requisitos se cumplen. El documento de aceptación debería ser generado conjuntamente también con este equipo, de forma que su trabajo pase por todas las etapas del ciclo de vida del software, no solo al final como se hacía clásicamente.

## Test Driven Development

Test-driven development es un proceso de desarrollo de software ideado por Kent Beck que basa el desarrollo en la repetición de un ciclo corto del mismo. De esta forma los requisitos se transforman en casos de prueba muy específicos, estos casos se implementan y el software se va mejorando para pasar estos tests solamente, sin añadir ningún tipo de lógica más. Este sistema se contrapone al clásico que permitía añadir código sin pruebas para que estas fueran añadidas a posteriori.

Kent Beck afirmó que TDD incentiva diseños simples e inspira confianza en el software. Esta confianza crece ya que una vez implementado un caso de uso con unos tests que nos aseguran que funciona, podemos refactorizar sin miedo alguno de romper nada, ya que en el momento que algo se rompa los tests nos lo indicarán.

Esta metodología nace de los conceptos de programación extrema, metodología que nació en 1999, adquiriendo en los últimos años entidad propia.

## Pruebas unitarias

Una parte importante del desarrollo de software son las pruebas unitarias, también conocidas como pruebas de caja negra, este tipo de pruebas prueban piezas de software de forma aislada, asegurando que con una entrada concreta recibimos la salida que queremos, en nuestro proyecto hemos querido limitar este tipo de pruebas, ya que a pesar de ser importantes, no aseguran el funcionamiento de forma integrada de todas las piezas, consideramos más importantes los tests de aceptación que prueban el software de punto a punto.

Aun así para piezas con cierta complejidad se hace casi obligatorio escribir este tipo de tests automáticos, estos tests pueden actuar además como documentación de esas piezas de software complejas, de forma que otros desarrolladores comprendan mejor su funcionamiento.

## Pruebas de aceptación

La prueba de aceptación es la última de las pruebas que debe atravesar una aplicación en un plan de QA, es la que prueba el software desde el punto de vista del usuario y prueba la integración de todas las piezas del sistema. Si la especificación es completa y las pruebas están pensadas, éstas deberían ser suficientes para cubrir casi todos los casos en los que un usuario dará uso al software.

Normalmente para las pruebas de aceptación se trabaja sobre historias de usuario, que son casos de uso definidos desde el punto de vista del usuario, para desarrollar este plan de pruebas partiremos de los casos de uso elicitados en el capítulo de análisis.

### Caso de uso Añadir usuario

* Historia de usuario: *Yo como usuario administrador de mi empresa X, quiero poder añadir un usuario en el sistema dentro de mi empresa.*
* Pruebas de aceptación:
    * Si el usuario identificado no tiene rol de administrador se devolverá un error en el que se indique que no hay permisos.
    * Si el usuario envía algún dato incorrecto se devolverá un error indicando de forma detallada el error.
    * Si el usuario envía un email ya existente se devolverá un error indicándolo.
    * Una vez finalizado el proceso el usuario aparece en la lista de usuarios inactivos de la empresa.
    * Una vez finalizado el proceso el usuario recibe un email para terminar el registro.

### Caso de uso Terminar configuración de usuario

* Historia de usuario: *Yo como usuario recién reigstrado quiero terminar el proceso de configuración para activar mi usuario*
* Pruebas de aceptación:
    * Una vez finalizado el proceso el usuario aparece en el listado de usuarios activos de la empresa.
    * Si alguno de los datos enviados es incorrectos el sistema debe indicar el error de forma detallada.

### Caso de uso Editar usuario

* Historia de usuario: *Yo como usuario X administrador de mi empresa, quiero poder editar un usuario Y diferente de mi en el sistema dentro de mi empresa.*
* Pruebas de aceptación:
    * Si el usuario Y pertenece a una empresa diferente se devolverá un error en el que se indique que no hay permisos o que el usuario no existe dentro de la empresa.
    * Si el usuario identificado no tiene rol de administrador se devolverá un error en el que se indique que no hay permisos.
    * Si el usuario envía algún dato incorrecto se devolverá un error indicando de forma detallada el error.
    * Si el usuario envía un email ya existente se devolverá un error indicándolo.
    * Una vez finalizado el proceso, en la vista del usuario actualizado aparecerán los datos modificados actualizados.

* Historia de usuario: *Yo como usuario de mi empresa X, quiero poder editar mi contraseña en el sistema*
* Pruebas de aceptación:
    * Si la contraseña no tiene el nivel de seguridad mínimo se devolverá un error indicándolo.
    * Una vez finalizado el proceso, la contraseña del usuario quedará actualizada y podrá crear tokens con la nueva contraseña, también sus tokens anteriores quedarán invalidados.

### Caso de uso Ver usuario

* Historia de usuario: *Yo como usuario X administrador de mi empresa, quiero poder ver los datos de un usuario Y diferente de mi perteneciente a mi misma empresa.*
* Pruebas de aceptación:
    * Si el usuario Y pertenece a una empresa diferente se devolverá un error en el que se indique que no hay permisos o que el usuario no existe dentro de la empresa.
    * Si el usuario identificado no tiene rol de administrador se devolverá un error en el que se indique que no hay permisos.
    * Si todo es correcto se devolverán los datos del usuario excluyendo su contraseña.

* Historia de usuario: *Yo como usuario X, quiero poder ver mis datos.*
* Pruebas de aceptación:
    * Si el usuario solicitado es diferente al identificado se devolverá un error en el que se indique que no hay permisos o que el usuario no existe dentro de la empresa.
    * Si todo es correcto se devolverán los datos del usuario excluyendo su contraseña.


### Caso de uso Borrar usuario

* Historia de usuario: *Yo como usuario X, administrador de mi empresa, quiero poder borrar un usuario Y diferente a mi.*
* Pruebas de aceptación:
    * Si el usuario solicitado es X, no se permitirá continuar por seguridad, primero otro administrador deberá quitarle el privilegio de administrador y borrarle.
    * Si el usuario identificado no tiene rol de administrador se devolverá un error en el que se indique que no hay permisos.
    * Si el proceso es correcto el usuario Y dejará de aparecer en la lista de usuarios activos de la empresa.
