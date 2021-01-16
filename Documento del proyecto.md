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

Para poder crear la interfaz, deberemos crear un componente en Angular con esa intención.
Este componente está compuesto por un archivo que contendrá el Template (.html), un archivo que contendrá la lógica (.ts),
y un archivo para el css donde se diseñará la apariencia de la interfaz. 
Concretamente, hemos creado el componente de la cabina , denominado “votings” con los siguientes archivos :

-	Votings.component.html : que contiene el template de la interfaz creada. En este archivo hemos utilizado el elemento *ngIf propio en el Angular en el form, ya que la votación solo se puede realizar si el usuario ha iniciado sesión, de esta forma y utilizando este elemento no aseguramos de que en caso de que el usuario no haya iniciado sesión se le presente una vista donde pueda hacerlo (Imagen1) y a continuación proceda a votar, o en el caso de que si esté loggeada pueda acceder directamente a la votación (Imagen 2). 

![Imagen 1](Imagenes/votings.component.html_sign_in.png "votings.component.html sign in")
![Imagen 2](Imagenes/votings.component.html_vote.png "votings.component.html vote")

-	Votings.component.css: En el archivo html se utiliza Bootstrap, aún así decidimos utilizar botones diseñados mediante css para darle un toque más original a la interfaz de de la cabina.

![Imagen 3](Imagenes/votings.component.css_ejemplo_de_diseño_de_botón.png "votings.component.css ejemplo de diseño de botón")

-	Votings.component.ts: Se trata del archivo TypeScript que contiene la lógica del componente. Podemos observar su código en las Imágenes 4 y 5.

![Imagen 4](Imagenes/votings.component.ts_1.png "votings.component.ts 1")
![Imagen 5](Imagenes/votings.component.ts_2.png "votings.component.ts 2")

-	Votings.component.spect.ts : Es el test del componente, que comprobará si todo funciona como ha sido previsto.

![Imagen 6](Imagenes/votings.component.spect.ts ejemplo_de_test.png "votings.component.spect.ts ejemplo_de_test")

La funcionalidad de esta parte del proyecto es la siguiente:  

-	Permitir iniciar sesión al usuario mediante el nombre de este y una contraseña.
-	Poder acceder a una votación una vez el usuario haya iniciado sesión.
-	Una vez elegida la votación, que el usuario puede participar en la misma.



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

Aunque como hemos dicho anteriormente existen diferencias en función al incremento funcional a desarrollar dentro del proyecto, hemos acordado utilizar el mismo ecosistema de desarrollo. Esta decisión la hemos tomado para tratar de evitar los problemas de configuración lo maximo posible y facilitar el "pair programming" en caso de ser necesario.

### Siststema operativo
En cuanto a sistema operativo se refiere utilizaremos Windows 10 (todos contamos con la versión 10 pero no es obligatorio, pues es de pago) y Ubuntu nativo en su versión 20.04.

Hemos decidido usar Ubuntu de forma nativa, pues al principio del proyecto y de la asignatura nos encontramos con numerosos problemas al usar máquinas virtuales y además, los profesores nos dieron la recomendación de instalar Ubuntu en una partición del ordenador para ahorrarnos problemas.

Los SO los hemos usado en función de las necesidades que han ido surgiendo. Al principio del proyecto, antes de tener desplegado Decide en heroku, era necesario desplegar de forma local Decide y probar las funcionalidades desarrolladas, para eso la sintaxis de la consola de comando de Ubuntu era mucho más sencilla y util. Sin embargo, en las etapas finales del proyecto donde teniamos que prestar más atención a la documentación y Decide ya se encontraba desplegado algunos integrantes usaron Windows 10 pues estaban más familiarizados con él.

En cualquier caso el trabajo en ambos sistemas era muy similar, pues se utilizaba el mismo IDE como se detallará a continuación.

### IDEs
Para abordar proyectos de un tamaño mediano o grande es recomendable usar lo que se conoce como IDE.

Los IDE (Integrated Development Enviroment) o en español Entornos de Desarrollo Integrado, aportan un marco de trabajo completo y amigable que permite incrementar la eficiencia y rapidez con la que se desarrolla el código.

En nuestro caso particular hemos decidido utilizar IDEs, en vez de programas de edición de texto como VIM o NANO que aunque son mucho más ligero requieren una curva de aprendizaje mayor, que no era asumible en este proyecto.

En concreto usaremos VSC (Visual Studio Code), porque nos ofrece numerosas ventajas sobre otros IDEs. VSC permíte una sencilla integración de librerías para el desarrollo de código, que permiten implementar desde un coloreado sintactico, con plug-ins como Monokai Night Theme, hasta cambiar los iconos de los archivos según su tipo para simplificar el entendimiento de estos y hacer así más ágil el trabajo (Material Icon Theme). También existen otros plugins que te ayudan a seguir las buenas practicas o autocompletar el código, pero en este caso no lo hemos utilizado. Otro beneficio que hemos encontrado en este IDE, es que integra la consola de comando lo que hace que cualquier tarea como subir codigo a github o abrir algún programa se realize de manera mucho más eficiente.

### Gestor de código

Como gestor de código usaremos Git-Hub, hemos decidido usarlo debido a que era el GC con el que más familiarizados estabamos todos los integrantes del equipo. Esto no es la única ventaja que nos aporta Git-Hub, si no que este además, nos permite crear una organización donde reunir los repositorios y los contribuidores del proyecto, coordinandolos de forma amigable mediante la creación de tableros Kanban donde se reflejan las tareas y avances en este. Además se pueden crear equipos de trabajo, que permiten esquivar la información general de la organización y centrarse en funciones parecidas a la de uno mismo.


### Lenguajes de programación

Nuestro equipo se dividirá en dos equipos de trabajo, `Bots` y `Angular`.
 - El equipo `Angular` basará su trabajo en el framework Angular, y por tanto harán uso de un servidor Node construido en JavaScript. Como lenguajes usaron **JavaScript**, **TypeScript** , **CSS** y **HTML**.

 - El equipo `Bots` usará como lenguaje de programación para el desarrollo de los incrementos funcionales **Python**. El hecho de que usemos todos este lenguaje no es casulidad, lo que buscamos es poder tener una gran transferencia de conocimiento, ya que aunque las APIs propias de cada Bot sean distintas la estructura general de este será bastante similar y es posible que los errores de un Bot los haya resuelto otro desarrollador previamente en su bot.

### Integración continua y despliegue.

Para lo referente a integración continua y despliegue del proyecto hemos usado Travis y Heroku.

Travis es un sistema web que permite realizar integración continua en repositorios públicos de github de forma completamente gratuita. Hemos decidido usar esta herramienta debido a que el equipo ya estaba familiarizada con ella.

Heroku es un sistema web que permite el despliegue de aplicaciones de forma gratuita, ya sea en repositorios publicos o privados de Git-Hub. Hemos decidido usar esta herramienta por recomendación del profesorado.




## Gestión de incidencias


