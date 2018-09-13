# Practica1_SemaforoVP
S I S T E M A   A L E R T I Z A D O R   S A L V A G U A R D I A   H U M A N O - C O C H E 
              

Descripción de la práctica
Circuito programado desde microcontralador Arduino. Posee botones para dar paso peatonal, mientras no se oprima el boton, el semaforo de vehiculos continua en verde para un mejor flujo vehicular.

Descripcion del problema
Realizar un sistema que simule un Semáforo interactivo usando Arduino. Este debe mostrar un conjunto de semaforos que cambiaran de verde a ámbar a rojo y viceversa, luedo de un periodo de tiempo establecido, utilizando el sistema de cuatro estados de los semáforos en México. Este se extiende para incluir un conjunto de luces y un botón peatonal para solicitar cruzar la calle. Cuando llega el peatón y se dispone a cruzar, pulsa el botón que se encuentra en la parte baja del semáforo, este debe reconocer la orden y cierra el paso de los vehiculos para que el viandante pueda cruzar con seguridad hasta el otro lado de la calle. Una vez qu se acaba el tiempo estipulado para que el peaton cruce, ese semaforo permanecerá abierto para mejorar la movibilidad de los vehículos. El sitema deberá contener una perilla para controlar el tiempo mínimo en que el semáforo vehicular va a durar en verde.

Material 
Microcontrolador Arduino UNO

Led verde (3)

Led ambar (1)

Led Rojo (3)

Resistencias 330k (3)

Resistencias 270k (4)

Resistencia 10k (2)

Push buton (2)

Potenciometro (1)

Jumpers

protoboard

CABLEADO DEL CIRCUITO     
Semáforo vehicular.

Colocar los led en su respectivo orden, iniciando por la izquierda: Rojo, Ambar, Verde
Cada led, deberá ir conectado a una resistencia de 330komh de la "patita" de voltaje, la otra "patita" se conecta a tierra en la proto.
Del lado del nodo de la resistencias con el led, con ayuda de jumpers, conectar a una salida DIGITAL, cada led, en el microcontrolador. 3.1 Led_verde = pin 2, Led_amarillo = pin 3, Led_Rojo = pin 4
Semáforo peatonal

En otro lado de la protoboard, conectar dos conjuntos de led posicionados uno frente al otro, cada uno con un led verde y uno rojo.
De lado del voltaje de cada led, conectar una resistencia de 270komh y de la otra "patita", conectar a tierra.
En el nodo que une la resistencias con el led, con un jumper, conectar a un pin como salida DIGITAL en el microcontrolador. 3.1 Verde1 = pin 5, Verde2 = pin 7, Rojo1 = pin 6, Rojo2 = pin 8
Botones

Colocamos dos botones a un lado de los semáforos peatonales.
Para ambos botones, una "patita" conectarla a una resistencias de 10komh y estas a su vez, a voltaje, la otra "patita del botón, conectar a tierra.
Del lado del nodo del boton con la resistencias, con ayuda de jumpers, conectar a un pin como salida DIGITAL del microcontrolador. 3.1 Boton1 = pin 9, boton2 = pin 10.
Potenciometro

El potenciometro es una resistencia variable que cuenta con 3 pines, los que se encuentran al extremo, uno se deberá conectar a tierra y el otro a voltaje.
El pin del centro se conecta, con ayuda de un jumper, a un pin como entrada ANALOGA. 2.1 Pote = pin 1
General

Se debe conectar, con ayuda de jumpers, el microcontrolador a voltaje desde el pin de POWER en 5V

Se debe conectar, con ayuda de jumpers, el microcontrolador a voltaje desde el pin de DIGITAL en GROUND

Al utilizar toda la protoboard, se debe "puentear" para que toda la proto este energizada, esto se hace conectando positivo con positivo y negativo con negativo. ** Nota: Se utilizaron jumpers y cables de diferente color para identificar facilmente cada componente, para el caso de los Led, se usaron del mismo color que los leds; para conexión a voltaje, se usan cables color anaranjado; para conexión a tierra, colorverde; para los botones, color negro; para el potenciometro, color blanco.

 PROGRAMACIÓN
