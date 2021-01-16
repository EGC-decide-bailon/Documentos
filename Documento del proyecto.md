# Documento del Proyecto

# Decide-Single-Bailón- Cabina





- Grupo: 2.
- ID de opera: - 
-	Curso escolar: 2020/2021
-	Asignatura: Evolución y gestión de la configuración
-	Milestone en el que se entrega la documentación: M3


### Miembros del equipo:


-	Acuña Romero, Carlos Luis
-	Benítez Vega, Daniel
-	Beza Quesada, Laura
-	García Nieto, Antonio Jesús
-	Jordá Arrillaga, Enrique
-	Muñoz del Bot, Pablo 

### Enlaces de interés:


-	repositorio de código
-	sistema desplegado
-	cualquier otro enlace de interés


## Resumen ejecutivo

### Indicadores del Proyecto

| Miembro	| Horas	| Commits |	LoC |	Test | Issues |	Incremento |
| --	| --	| -- |	-- |	--  | -- |	-- |
| Acuña Romero, Carlos Luis	| 	|  |	 |	 |  |	 |
| Benítez Vega, Daniel	| 	|  |	 |	 |  |	 |
| Beza Quesada, Laura	| 	|  |	 |	 |  |	 |
| García Nieto, Antonio Jesús	| 	|  |	 |	 |  |	 |
| Jordá Arrillaga, Enrique	| 	|  |	 |	 |  |	 |
| Muñoz del Bot, Pablo	| 	|  |	 |	 |  |	 |


 En la tabla se representa la aportación de cada miembro sobre el proyecto y su trabajo de la siguiente forma:
 
 
-	Commits: solo aquellos hechos por miembros del equipo, no lo commits previos.
-	LoC (líneas de código): solo se tienen en cuenta aquellas producidas por el equipo, no las que ya existían previamente.
-	Test: solo se tienen en cuenta aquellos realizados por el equipo.
-	Issues: Aquellos issues gestionadas dentro del proyecto y por el equipo.
-	Incremento: principal incremento funcional del que se ha hecho cargo el miembro del proyecto


### Integración con otros equipos

Al tratarse de un proyecto que realizaremos en solitario (single), este apartado no procede.

### Descripción del sistema 

A lo largo de este apartado, explicaremos el sistema desarrollado desde un punto de vista funcional y arquitectónico. Procuraremos realizar una descripción clara de la funcionalidad técnicas de los componentes que intervienen. 
Para el desarrollo de nuestro proyecto partimos de Decide, un sistema de voto en formato digital compuesta por una serie de módulos o subsistemas, cada uno de estos conectados mediante la API.
Decide-Single-Bailón cabina se encarga de uno de los módulos mencionados anteriormente, en concreto “Cabina de votación”, una interfaz para votar. Este subsistema se encarga de mostrar la interfaz de voto de una votación en concreto, y permite al votante votar de la forma más sencilla posible.
El proyecto consiste en el desarrollo en Angular de esta interfaz, aportando así una interfaz diferente a la ya implementada en Decide. Por otro lado, aportaremos el desarrollo de cuatro bots de votación que permitirán la votación de decide mediante diversas plataformas (Discord, Telegram, Slack y Line).
Ambas partes del proyecto, tanto la interfaz de en Angular como los bots se comunicarán con Decide mediante llamadas a su API para poder realizar las votaciones. 
A continuación, se realizará una explicación más exhaustiva de cada parte del proyecto.

#### Interfaz en Angular.

Angular es un framework para la creacion de webs de una sola pagina basada en componentes.
Un componente necesita una carpeta con 4 archivos principales, un *.html* con la estructura, un archivo *.ts* que contendrá toda la logica relacionada con el template, un archivo *.css* con los estilos.
La gran parte de los documentos relacionados con los estilos estarán vacios, puesto que usaremos el framework bootstrap para todos los estilos.

Tratamos de utilizar el modelo vista controlador para la produccion de esta aplicación concentrando todas las peticiones API al backend desde archivos denominados *.services*, Y almacenando los modelos que recibiamos en su respectivo contendor.

