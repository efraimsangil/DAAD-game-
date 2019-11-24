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


