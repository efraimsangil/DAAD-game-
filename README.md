# DAAD
 DAAD Adventures

En este espacio podrás encontrar un juego totalmente documentado, para que puedas aprender a usar DAAD.

Poco a poco voy actualizando el documento y el juego, según vaya avanzando en mi aprendizaje.

Espero que te sirva de ayuda.
----------

### Introducción

Esta es una aventura de terror con ciertos toques en clave de humor, de modo que la tensión suba y baje para no causar un ataque al corazón a ningún jugador.

Nuestro protagonista, Marcos, que vive en Madrid, recibe una llamada de su Padre para decirle que se reúna con la familia en su vieja cabaña en un bosque de la Sierra de Gredos. Tiene que contarle algo.

Todo el desarrollo de la historia ocurrirá en dicha cabaña y nuestro objetivo será lograr salir de la misma una vez hayamos entrado.

#### Mapa

Este es el mapa en el que se desarrolla la primera parte del juego.

(Falta la imagen)

#### Comienza la aventura

Lo primero que vamos a configurar son las **CONexiones** de las **LOCalizaciones**, es decir, las cajitas del mapa que son los espacios donde van a suceder cosas y por donde se moverá nuestro protagonista.

```json
/CON    ;Conexiones
/0
/1
E lCoche
/2
E lExtcasa
/3
N lLenera
S lGarage
E lPorche
/4
S lExtcasa
/5
N lExtcasa
/6
O lExtcasa
```

**Localidades** o **localizaciones** que usaremos en el juego. Tiene que haber una por cada cajita de nuestro mapa. Esto se pone en el comienzo de la plantilla **DSF**, en la zona **/CTL**:

```json
;localidades
#define lIntro      0
#define lMadrid     1
#define lCoche      2
#define lExtcasa    3
#define lLenera     4
#define lGarage     5
#define lPorche     6
```

Explicación: 