-- Semáforo vehicular

Se definen cada uno de los led con sus respectivos pines a los que estan conectados y el potenciometro, ya que con este manejaremos el tiempo en que dura el paso vehicular (led verde prendido).

#define led_verde 2
#define led_amarillo 3
#define led_rojo 4
#define pote 1 
1.1 Añadiremos un valor de tipo entero el que inicializaremos en 0, esto será para almacenar el valor que vaya tomando el potenciomentro conforme movamos la perilla. ** Verificar que el núumero asignado en el Sketch coincida con el pin al que esta conectado en el microcontrolador.

int valor_pote = 0;
Dentro del void setup, declaramos cada led como salida, esto indica que sera utilizado como salida digital, es decir, emanará luz. El potenciometro no es necesario indicarlo.

pinMode(led_verde,OUTPUT); pinMode(led_amarillo, OUTPUT); pinMode(led_rojo,OUTPUT);

Primer estado: Verde. Indica al conductor que puede pasar sin ningun problema. Se utilizará un void para controlar el manejo de cada estado.

  void verde () { 
3.1 Activamos el led en alto (HIGH) ya que deberá iniciar el circuito dando el paso al vehículo.

  digitalWrite(led_verde,HIGH);   //alto
3.2 Para controlar el tiempo de espera del led verde, se indica primero una variable que será en donde se guarde el valor del potenciometro por lo que debe de ser de tipo entero inicializandola en cero.

  int valor_pote = 0;
3.2.1 Para convertir el valor recibido en una señar de lectura analoga se necesita de hacer indicarsela a la variable creada en el punto anterior. Esto se puede hacer dentro del void loop para que pueda ser manejado en cualquier momento.

void loop () {
  valor_pote = analogRead(pote);
  }
3.2.2 Con las intrucciones hechas para el control del potenciomentro, agregaremos un tiempo de duracion dentro del estado verde, este tiempo será dictado por el valor que vaya tomanto el potenciometro.

     delay(valor_pote); //valor del potenciometro
3.3 Una vez que pase dicho tiempo debera apagarse (LOW) para dar paso al siguiente estado.

  digitalWrite(led_verde, LOW);     
  }
Segundo estado: parpadeo preventivo verde. Le indica al conductor que cruce con precaución ya que pronto estará en un alto total. Al igual que el verde, usaremos un void.