**Modelos**

Almacenan la estructura de los objetos recibidos desde decide. Clases etiquetadas con una librería para analizar cadenas json (*typedjson*)
-   *voting.model.ts* : Crearemos todas las clases correspondientes a la la estructuras de las votaciones. Todas las clases y atributos están etiquetadas para trabajar con objetos json.

```typescript
    @jsonObject
    export class Voting {
        @jsonMember({constructor: Number})
        id: number;
        @jsonMember({constructor: String})
        name: string;
        @jsonMember({constructor: String})
        desc: string;
        @jsonMember({constructor: Question})
        question: Question;
        @jsonMember({constructor: Date})
        // tslint:disable-next-line:variable-name
        start_date: Date;
        @jsonMember({constructor: Date})
        // tslint:disable-next-line:variable-name
        end_date: Date;
        @jsonMember({constructor: PubKey})
        // tslint:disable-next-line:variable-name
        pub_key: PubKey;
        @jsonArrayMember(Auth)
        auths: Auth[];
        @jsonMember({constructor: Object})
        tally: null;
        @jsonMember({constructor: Object})
        postproc: null;
    }
```

**Servicios**

Aquí se concentra todo el código relacionado con el backend, estos archivos proporcionan information relevante a los componentes para que la aplicación funcione correctamente.
Desde llamadas API al framework de python que contiene decide hasta la máquina de estados de login del usuario y el almacenamiento de sus credenciales.
-   *authentication.service.ts* : Gestiona el login y logout del usuario.
-   *voting.service.ts* : Realiza todas las peticiones relacionadas con las votaciones y el usuario. También contiene el código para transformar de json a objeto typescript.

```angularjs
    getVotings(): Observable<object> {
        return this.http.get(`${environment.apiUrl}gateway/voting/`);
    }

    parseVotings(votings: any): Voting[] {
        const res: Voting[] = [];
        votings.forEach(v => {
            res.push(v.parseVoting);
        });
        return res;
    }
```
En este codigo vemos como se realizan las llamadas a la api y como son transformadas a objetos typescript

**Componentes**

La aplicación muestra en todo momento el header, la salida de enrutamiento y el footer. La salida de enrutamiento es una herramienta que nos ofrece angular y nos permite cambiar el dom en tiempo de ejecucion.
Veremos el codigo de las votaciones como ejemplo:

-	*voting.component.html* : que contiene el template de la interfaz creada

```angular2html
    <div id="app-booth" class="principal">
        <h1>{{ votingId }} - {{ votingName }}</h1>
        <div *ngIf="logged">
          <h2>{{ votingQuestionDesc }}</h2>
          <form [formGroup]="votingForm" (ngSubmit)="onSubmitVote($event)" novalidate>
            <div class="custom-control custom-radio" >
              <input type="radio" class="form-check-input" id="si" name="option" value="1">
              <label for="si" class="form-check-label" >1. Si</label>
            </div>
            <div class="custom-control custom-radio" >
              <input type="radio" class="form-check-input" id="no" name="option" value="0">
              <label for="no" class="form-check-label" >2. No</label>
            </div>
            <div *ngIf="isSubmitted && myForm.invalid">
              <p>Por favor seleccione una opción</p>
           </div>
            <div class="boton">
              <button class="button button-3" type="submit"> Vota </button>
            </div>
          </form>
        </div>
      <div *ngIf="!logged">
        <h1>No has iniciado sesión</h1>
      </div>
    </div>
```

Como podemos ver usamos la directiva ngIf, esta oculta o muestra parte del html dependiendo del boolean que reciba

-	voting.component.css : Tenemos angular configurado para trabajar con bootstrap, asi que no es necesario añadir estilos en los documentos *.css*

-	voting.component.ts: Se trata del archivo TypeScript que contiene la lógica del componente.

```typescript
    ngOnInit(): void {
        this.logged = !this.authService.isLogged;
        const id = +this.route.snapshot.params.id - 1;
        this.votingService.getVoting(id).subscribe((res) => {
            this.voting = TypedJSON.parse(res[id], Voting);
        }, error => {
            console.log(error);
        });
    }
```
Esta funcion se ejecuta cuando el componente voting es cargado, aquí se pide la consulta de la votacion que es parseada de json a TS gracias a la libreria *typedjson*