Lo primero de todo es decir que, por cada **CONexión**, puedes definir (#define) una **Localización** en **/CTL**. Si no, puedes referirte a ellas con su número (**AT 1**).

- **/0** es el comienzo de la aventura, la primera pantalla que aparece nada más cargar nuestro juego. En esta localidad no hay objetos ni acciones. Es sencillamente la presentación de nuestra aventura y una vez leída, pasa automáticamente a la siguiente localidad, la **/1**. El texto que se muestra aquí es el definido en **/LTX** como **/0**. Y también debe existir una entrada de dicha localidad en **/CTL**, ya que dicha entrada o definición, será la que se use más adelante en las secciones de **PROcesos** o **/PRO** de nuestro fichero **DSF**. Ya, explicaremos esto de los **/PRO** más adelante.

- **/1** es la localidad siguiente después de mostrar la presentación de la aventura. Es decir, que después de mostrar el **/0**, que no deja de ser una introducción o presentación de la aventura, inmediatamente después, aparecemos en esta localidad. Al igual que la anterior, mostrará el texto definido en **/LTX** y la tendremos definida en **/CTL**. Aquí ya puede haber acciones, objetos, etc. Aquí comienza la interactuación con el usuario que comienza a jugar y a introducir comandos. Si os fijáis bien, en la definición de **/1** tenemos un **E lCoche**, que sirve para indicar que desde la localidad **/1**, que es **lMadrid**, podemos movernos al **ESTE**, a la localidad **lCoche**.

- **/2** funciona de la misma manera que **/1** con la diferencia de que nos podemos mover la localidad **lExtCasa** si vamos al **ESTE**.

- **/3** sigue siendo igual con la diferencia de que tenemos tres posibles movimientos, **NORTE**, **SUR** y **ESTE** y nos desplazamos a **lLenera**, **lGarage** y **lPorche**.

No explico los demás, porque entiendo que ha quedado claro el funcionamiento del **/CON**.

Hemos comentado que cada vez que te mueves a una nueva localidad (desde ahora voy a llamarlo localización, que me gusta más), mostramos un texto descriptivo de la misma. Pues bien, aquí es donde pondremos las descripciones de las **LOCalizaciones**, en la zona **/LTX**:

```json
/LTX    ;Location Texts
/0 "Es sábado, son las 08:00AM y aún sigues en la cama. Semi despierto y sin ganas de levantarte. Madrugas mucho durante la semana y no te apetece mucho abandonar el calorcito que sientes bajo el plumón."
/1 "Te giras en la cama con la firme intención de dormir un poquito más. Suena el teléfono móvil. Alguien ya decidió por ti que te debes despertar del todo. Tienes a tu lado la mesita de noche."
/2 "Sales de casa y coges tu coche rumbo a la sierra de gredos desde Madrid. Es un paseito, pero el viaje se hace ameno escuchando un poco de música."
/3 "Estás en el exterior de la casa. Desde aquí puedes ir al Norte, Sur y Este"
/4 "Estás en la leñera. Puedes entrar o volver al Sur"
/5 "Lograste entrar en el garage. Que fácil es todo cuando tienes las llaves"
/6 "Estás en el porche de la casa. Al Este está la puerta principal. Oeste para volver al exterior de la casa"
```

También vamos a definir algunos verbos nuevos que vamos a usar en la aventura y que no vienen por defecto en nuestra plantilla de inicio (**BLANK_ES.DSF**). Se define en la zona que pone** /VOC** de **VOCabulario**.

```json
;verbos jugador
USAR    33      verb
```

Para esta primera parte, hemos definido el verbo **USAR** porque será lo que tendrá que hacer nuestro protagonista con el móvil, **USAR MOVIL**. De este modo, es como podrá continuar la aventura. Luego veremos el por qué.

En la parte principal, en la zona **/CTL** o **Sección de ConTroL**, vamos a añadir algunas cositas más.

Algunos **Flags** que necesitaremos para esta primera parte. Ahora explicaré para que los usaremos:

```json
;Flags de usuario   
#define fPuertaLenera       64
#define fPuertaGarage       65
#define fPuertaPorche       66
#define fUsarMovil          67
```

Estos **Flags**, son los que vamos a usar durante la aventura para saber si abrió alguna puerta o si usó el móvil. Son condicionales para avanzar en la aventura, me explico, si por ejemplo no has usado el móvil, es decir, no decidiste coger la llamada de teléfono, no puedes seguir avanzando en la aventura. Los **Flags** de las puertas, igual, si no se activan dichos **Flags**, no podrás acceder a nuevas localizaciones, como la *Leñera*, el *Garage* o abrir la puerta principal de la casa ubicada en la localización *Porche de la casa*. Estos **Flags** van a contener un 0 al inicio de la aventura y valdrán 1 cuando hayamos realizado ciertas acciones, como **COGER MOVIL** o **COGER LLAVE**.

Y ahora definimos todos los objetos que vamos a usar en estas **Localizaciones**.

```json
;Objetos
#define oMovil          0
#define oLlave1         1
#define oLlave2         2
#define oBateriaCoche   3
#define oPinzasCoche    4
#define oLlave0         5
#define oFelpudo        6
#define oMaceta         7
```

También tenemos que definir nuestros **OBJetos** en la zona **/OBJ**. 

```json
/OBJ    ;Objetos
;obj  starts  weight    c w  5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0    noun   adjective
;num    at
/0      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    MOVIL       _
/1      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    LLAV1       _
/2      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    LLAV2       _
/3      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    BATERIA     _
/4      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    PINZAS      _
/5      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    LLAV0       _
/6      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    FELPUDO     _
/7      252     1       _ _  _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _    MACETA      _
```

Este apartado es especialmente importante.
- **Obj**: es el número del objeto definido en /CTL
- **Starts**: hay tres posibilidades:
 - 252: El objeto no está en ninguna parte (de momento no es visible).
 - 253: El objeto lo llevamos puesto (es una prenda).
 - 254: El objeto lo llevamos en nuestro inventario.
- **Weight**: (Buscar documentación)
- **C**: (Buscar documentación)
- **W**: (Buscar documentación)
- **Números**: (Buscar documentación)
- **Noun**: Nombre del objeto
- **Adjetive**: (Buscar documentación)

#### Acciones

Vamos a comenzar a introducir cierta funcionalidad a la aventura. Para eso vamos a hacer uso de la zona **/PRO**, en este caso **/PRO 5**.

```json
> EXAMINA MESITA AT lMadrid
                MESSAGE "La mesita de noche está limpia y perfecta."
                ISAT oMovil 252
                MESSAGE "Hay un teléfono móvil en ella."
                CREATE oMovil
                DONE

> EXAMINA MESITA AT lMadrid
                NOTCARR oMovil
                MESSAGE "El teléfono sigue sonando"
                DONE  

> EXAMINA MESITA AT lMadrid
                DONE  
```

Explicación de la primera entrada:

La primera que pones **EXAMINAR MESITA**, va a encontrar el objeto (lo hacemos visible en esta localización) y lo creamos.

- **EXAMINA**: este es el verbo al que vamos a darle funcionalidad
- **MESITA**: es el objeto al cual podemos hacer uso del verbo
- **AT lMadrid**: indica que se puede usar en esta localización
- **MESSAGE**: El texto que mostramos al ejecutar esta acción.
- **ISAT oMovil 252**: Indicamos que hay un móvil. Si no ha usado antes **EXAMINA MESITA**, ese móvil no aparece por ningún sitio porque es un objeto oculto que deja de serlo al usar esta acción.
- **MESSAGE**: El texto que vamos a mostrar en pantalla tras descubrir el nuevo objeto.
- **CREATE oMovil**: Acabamos de crear el objeto y ya será siempre visible.
- **DONE**: Acaba la acción. No se ejecuta nada más.

Explicación de la segunda entrada: (Sólo los cambios que hay)

Si pones de nuevo **EXAMINAR MESITA** y **NOTCARR oMovil**, es decir, no hemos cogido el móvil aún (es el acrónimo de **NOT CARRIED**), el mensaje de texto “*El teléfono sigue sonando*” es una pista más que te indica que debes cogerlo.

- **NOTCARR oMobil**: Comprueba si aún no hemos cogido el móvil
- **MESSAGE**: El texto que mostramos
- **DONE**: Fin de acción

Explicación de la última entrada:

Habiendo cogido ya el móvil, el mensaje simplemente describe la mesita.

#### Seguimos con más acciones

Vamos a poner un par de acciones más para la localización **lMadrid**:

```json
> USAR  MOVIL AT lMadrid
                CARRIED oMovil ; ¿lleva móvil?
                MESSAGE "+Buenos días hijo. Qué tal estás?.#n-Hasta que me llamaste estaba como un rey, en cama, calentito y tratando de negociar con Morpheo una segunda ronda.#n+Dile a Morpheo que espere un momento. Escucha, vente a la cabaña a pasar el día con nosotros, tenemos algo que contarte#n-Me apetece mucho, si... Pero no puede...#n+Hasta luego#n-Cojonudo, no he podido poner un excusa, tengo que ir."
                SET fUsarMovil
                DONE

> MIRAR   _    AT lMadrid
                CARRIED oMovil ; ¿lleva movil?
                NOTZERO fUsarMovil
                MESSAGE "Aquí ya no tienes nada que hacer. Tira para el coche: Este"
                DONE

> E     _      AT lMadrid
                Zero fUsarMovil
                MESSAGE "No crees que deberías atender al móvil?"
                DONE
```

Explicación de la primera entrada:

Aquí vamos a controlar que, una vez cogido el móvil, lo use.

- **USAR MOVIL**: la acción que vamos a controlar.
- **AT lMadrid**: en la localización de **lMadrid**
- **CARRIED oMovil**: controlamos que cogió el móvil.
- **MESSAGE**: Es el texto que aparece al coger el móvil. La conversación con su padre.
- **SET fUsarMovil**: Activamos el Flag de ha usado el móvil. El **Flag** se pone a 1.
- **DONE**: Acaba la acción.

Explicación de la segunda entrada:

Como se supone que ya hemos cogido el móvil y hablado con nuestro Padre, debemos ayudar al jugador a que dé el siguiente paso. Y para que no se ponga a enredar en la habitación porque NO hay nada más que hacer, le ayudamos con la acción **MIRAR**, que siempre que la usamos, nos muestra la descripción de dicha localización. Al definirla aquí, le podemos dar más pistas al jugador.

- **MIRAR**  __ : Es el verbo al que le programamos esta funcionalidad, que es cuando use **MIRAR** lo que sea (ese **_** es lo que indica).
- **AT lMadrid**: en la localización **lMadrid**
- **CARRIED**: Comprobamos que lleve el móvil en su inventario
- **NOTZERO fUsarMovil**: Comprobamos que usó el móvil, es decir, que cogió la llamada.
- **MESSAGE**: El mensaje que mostramos al jugador.
- 	**DONE**: Acaba la acción.

Explicación de la tercera entrada:

Esta entrada es para controlar que no se vaya de la habitación a la localización **lCoche (Este)* sin haber cogido la llamada.

- **E  _**: Si quiere ir al este .
- **AT lMadrid**: estando en dicha localización.
- **ZERO fUsarMovil:** comprobamos si el Flag es 0, es decir, que no ha usado el móvil, que no cogió la llamada.
- **MESSAGE**: El texto a mostrar.
- **DONE**: Acaba la acción.
