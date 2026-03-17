# PREGUNTAS LAB 0 - Facundo Tomás Nogués Margarit
## 1. ¿Cómo sabe el cliente que terminó de recibir la respuesta?

El cliente utiliza **HTTP/1.0**, versión en la cual el servidor cierra la conexión al finalizar la respuesta. 
El cliente sabe que la comunicación terminó porque al hacer **recv()** se devuelve un string vacío **''**. Esta es la señal definitiva de que el flujo continuo de bytes (stream) ha sido cerrado por el servidor.

## 2. ¿Qué parte del comportamiento depende de TCP y cuál de HTTP?

El protocolo **TCP** gestiona que la comunicación se dé eficientemente, es decir, que ambas partes se conecten mutuamente, la manera en que éstos bytes van a ser enviados (stream), que no falten pedazos por llegar y que los que si lleguen, lo hagan de manera correcta (íntegramente y en orden) y tambien que la comunicación se cierre correctamente. Es decir, **TCP** garantiza orden e integridad de la información. Por otra parte pero apoyandose en la conexión que otorga **TCP**, **HTTP** define el formato y significado de lo que se está diciendo, es decir, como se van a estructurar los mensajes para que ambas partes logren entenderse, tanto con las peticiones como con las respuestas y las reglas que aplican a la interacción.

## 3. ¿Qué pasaría si el servidor no cerrara la conexión después de enviar la respuesta?

Si el servidor no cierra la conexión tras enviar la respuesta, entonces, como se está trabajando en **TCP**, se trata de un flujo continuo de bytes, lo que generaría que el cliente se quede bloqueado en el **recv()** esperando eternamente más datos que nunca llegarán. Queda así el proceso en un estado de espera infinito.

## 4. ¿Por qué la IP obtenida para el mismo hostname podría ser distinta entre ejecuciones (o entre máquinas)?
La respuesta a ésto son los **centros de datos** y sus correspondientes **puntos de presencia**. Los servidores se encuentran disponible para toda la internet, por lo que si todo el volúmen de consultas fuera a la misma IP, nos encontraríamos ante un grave problema de sobrecarga. Es por ésto que en los centros de datos y en los mencionados anteriormente puntos de presencia o **POPs**, se utiliza el **balanceo de cargas**. Estos últimos fortalecen la descentralización de los servidores y acercan al usuario la información correspondiente en su región. Se utilizan **servidores de borde** para atender usuarios y balanceadores de carga para repartir la carga. Es por éste motivo que la IP puede fluctuar, si bien apunta al sitio que deseamos, depende la región en la que nos encontremos y el tránsito de datos que esté impactando en la red en ése momento.

## 5. ¿Por qué DNS suele usar UDP y HTTP usa TCP? ¿Qué pasaría si usáramos TCP para las consultas DNS en este cliente?

Se suele usar **TCP** para consultas **HTTP** pues necesitamos que el contenido de los mensajes llegue de manera íntegra y ordenada para responder a la estructura **HTTP** y a su vez para los archivos que desplazan los distintos mensajes. Por lo tanto, vale la pena sacrificar un poco de latencia en pos de que el contenido llegue correctamente. Recordemos que **TCP** se encarga de retransmitir automáticamente lo que se perdió. No se menciona el tema de seguridad de la información pues hablamos de **HTTP** y no de **HTTPS**
Mientras que en **DNS** necesitamos consultas rápidas a los sitos, que no generen demoras, y que a su vez en caso de fallos simplemente se reintenten, pues es mucho más ágil la comunicación. También corresponde a carga del servidor, pues si por cada consulta tuviera que establecer el Handshake entonces se sobrecargaría.

Si usáramos **TCP** para las consultas **DNS** en este cliente, tendríamos principalmente un problema de latencia, la resolución de nombres sería más lenta. A su vez, generaríamos una sobrecarga innecesaria en el tamaño del mensaje pues se gastarían más datos en el protocolo que en la respuesta del mismo. La única ventaja sería ante una conexión inestable, **TCP** se aseguraría de que la consulta y la respuesta lleguen correctamente.

## 1.5 Tarea Estrella

**Unicode (UTF-8)** es un estándar de codificación de caracteres, éste asigna a cada uno un identificador único, pero el protocolo **DNS**, originalmente solo permitía caracteres **ASCII** (muy limitado pues solo tenía 128 caracteres posibles). Para resolver ésto, python utiliza la librería **codecs**, específicamente el codec **IDNA (International Domain Names in Application)**, que implementa el **RFC 3490** y **Stringprep** con el **RFC 3454** que define el proceso de preparación de cadenas unicode para protocolos de internet. Estas RFC juntas definen un protocolo para admitir caracteres **no ASCII** en los nombres de dominio.
Con éstos codecs en python, podemos 'codificar' caracteres y convertirlos en bytes compatibles. Ésto gracias al algoritmo **Punycode** sobre el que se apoya el codec **IDNA**, el cual transforma los caracteres especiales en una secuencia de caracteres **ASCII** que comienzan con el prefijo **xn--**, conocida como **ACE (ASCII Compatible Encoding)**.
Otra ventaja es que el módulo socket convierte de forma transparente los nombres de host **Unicode** a **ACE**, por lo que las aplicaciones no necesitan preocuparse por convertir los nombres de host ellos mismos cuando los pasan al módulo de socket.

### En resumen: 

* Paso un **str** como host en **unicode** (con caracteres especiales si quiero).
+ **---- De lo siguiente se encarga el socket ----**
* **strprep()** prepara el texto, lo 'limpia' y normaliza.
* El algoritmo **Punycode** del Codec toma esos caracteres especiales y los transforma en caracteres **ASCII**.
* Se le pega el **xn--** delante para que tenga el formato **ACE**.
* Se manda la consulta DNS.
+ **---- Entra a DNS ----**
* El servidor **DNS** simplemente compara la cadena **ACE** recibida con sus registros **ASCII** y devuelve la **IP** asociada. No necesita entender Unicode.