-	voting.component.spect.ts : Es el test del componente.

```typescript
    it('should create', () => {
        expect(component).toBeTruthy();
    });
```
Un codigo muy sencillo que comprueba si el componente se crea correctamente.

La funcionalidad de esta parte del proyecto es la siguiente:  

-	Permitir iniciar sesión al usuario mediante el nombre de este y una contraseña.
-	Poder acceder a una lista de votaciones permitidas para el usuario una vez haya iniciado sesión.
-	Cuando se escoge la votación, el usuario puede participar en la misma.

#### Bot de discord.

Comenzaremos analizando la estructura del proyecto.

![Imagen 6](Imagenes/BotImages/estructuraProyecto.png "Estructura del proyecto")

En la imágen superior podemos ver:
- README.md que da una explicación genereal del funcionamiento del bot.
- Images que es una carpeta donde se depositan las imágenes que se muestran en el README.md
- Ficheros de configuración para el despliegue en heroku:
- Procfile, donde se expecifica que queremos un "worker"(una instancia del bot) y no un sistema web, tambien indicamos el idioma en el que se debe ejecutar, en este caso python, y donde está el archivo de arranque, en este caso bot.py.

 ![Imagen 7](Imagenes/BotImages/procfile.png "Estructura del proyecto")

- requirements.txt, en el se indican a heroku cuales van a ser las dependencias, es decir, las librerias necesearias para el funcionamiento del bot.

![Imagen 9](Imagenes/BotImages/requirements.png "Requirements")
    
- Un fichero para el depliegue en travis : travis.yml. En el usamos los siguientes etiquetas:
            - languaje -> indica el lenguaje que usará travis.
            - python -> indica la versión de python que usaremos.
            - install -> indicaremos que es necesario descargar para la correcta ejecución en el entorno virtual de Travis.
            - script -> indicaremos que script tiene que ejecutar, en este caso el fichero de test (este comando es posible gracias a la librería unitest).
            - deploy - indica opciones referentes al despliegue, en este caso con provider le decimos que lo queremos desplegar con heroku y con on y all_branches le forzamos a que lo haga con todos los cambios.

![Imagen 10](Imagenes/BotImages/travis.png "Travis")

