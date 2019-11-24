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

Lo primero de todo es decir que, por cada **CONexión**, debe existir una localidad definida en **/CTL**. 

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

