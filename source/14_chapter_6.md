# Implementación

Llegada la etapa de implementación, hay que transformar a código lo analizado y diseñado en etapas anteriores. La idea original era hacer una aplicación web, esta aplicación estaría separada en dos subsistemas, uno de administración y otro de integración con aplicaciones.

## Subsistema de administración

Este subsistema consiste en una aplicación web con diferentes paneles para gestionar cada una de las partes del sistema, usuarios, empresas, etc.

A este subsistema tendrán acceso los administradores de la empresa principal con licencia del software, cada usuario tendrá visibilidad limitada según sus permisos. Empresas externas clientes de la empresa licenciada podrán tener acceso pero con visibilidad muy limitada, solo para gestionar los usuarios propios y el acceso a las aplicaciones que tenga contratadas.

### Lenguajes

Para esta sección se ha utilizado *Python* como lenguaje principal, usando *Django* como framework de desarrollo web. La razón por la que se eligen *Python* y *Django* es por la actual experiencia del autor del proyecto en estos lenguajes, además de existir una comunidad bastante grande alrededor de ellos, con una documentación amplia disponible.

*Python* es un lenguaje fácil de aprender y con parecido al lenguaje natural, por lo que facilita la labor a la hora de hacer un proyecto de estas características, *Django* además provee de diferentes herramientas para abstraer el desarrollo de una aplicación web e implementar solo la lógica de negocio.

*Django* sigue una variación del patrón MVC, en el cual separa la capa de modelo de datos del resto, en esta capa está la lógica del dominio del negocio, aunque se puede separar en más subniveles. En lugar del clásico controlador, *Django* usa el concepto de Vista, que son funciones que reciben la petición del servidor y devuelven una respuesta acorde utilizando la lógica de los modelos. El equivalente a la vista del MVC sería en este caso el sistema de plantillas, plantillas que dado el caso la View de *Django* renderiza con los datos necesarios.

Para la interfaz se ha aprovechado la librería *django-admin* que autogenera formularios para los modelos implementados, además sobre esta se utiliza una capa llamada *django-suit* para mejorar el resultado de la interfaz que se mostrará al usuario, esto ahorra trabajo de maquetación y diseño, trabajos que escapan al alcance de este proyecto.


## Subsistema de integración

Este subsistema consiste en una API REST para que las aplicaciones puedan interactuar con el sistema central, este subsistema pone a disposición de la aplicación tokens de acceso con los cuales puede acceder a la API y realizar diferentes labores, además de identificar al usuario.

### Lenguajes

Para este subsistema se ha utilizado también *Python* y *Django* pero con el añadido de usar *django-rest-framework* como capa por encima de *Django*.

Este framework nos proporciona varias herramientas para construir una API REST con *Django*, como son los serializadores y las vistas genéricas.

### Herramientas utilizadas

Para el desarrollo del proyecto se hace necesario el uso de una serie de herramientas, como editores de código, sistemas de control de versiones, etc.

A continuación se detallarán todas las herramientas usadas en este proyecto.

La herramienta principal que se ha utilizado ha sido un IDE, en este caso *PyCharm*. *PyCharm* es un entorno escrito en Java y pensado en un principio para desarrollar proyectos Python, es una evolución de *IntellijIdea*, el cual es una herramienta para desarrollar *Java*, pero conforme ha avanzado el tiempo se ha ido ampliando a más lenguajes, como por ejemplo Python.
La integración con este último es perfecta, proporcionando útiles herramientas como el autocompletado.

Para la detección de errores se hace casi obligado el uso de un debugger, en este caso hemos usado el debugger integrado de PyCharm, permitiendo utilizar puntos de ruptura en el código para comprobar el estado del sistema en un momento dado.

Para el despliegue de la aplicación se ha utilizado un entorno compuesto por un servidor Nginx, base de datos MySQL y el intérprete de *Python*, todo ello sobre un sistema GNU/Linux.

Otra herramienta utilizada que facilita el trabajo enormemente ha sido Git. Git es un sistema de control de versiones que facilita el desarrollo colaborativo y el mantenimiento de un software, versionando todos los cambios que se vayan produciendo en el código. Esto permite que si queremos volver a una versión anterior del sistema podamos hacerlo sin problema alguno, además de la creación de ramas de desarrollo, pudiendo fusionar ramas sin problema alguno.

## Detalles de implementación de la arquitectura del sistema

### Capa modelo

Como hemos dicho se ha utilizado *Django* como framework de desarrollo, *Django* trae integrado un ORM que abstrae el acceso a base de datos, funcionando de forma agnóstica con cualquier SGBD. Para construir un modelo definimos sus atributos de clase, además de sus relacione, utilizando los modelos que nos proporciona *Django*. *Django* a su vez, utilizando el comando `make migrations` generará las migraciones correspondientes que una vez ejecutadas con `migrate` se aplicarán en la base de datos y crearán las tablas necesarias.

