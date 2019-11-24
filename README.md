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

	-	**/0 **es el comienzo de la aventura, la primera pantalla que aparece nada más cargar nuestro juego. En esta localidad no hay objetos ni acciones. Es sencillamente la presentación de nuestra aventura y una vez leída, pasa automáticamente a la siguiente localidad, la **/1**.
	
	El texto que se muestra aquí es el definido en **/LTX** como **/0**. Y también debe existir una entrada de dicha localidad en **/CTL**, ya que dicha entrada o definición, será la que se use más adelante en las secciones de **PROcesos** o **/PRO** de nuestro fichero **DSF**. Ya, explicaremos esto de los **/PRO** más adelante.
