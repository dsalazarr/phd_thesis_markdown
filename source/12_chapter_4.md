# Análisis

## Metodología de desarrollo

Para la realización del proyecto y su documentación se ha utilizado el Rational Unified Process (RUP), junto con el Lenguaje Unificado de Modelado (UML). Se ha elegido este sistema ya que es la metodología estándar más utilizada, además de ser un grupo de metodologías que se adaptan muy bien a las necesidades de un producto.

## Especificación de requisitos del sistema

A continuación se enumeran los requisitos funcionales que se consideran fundamentales para el sistema. Éstos serán detallados utilizando casos de uso, describiendo tanto su escenario principal como sus posibles flujos alternativos. Además se detallará cada caso de uso con su diagrama de secuencia correspondiente.

### Gestión de usuarios

**Caso de uso: Añadir usuario**
* **Descripción**: Caso de uso para la creación de un usuario.
* **Actores**: Administrador.
* **Precondiciones**: El administrador se ha identificado correctamente en el sistema.
* **Postcondiciones**: Se crea un usuario con el perfil correspondiente.
* **Escenario principal**:
** El administrador introduce los datos del usuario y el nivel de privilegios.
** El sistema valida que los datos son correctos y no hay ningún usuario con el mismo email.
** El sistema crea el usuario y envía por correo el password al usuario.
* Escenarios alternativos:
** Alguno de los datos no es correcto.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** Ya existe algún usuario con el mismo email.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** En cualquier momento el administrador decide cancelar el proceso.
*** El caso de uso finaliza.

**Caso de uso: Editar usuario**
* **Descripción**: Caso de uso para la edición de un usuario.
* **Actores**: Usuario.
* **Precondiciones**: El usuario que se intenta editar coincide con el identificado en el sistema o bien el usuario identificado es un administrador.
* **Postcondiciones**: Se actualizan los datos del usuario.
* **Escenario principal**:
** El sistema muestra los datos actuales del usuario.
** El usuario modifica sus datos.
** El sistema valida que los datos introducidos son correctos y no hay ningún otro usuario con el mismo email.
** El usuario elige guardar los datos.
** El sistema modifica el usuario.
* Escenarios alternativos:
** Alguno de los datos no es correcto.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** Ya existe algún usuario con el mismo email.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** En cualquier momento el administrador decide cancelar el proceso.
*** El caso de uso finaliza.

**Caso de uso: editar usuario**
* **Descripción**: Caso de uso para la edición de un usuario.
* **Actores**: Usuario.
* **Precondiciones**: El usuario que se intenta editar coincide con el identificado en el sistema o bien el usuario identificado es un administrador.
* **Postcondiciones**: Se actualizan los datos del usuario.
* **Escenario principal**:
** El sistema muestra los datos actuales del usuario.
** El usuario modifica sus datos.
** El sistema valida que los datos introducidos son correctos y no hay ningún otro usuario con el mismo email.
** El usuario elige guardar los datos.
** El sistema modifica el usuario.
* Escenarios alternativos:
** Alguno de los datos no es correcto.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** Ya existe algún usuario con el mismo email.
*** El sistema indica el error y el caso de uso vuelve al paso anterior.
** En cualquier momento el administrador decide cancelar el proceso.
*** El caso de uso finaliza.



### Subsection 2

This is the second part of the methodology. Sed ut ipsum ultrices, interdum ipsum vel, lobortis diam. Curabitur sit amet massa quis tortor molestie dapibus a at libero. Mauris mollis magna quis ante vulputate consequat. Integer leo turpis, suscipit ac venenatis pellentesque, efficitur non sem. Pellentesque eget vulputate turpis. Etiam id nibh at elit fermentum interdum.

<!-- 
Comments can be added like this.
--> 

## Results

These are the results. In vitae odio at libero elementum fermentum vel iaculis enim. Nullam finibus sapien in congue condimentum. Curabitur et ligula et ipsum mollis fringilla.

## Discussion

Figure \ref{ref_a_figure} shows how to add a figure. Donec ut lacinia nibh. Nam tincidunt augue et tristique cursus. Vestibulum sagittis odio nisl, a malesuada turpis blandit quis. Cras ultrices metus tempor laoreet sodales. Nam molestie ipsum ac imperdiet laoreet. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.

<!-- 
Figures can be added with the following syntax:
![my_caption \label{my_label}](source/figures/my_image.pdf){ width=50% }

For details on setting attributes like width and height, see:
http://pandoc.org/MANUAL.html#extension-link_attributes
--> 

![RV Calypso is a former British Royal Navy minesweeper converted into a research vessel for the oceanographic researcher Jacques-Yves Cousteau. It was equipped with a mobile laboratory for underwater field research. \label{ref_a_figure}](source/figures/example_figure.pdf){ width=100% }

## Conclusion

This is the conclusion to the chapter. Quisque nec purus a quam consectetur volutpat. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. In lorem justo, convallis quis lacinia eget, laoreet eu metus. Fusce blandit tellus tellus. Curabitur nec cursus odio. Quisque tristique eros nulla, vitae finibus lorem aliquam quis. Interdum et malesuada fames ac ante ipsum primis in faucibus.