Un ejemplo de modelo es el siguiente:

```{.python}

class Account(models.Model):
    objects = AccountManager()

    EMAIL_FIELD_MAX_LENGTH = 255

    id = models.AutoField(primary_key=True,
                          help_text="the account id, e.g. 1234")
    uid = UUIDField(default=get_uuid,
                    help_text="A unique uuid string")
    enabled = models.BooleanField(default=True,
                                  help_text=("A flag that indicates if the account is enabled or "
                                             "not. A disabled account cannot log in the system and"
                                             "will not receive notifications."))
    email = models.EmailField(max_length=EMAIL_FIELD_MAX_LENGTH,
                              help_text="Account email, e.g. example@gmail.com")
    password = models.CharField(max_length=56,
                                help_text="The account holder's password.")
    name = models.CharField(max_length=40,
                            help_text="The account holder's name.")

```

Una capa intermedia de los modelos es la capa de Model Managers, esta capa permite abstraer diferentes consultas a la base de datos para no repetirlas cada vez.

Un ejemplo sería el siguiente:

```{.python}
class AccountManager(models.Manager):
    @transaction.atomic
    def create(self, **kwargs):
        account = super(AccountManager, self).create(**kwargs)
        account.groups.add(self.default_group)
        return account

    @cached_property
    def default_group(self):
        return Group.objects.get_by_natural_key('default')

    def get_by_id_or_uid(self, uid):
        if isinstance(uid, int) or uid.isdigit():
            params = {'pk': uid}
        elif len(uid) == 32:
            params = {'uid': uid}
        else:
            from api.models import Account
            raise Account.DoesNotExist()

        return self.get(**params)
```

En cuanto a las migraciones, son sentencias que permiten versionar los cambios en la base de datos, un ejemplo sería el siguiente:


```{.python}

class Migration(migrations.Migration):

    dependencies = [
    ]

    operations = [
        migrations.CreateModel(
            name='Account',
            fields=[
                ('id', models.AutoField(serialize=False, primary_key=True, db_column=b'ac_id')),
                ('enabled', models.BooleanField(default=True, db_column=b'ac_enabled')),
                ('email', models.EmailField(max_length=255, db_column=b'ac_email')),
                ('password', models.CharField(max_length=56, db_column=b'ac_password')),
                ('name', models.CharField(max_length=40, db_column=b'ac_name')),
                ('surname', models.CharField(default=b'', max_length=40, db_column=b'ac_surname', blank=True)),
            ],
            options={
                'db_table': 'Account',
            },
        ),

```

<!--
Comments can be added like this.
-->

### Capa View (Controlador)

La capa controlador es la que se encarga de recibir las peticiones de la api y devolver una respuesta adecuada, es la capa de interfaz al exterior por lo que es bastante importante.

El controlador se compone de un Router y las Vistas, el Router se encarga de parsear las urls y pasar la petición a la vista correspondiente, un ejemplo de router sería el siguiente:

```{.python}
urlpatterns = [
    url(
        regex=r'^$',
        view=views.UserListView.as_view(),
        name='list'
    ),
    url(
        regex=r'^~redirect/$',
        view=views.UserRedirectView.as_view(),
        name='redirect'
    ),
    url(
        regex=r'^companies/(?P<slug>[\w.@+-]+)/users/(?P<username>[\w.@+-]+)/$',
        view=views.UserDetailView.as_view(),
        name='detail'
    ),
    url(
        regex=r'^~update/$',
        view=views.UserUpdateView.as_view(),
        name='update'
    ),
]
```

Las vistas genéricas de *django-rest-framework* se componen de varios atributos para configurarlas, permitiendo generar una respuesta sin escribir apenas código, un ejemplo sería el siguiente:

```{.python}

class AccountDetailView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Account.objects.all()
    serializer_class = AccountSerializer
    permission_classes = (UserCanManagePermission,)
    lookup_field = 'id'
    lookup_url_kwarg = 'account_id'
```

La vista del ejemplo genera acciones para el GET en detalle, para el PUT y para el DELETE, utilizando el queryset configurado.

### Capa Templates (Vista)

Para esta capa hemos utilizado *django-admin* con la capa de *django-suit*, las vistas se configuran mediante código Python y en parte se autogeneran a partir de los modelos.

Ejemplo:

```{.python}
class AccountAdmin(DjangoObjectActions, admin.ModelAdmin):
    model = Account
    list_display = ('id', 'name', 'enabled')
    list_filter = ['enabled']
    search_fields = ['name']

    readonly_fields = []

    def get_readonly_fields(self, request, obj=None):
        return [f.name for f in self.model._meta.fields]
```