void preventivaVerde (){
4.1 Se utilizará un ciclo for para controlar el encendido y apagado del led verde, esto se hará 3 veces. Inicia el led predido y posteriormente, comienza a parpadear 3 veces. El tiempo en que dura encendido el led antes de comenzar a parpadear (2do estado), será controlado por el valor que tome el potenciometro. Dentro del ciclo, se declara una variable de tipo entero la cual funcionará como contador del encendido y apagado, cuanto este llegue a 3, termina este estado. Se debe declarar primero el led en estado alto (HIGH) para el encendido asignando un tiempo de duración del encendido, posteriormente, se declara el mismo led, en estado bajo (LOW) y de igual manera, un tiempo asignado en el que debera estar apagado.

digitalWrite(led_verde,HIGH);   //alto
delay(valor_pote); //31500 Es el tiempo real que dura un semaforo

digitalWrite(led_verde, LOW); //debe apagarse antes de comenzar con el 2do estado, parpadeo

//parpadeo
for(int i = 0; i < 3; i++) {

digitalWrite(led_verde, HIGH); //alto
delay (500);        // tiempo de duracion en alto

digitalWrite(led_verde,LOW); //bajo
delay(500);        // tiempo de duracion en bajo
} //fin for
3er Estado: Amarillo/ambar. Indica al conductor que vaya frenando para que en unos segundos se encuentre en alto total. Se utilizará un void exclusivo para este estado, como en los otros 2.
void amarillo (){
5.1 Para este estado, solo es necesario indicar el led ambar en un estado alto (HIGH) en un tiempo determinado y en un estado bajo (LOW). Para el estado bajo, no se debe configurar un tiempo ya que si se hace, al finalizar dicho tiempo, se prenderá nuevamente el led y la funcionalidad es que al momento de aparse, no se encienda hasta que haga el ciclo del semaforo nuevamente. Este estado, pasa rapidamente al siguiente estado por lo que no es necesario configurar algo mas.

 digitalWrite(led_amarillo, HIGH);  //alto
 delay(3000); //tiempo en que se encuentra encendido

digitalWrite(led_amarillo,LOW); 
4to Estado: Rojo. Indica un alto total ya que se activará el paso para la via perpendicular. Es el último estado de un semaforo, se usa, de igual manera, un void para su control.
void rojo (){
6.1 Al igual que el tercer estado, se necesita unicamente encendido (HIGH) y apagado (LOW). Por el momento, no se agrega un tiempo de espera, dicho tiempo será configurado dentro de la programación del paso peatonal que se ve en el punto 3 del siquiente apartado.

digitalWrite(led_rojo, HIGH); //alto
digitalWrite(led_rojo,LOW); //bajo
** Para comprobar la funcionalidad del semáforo, dentro del void loop, se mandan llamar cada uno de los estados.

void loop() { 
    verde();
    preventivaVerder();
    amarillo();   // al activarse se ve la funcionalidad normal de semaforos vehicular y peatonal
    rojo(); 
  }


-- Semáforo peatonal 
Cada una de las instrucciones se adicionan a las ya indicadas anteriormente.

Se definen cada uno de los led con sus respectivos pines a los que estan conectados. Recordemos que son 2 semaforos peatonales.

#define verde1 5  //paso 1
#define rojo1 6 
#define verde2 7 //paso 2
#define rojo2 8
Ya que los semaforos peatonales deben de estar sincronizados con los semáforos vehiculares, la programacion de estos, estará dentro de los diferentes estados del semáforo vehicular. Dentro del primer estado: Verde, se activan los led rojos, ya que mientras el vehículo tenga paso, los peatones no deben de pasar. No se configura tiempo de duración ya que estan conectados a los estados del semaforo vehicular y cambiaran con estos.

void verde (){ digitalWrite(rojo1,HIGH); //alto peaton digitalWrite(rojo2,HIGH);

2.1 Dentro el segundo estado: preventivaVerde, fuera del ciclo for, se configurará de igual manera el semaforo peatonal en rojo ya que aun no tienen un cruzo seguro.

  void preventivaVerde (){

      digitalWrite(rojo1,HIGH); //alto peaton
      digitalWrite(rojo2,HIGH);
  }
** Nota: No es necesario configurar en semáforo peatonal en el 3er estado (amarillo) ya que como no se indico un apagado en el estado del led verde, este seguira predido hasta que se le indique lo contrario.

En el cuarto y último estado: rojo. Apagaremos (LOW) el led rojo y encenderemos el verde (HIGH) de los semaforos de peatón ya que al estar el semáforo vehícular en rojo, el peatón tiene un paso seguro hacia el otro extremo.
void rojo () {
   digitalWrite(rojo1,LOW);  //peaton
   digitalWrite(rojo2,LOW);
   
   digitalWrite(verde1,HIGH); // paso peaton
   digitalWrite(verde2,HIGH);
3.1 Se agregan dos tiempos de duracion, ya que deben de comenzar a parpadear como preveción al peaton para indicar que los vehículos comenzarán a pasar. Se divide en dos tiempos ya que el parpadeo debe debe de ser unos segundos antes de que termine el tiempo de rojo del paso de vehículos, la suma de ambos tiempos de espera, es de 34 ya que es lo que dura el semaforo vehícular en rojo.

   delay(15000); // la suma de ambos es 34, el tiempo real de duración de los semaforos
   delay(17000); //
 }
3.2 Para realizar el parpadeo preventivo para los peatones, se utilizarán un ciclo for a 3 tiempos, tal como se hizo en el primer estado del semáforo vehícular. Se manejan los dos led a la par, con un tiempo de duracion igual tanto en encendido (HIGH) como en bajo (LOW)

 //parpadear 
for(int i = 0; i<3; i++){
  digitalWrite(verde1,HIGH);  //paso peatonal
  digitalWrite(verde2,HIGH); 
  delay (500);

 digitalWrite(verde1,LOW);  //paso peatonal
 digitalWrite(verde2,LOW); 
 delay(500);
}
3.3 Despues de que se completa el ciclo de prevencion para los peatones, el led verde deberá volver a estar apagado.

digitalWrite(verde1,LOW); //peaton
digitalWrite(verde2,LOW);
}
** Con el mismo metodo que se inicio en el void loop se podrá ver la funcionalidad de ambos semáforos funcionando a la par.

-- Botones: Control de paso peatonal.
Cada una de las instrucciones se adicionan a las ya indicadas anteriormente.

El botón se configurará para dar paso al peaton, dejando el semáforo del vehículo en rojo para que el viandante, pueda pasar seguro hacia el otro extremo una vez que haya presionado el botón. Está configuración se realizará directamente en el void loop para que este disponible desde la iniciación del semaforo. Dentro de la definición del problema, se plantea que el semaforo vehícular debe quedar abierto para no generar tráfico innecesario, es decir, solo se podrá el rojo cuando un peaton desee cruzar. Para esto, se deberá eliminar o comentar las inicializaciones de todos los estados, excepto el primero (verde), ya que este es el que indicará que los vehículos pueden pasar sin ningún problema.
void loop () {
    verde();
  //preventivaVerde();
 // amarillo();   // al activarse se ve la funcionalidad normal de semaforos vehicular y peatonal
//  rojo(); 
1.1 Se definen cada uno de los botones con sus respectivos pines a los que estan conectados. Estas serán de tipo entero ya que posteriormente se indicara un numero para controlar cuando el boton sea presionado.

  int boton1 = 9;
  int boton2 = 10;
1.2 Se indicará un manejo del botón, este para ser convertido a una señal de LECTURA ANALOGA y poder leer la señal recibida.

  int push1 = 0;
  int push2 = 0 ;
1.3 Se indica un control para almacenar ahí un número, el cual pueda ser manejado posteriormente como indicador de que fue presionado el botón y pueda acceder las operaciones necesarias.

  int control = 0;
Dentro del void setup, se indica que los botones seran de tipo INPUT indicando con esto, que recibiran una señal.

pinMode(boton1, INPUT);
pinMode(boton2, INPUT);
Dentro del void loop declaramos nuestras variables push en bajo (LOW) para comenzar con el manejo de los botones.

void loop(){
   push1 = LOW;//boton1
   push2 = LOW; // boton2
Convertimos la señal que reciben los botones en señal de lectura digital, esta es se almacenará en nuestra variable PUSH que será la que manejaremos para el control de los botones.

push1 = digitalRead(boton1);
push2 = digitalRead(boton2);
Para el control de los botones, lo manejaremos como 1 cuando sea apretado y 0 cuando no, para esto usaremos una condicional if, si el valor de push, ya sea desde el botón uno o dos, es igual a LOW, nuestro control sera igual a 1, lo que significa que el boton recibio una señal de que un peatón desea cruzar, por consiguiente, manda llamar a los metodos preventivaVerde, Amarillo y Rojo. Si se encuentra en 0, quiere decir que no se presionó, por lo que solo debe de llamar al estado verde para mantener el semáforo vehicular abierto.

if(push1 == LOW || push2 == LOW) {
  control = 1; 
} else {control = 0;}   
    if (control == 1) {
        preventivaVerde();
        amarillo();
        rojo();    
} else { if (control = 0) { verde();}}
Una vez concluido todo el proceso, la funcionalidad de nuestro semáforo deberá ser el siguiente:
El semáforo vehicular debe de estar siempre en verde y por consiguiente, el semáforo peatonal, en rojo.
Al presional uno de los dos botones, el semáforo vehicular debe comenzar su ciclo de 4 estados, al llegar al 4to estado, el semáforo peatonal deberá cambiar a verde indicando que puede pasar.
Una vez concluido el ciclo del semáforo peatonal, deberá ponerse en rojo y el semáforo vehicular en verde, iniciando todo el proceso hasta que se presione uno de los botones.