- Como ultimo fichero nos encontramos con el bot.py y config.py, el segundo simplemente son variables globales de configuaración. En lo referente a bot.py ahí es donde sucede la magia. No se va ha realizar una fotografía del código completo, iremos guiandonos por las lineas del fichero y nombres de métodos. Para acceder al código clique [aquí](https://github.com/EGC-decide-bailon/bot-dc/blob/main/bot.py) .Comenzaremos viendo las distintas partes del fichero:

    - Líneas 1 a 7. **Imports**. No hay nada demasiado interesante, importamos las librerías de referentes a la API de Discord, la librería asyncio para poder trabajar de forma asincrona, y la librería request para hacer peticiones. El resto de imports son para dar utilidad.

    - Líneas de la 9 a la 18 y de la 214 a la 219. **Basics**. Se inicializan las partes básicas para el funcionamiento del bot(lineas 9-18) y se crea un evento para que podamos ver en consola que funciona del modo esperado.

    - Líneas de la 22 a las 208. **BotCommands**. En esta sección se encuentran definido los comandos que ejecutará el bot, todos han sido comentados de forma correcta, explicando la funcionalidad del método y los inputs en caso de que sean necesarios. Por ello a continuación dejaremos una lista de comandos y lineas en las que se empiezan y después detallaremos algunos aspectos clave.
        - ##### Lista de comandos
            - 37 -> info
            - 52 -> loginAsUser
            - 88 -> votings
            - 128 -> voting
            - 161 -> vote
            - 200 -> clean
        - ##### Conceptos claves
            Una idea de las ideas detras del funcionamiento del bot es la seguridad en la votación, lo normal es que se llame al bot en un canal, y se interactue con el ahí, sin embargo este bot manda un mensaje directo al usuario que le invoque en un canal público. A nivel de código tan solo necesimos extraer del contexto, por convención ctx, el autor mediante el método .author() y después usamos los comandos .create_dm() y .dm_channel() sobre el autor obtenido para crear el mensaje, esto los usamos mucho. Por ejemplo en el método iniVotación, en concreto en las líneas 31,33 y 34.

    - Líneas de la 224 a las 272. **Utiles**. En esta sección se encuentran definido los comandos que sirven de apoyo a los principales y permiten hacer más comprensible y modular el código. Todos han sido comentados de forma correcta, explicando la funcionalidad del método y los inputs en caso de que sean necesarios igual que en los anteriores. Por ello a continuación dejaremos una lista de comandos y lineas en las que se empiezan.

       - ##### Lista de comandos
            - 228 -> getUser
            - 240 -> saveVoteDate
            - 255 -> parseVotings
    
    - Líneas 276 y 277. **Main**. En estas lineas, tan solo se le indica al bot que comience a funcionar mediante la función .run() pasandole como parametro el token del bot suministrado por Discord.

A modo de resumen,la funcionalidad implementada en esta parte del proyecto es la siguiente:  
- Permitir iniciar sesión al usuario mediante el nombre de este y una contraseña.
- Listar todas las votaciones posibles, una vez el usuario se haya logeado.
- Poder acceder a una votación en detalle, una vez el usuario se haya logeado.
- Permitir al usuario votar,una vez registrado.

#### Bot de Telegram.

Esta es la estructura básica del bot:

![Imagen 11](Imagenes/BotTelegram/estructuraProyecto.png "Estructura del proyecto")

Los puntos más remarcables y distintos a los demás bots son:
 - Bot es una carpeta con todo el código esencial del bot.
 - Configs contiene toda la conmfiguración necesaria para conectar el bot a las distintas plataformas.
 - Util tiene un par de clases de utilidad como código para el parseo de los datos recuperados y la definición de variables globales.
 - Main es el archivo base del bot, con la definición de la estructura y métodos del bot. Este archivo es el que hay que lanzar para iniciar el funcionamiento.

Los demás archivos, menos por mínimas diferencias de configuración, es igual al bot anterior.

Dentro del bot encontramos varias funcionalidades que trabajan a modo de pipeline. En orden, los métodos serían:

![Imagen 12](Imagenes/BotTelegram/start.png "Start")

- Aquí definimos el método inicial del bot. Asignamos el comando `start` para iniciar la conversación.

![Imagen 13](Imagenes/BotTelegram/start2.png "Start2")

- Creamos con `boton_login = [['Login']]` y `reply_markup=ReplyKeyboardMarkup(boton_login, one_time_keyboard=True)` un botón de login aparte de darle la bienvenida al usuario.
 
![Imagen 14](Imagenes/BotTelegram/login.png "Login")

- El primer método que vemos, es el mensaje pidiendo el usuario y contraseña con un formato predefinido.

- El siguiente se compone de varias partes:
  + La recuperación de los datos.
  + Creamos los credenciales con el formato adecuado para enviarlo a la aplicación principal.
  + Comprobamos que la llamada con los credenciales son correctos antes de pasar a la siguiente fase, si no, volvemos a insistir en los credenciales.

![Imagen 15](Imagenes/BotTelegram/votings.png "votings")

El siguiente paso sería recuperar todas las votaciones existentes y mostrarlas para que el usuario pueda decidir qué hacer.

- Con la primera parte del método, obtemenos la lista de votaciones totales del sistema.

- Cogemos esos objetos y los parseamos y filtramos para coger sólo las votaciones activas.

- Por último y usando los mismos métodos que para crear el botón anterior, creamos uno para cada una de las votaciones.

![Imagen 16](Imagenes/BotTelegram/voting.png "voting")

Una vez seleccionado la votación, tendríamos lo siguiente:

- Con la primera parte, recuperamos el id de la votación elegida.

- Después, creamos un botón para cada opción, con el id y la opción a escoger.

- Por último, recuperamos la descripción de la votación para mostrarla por chat junto a los botones creados por cada opción.

En este punto, usamos la misma llamada que con el anterior, ya que si la url la buscamos sin ningún campo id, recuperamos todos los votos y nos ahorramos hacer un método propio para ello.

![Imagen 17](Imagenes/BotTelegram/save.png "save")

Ya por último, tenemos que guardar ese voto según nuestro estándar:

- Priemro recuperamos el id de la opción recogida.

- Recuperamos el usuario para poder asignarle el voto a él.

- Comparamos la opción para poder adaptar la respuesta a nuestra solución, poniendo un 1 a la opción cogida y un 0 a la que no.

- Guardamos todos los valores necesarios en el formato correspondiente y lo mandamos a la aplicación con su llamada correspondiente.

- Por último y para finalizar el proceso, mandamos un mensaje informando que han realizado la votación correctamente. Con el método `ConversationHandler.END` terminamos el pipeline y estará listo para empezar de nuevo el proceso completo.

Todos los comandos junto al manual de uso, se encuentra en el propio README del bot. Las llamadas y parseos que se ven durante el desarrollo del bot, son iguales a los de los demás bots desarrollados y definidos en esta documentación.

La idea del bot es usarlo de forma privada, ya que en un punto del proceso necesitamos pasarle por escrito las credenciales de nuestro usuario. Debido a la API actual, no podemos borrar ese mensaje sin borrar toda la conversación existente, por lo que si se usa en un grupo, es responsabilidad del usuario el pasar las credenciales por el chat. Una vez terminado el proceso, podemos usar como usuarios, una opción de la aplicación que permite el borrado de los mensajes mandados por ambos y así poder mantener la privacidad de nuestro usuario y contraseña de Decide.

## Visión global del proceso de desarrollo

En esta sección proporcionaremos una visión general del proceso que hemos seguido a lo largo del desarrollo del proyecto.
En nuestro caso concreto, hemos elegido la parte de Cabina como subsistema a desarrollar. Este componente cumple la función de servir de interfaz gráfica al sistema de Decide, por eso desde el grupo `Bailón` hemos apostado por ramas bien diferenciadas:
- Por una parte queríamos modernizar la interfaz actual del portal web haciendo uso de tecnologías más novedosas como Angular.
- Por otra parte, queríamos hacer accesible la funcionalidad del sistema a un mayor número de personas, llevando el alcance de Decide a otras plataformas comúnmente usadas por los usuarios en otros ámbitos como pueden ser Discord, Line, Slack y Telegram; facilitando su uso de forma generalizada.
Teniendo en cuenta lo anterior, cobra sentido el hecho de crear varios repositorios dentro de una organización de forma que todas las funcionalidades estén disponibles para la consulta por todos los miembros del equipo en un mismo sitio, pero manteniendo la individualidad de cada incremento funcional.

Se ha creado un tablero Scrum para la organización general del proyecto, donde se describen todas las tareas, responsables y estados de las mismas. Esto permite tener una visión general del proyecto a todo el equipo durante el proceso completo de desarrollo, permitiendo una mejor comunicación junto con un mayor control del avance del proyecto.

Las tareas se van a definir en su mayoría en una reunión de lanzamiento del proyecto. Iremos mirando todas las funcionalidades añadiendo tareas según las necesidades vistas en cada incremento funcional. Esto no quiere decir que una vez puestas estas funcionalidades, no puedan añadirse más. Si durante el desarrollo de un incremento se detecta una nueva funcionalidad, o alguien requiere ayuda con algo, puede libremente crear un nuevo issue (siguiendo el mismo formato ya seleccionado) y notificar de ello a las partes interesadas del equipo de desarrollo.

Debido a la cantidad de issues presentes en el tablero, para organizarlo un poco más y facilitar la búsqueda de alguna tarea concreta, se han creado dos grupos de desarrollo, `Angular` y `Bots`. Con esto conseguimos diversas ventajas:
 - Cada equipo dispone de un foro propio en el que poder comentar problemas encontrados y que puedan interesar a otros miembros del mismo equipo.
 - Podemos ver una lista de los integrantes de cada grupo, lo que facilita la comunicación con una persona en concreto del mismo equipo.
 - Se puede mencionar a todo el equipo en cualquier comentario sin necesidad de añadir a todos los integrantes.
 - Añade un nivel de jerarquía sobre los documentos, pudiendo elegir el nivel de privilegio de cada grupo de trabajo.
 - Pueden verse los repositorios específicos del grupo en un listado aparte, pudiendo así encontrar cualquiera más rapido entre el listado total.

A la hora de realizar cambios o añadir funcionalidades, se abordarán según estén relacionadas con la mejora visual de la cabina o la creación de bots:
 - En el primer caso, debido a que se trata de una funcionalidad con mayor complejidad que las otras, se va a asignar a dos integrantes del equipo. Por eso, la política de asignación de tareas para este caso concreto atenderá en primer lugar a la disponibilidad de los miembros asignados, es decir, quien tenga posibilidad de desarrollar una nueva tarea por haber terminado la anterior, será asignado a ésta. En segundo lugar, se tendrá en cuenta la prioridad de cada tarea con respecto a las demás, siendo desarrolladas de mayor a menor prioridad. Existe una excepción con las de prioridad `Critical`, ya que deberán ser cogidas por cualquier persona que esté libre, pero esto se desarrollará más adelante. Por último, se tendrán en cuenta las preferencias personales de cada uno.
 - Con los bots es algo distinto, ya que al ser tecnologías bastantes diferentes, hay un responsable más claro para las tareas relativas a cada uno de los bots (aunque esto no implica que una persona no pueda trabajar en dos o más de ellos). Esto supone el primer punto en la política de asignación de tareas, ya que cada miembro cogerá en primer lugar las tareas propias de la tecnología que es experto. El segundo punto, como en el anterior, es la prioridad de las tareas que queden libres.

A continuación, se describe un ejemplo de como se abordarían todo el ciclo hasta llevar un cambio a producción para cada tipo de mejora funcional. Para las mejoras relativas a la cabina el modo de proceder será el siguiente:

1. Se creará una Issue en función de las necesidades del proyecto en la columna de `No Assigned`.
2. Esa tarea se moverá dependiendo de su prioridad a una de las columnas designadas a ello `Critical`, `High` y `Low`.
3. Se crea una rama específica para desarrollar la funcionalidad sin afectar a la rama master con el siguiente formato `feature/palabra-clave`.
4. Se asigna según la política de asignación correspondiente la tarea a un encargado, que la moverá al estado `In progress`.
5. Se empieza a desarrollar la funcionalidad haciendo mínimo un commit al día hasta tener una versión estable.
6. Se crea un pull request y se aprueba si Travis no da fallo y se pasa la issue a `Done`
7. Se elimina la rama y la tarea pasa a `Closed`

Para los bots, el proceso cambia un poco debido a cómo hemos trabajado con ellos, poniendo un repositorio propio para cada uno. Esto no es una decisión interna, ya que era necesario para poder desplegarlos por separado pero manteniendo un orden en nuestros respositorios. El proceso sería el siguiente:

1. Se creará una Issue en función de las necesidades del proyecto en la columna de `No Assigned`.
2. Esa tarea se moverá dependiendo de su prioridad a una de las columnas designadas a ello `Critical`, `High` y `Low`.
3. Como el repositorio es separado para cada bot, se empieza a desarrollar la funcionalidad en local.
4. Se realizan varias pruebas por el desarrollador hasta que se obtenga el funcionamiento esperado.
5. Una vez obtenida una versión estable del bot, se sube el código a la rama master de su propio repositorio.
6. La tarea pasa a estar en `Done`.
7. El bot, es posteriormente desplegado en Heroku a la espera que otro miembro del equipo realice las pruebas oportunas por su cuenta y confirme que la funcionalidad está completa.
8. La tarea pasa a `Closed` a la espera que se conecte con Travis y se hagan sus pruebas unitarias.

## Entorno de desarrollo
## Gestión de incidencias


