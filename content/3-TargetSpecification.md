### Especificación de destino
Todo lo que hay en la línea de comandos de Nmap que no es una opción (o un argumento de opción) se trata como una especificación del host de destino. El caso más simple es especificar una dirección IP de destino o un nombre de host para escanear.

Cuando se proporciona un nombre de host como destino, se resuelve a través del Sistema de nombres de dominio (DNS) para determinar la dirección IP a escanear. Si el nombre se resuelve en más de una dirección IP, solo se escaneará la primera. Para hacer que Nmap escanee todas las direcciones resueltas en lugar de solo la primera, use la opción `--resolve-all`.

A veces desea escanear una red completa de hosts adyacentes. Para ello, Nmap admite direccionamiento estilo CIDR. Puede agregar `/<numbits>` a una dirección IP o nombre de host y Nmap escaneará cada dirección IP cuyos primeros `<numbits>` sean los mismos que los de la IP de referencia o nombre de host proporcionado. Por ejemplo, `192.168.10.0/24` escanearía los 256 hosts entre `192.168.10.0 (binario: 11000000 10101000 00001010 00000000)` y `192.168.10.255 (binario: 11000000 10101000 0001010 11111111)`, inclusive. `192.168.10.40/24` escanearía exactamente los mismos objetivos. Dado que el host scanme.nmap.org está en la dirección IP `64.13.134.52`, la especificación scanme.nmap.org/16 escanearía las 65.536 direcciones IP entre `64.13.0.0` y `64.13.255.255`. El valor más pequeño permitido es `/0`, que apunta a todo Internet. El valor más grande para IPv4 es `/32`, que escanea solo el host nombrado o la dirección IP porque todos los bits de dirección son fijos. El valor más grande para IPv6 es `/128`, que hace lo mismo.

La notación CIDR es breve pero no siempre lo suficientemente flexible. Por ejemplo, es posible que desee escanear `192.168.0.0/16` pero omitir las IP que terminen en `.0` o `.255` porque pueden usarse como red de subred y direcciones de transmisión. Nmap admite esto mediante direccionamiento por rango de octetos. En lugar de especificar una dirección IP normal, puede especificar una lista de números o rangos separados por comas para cada octeto. Por ejemplo, `192.168.0-255.1-254` omitirá todas las direcciones en el rango que terminen en `.0` o` .255`, y `192.168.3-5,7.1` escaneará las cuatro direcciones `192.168.3.1`, `192.168.4.1`, `192.168.5.1` y `192.168.7.1`. Se puede omitir cualquiera de los lados de un rango; los valores predeterminados son 0 a la izquierda y 255 a la derecha. Usar `-` por sí solo es lo mismo que` 0-255`, pero recuerde usar `0-` en el primer octeto para que la especificación de destino no parezca una opción de línea de comandos. No es necesario limitar los rangos a los octetos finales: el especificador `0-255.0-255.13.37` realizará un escaneo en Internet para todas las direcciones IP que terminen en `13.37.` Este tipo de muestreo amplio puede resultar útil para encuestas e investigaciones en Internet.

Las direcciones IPv6 se pueden especificar mediante su dirección IPv6 completa o nombre de host o con notación CIDR para subredes. Los rangos de octetos aún no son compatibles con IPv6.

Las direcciones IPv6 con alcance no global deben tener un sufijo de ID de zona. En sistemas Unix, es un signo de porcentaje seguido de un nombre de interfaz; una dirección completa podría ser `fe80::a8bb:ccff:fedd:eeff%eth0`. En Windows, utilice un número de índice de interfaz en lugar de un nombre de interfaz: `fe80::a8bb:ccff:fedd:eeff%1`. Puede ver una lista de índices de interfaz ejecutando el comando netsh.exe interface ipv6 show interface.

Nmap acepta múltiples especificaciones de host en la línea de comando y no es necesario que sean del mismo tipo. El comando nmap scanme.nmap.org `192.168.0.0/8 10.0.0,1,3-7`.`-` hace lo que cabría esperar.

Si bien los objetivos generalmente se especifican en las líneas de comando, las siguientes opciones también están disponibles para controlar la selección de objetivos:

`-iL <nombre de archivo de entrada>` (entrada de la lista)
Lee las especificaciones de destino de <inputfilename>. Pasar una lista enorme de hosts suele resultar incómodo en la línea de comandos, pero es un deseo común. Por ejemplo, su servidor DHCP podría exportar una lista de 10.000 arrendamientos actuales que desea escanear. O tal vez desee escanear todas las direcciones IP excepto aquellas para localizar hosts que utilizan direcciones IP estáticas no autorizadas. Simplemente genere la lista de hosts para escanear y pase ese nombre de archivo a Nmap como argumento para la opción -iL. Las entradas pueden estar en cualquiera de los formatos aceptados por Nmap en la línea de comando (dirección IP, nombre de host, CIDR, IPv6 o rangos de octetos). Cada entrada debe estar separada por uno o más espacios, tabulaciones o nuevas líneas. Puede especificar un guión (-) como nombre de archivo si desea que Nmap lea hosts desde la entrada estándar en lugar de un archivo real.

El archivo de entrada puede contener comentarios que comiencen con # y se extiendan hasta el final de la línea.

`-iR <num hosts>` (Elija objetivos aleatorios)
Para encuestas en Internet y otras investigaciones, es posible que desee elegir objetivos al azar. El argumento <num hosts> le dice a Nmap cuántas IP generar. Las IP no deseadas, como aquellas en ciertos rangos de direcciones privadas, de multidifusión o no asignadas, se omiten automáticamente. El argumento 0 se puede especificar para un escaneo interminable. Tenga en cuenta que algunos administradores de red se enfadan ante los análisis no autorizados de sus redes y pueden quejarse. ¡Utilice esta opción bajo su propio riesgo! Si se aburre mucho una tarde lluviosa, pruebe el comando nmap -Pn -sS -p 80 -iR 0 --open para localizar servidores web aleatorios para navegar.

`--exclude <host1>[,<host2>[,...]]` (Excluir hosts/redes)
Especifica una lista de objetivos separados por comas que se excluirán del análisis incluso si forman parte del rango de red general que especifique. La lista que usted pasa utiliza la sintaxis normal de Nmap, por lo que puede incluir nombres de host, bloques de red CIDR, rangos de octetos, etc. Esto puede ser útil cuando la red que desea escanear incluye servidores intocables de misión crítica, sistemas que se sabe que reaccionan de manera adversa a escaneos de puertos o subredes administradas por otras personas.

`--excludefile <exclude_file>` (Excluir lista del archivo)
Esto ofrece la misma funcionalidad que la opción `--exclude`, excepto que los destinos excluidos se proporcionan en un <exclude_file> delimitado por nueva línea, espacio o tabulaciones en lugar de en la línea de comando.

El archivo de exclusión puede contener comentarios que comiencen con # y se extiendan hasta el final de la línea.

`-n` (Sin resolución DNS)
Le dice a Nmap que nunca realice una resolución DNS inversa en las direcciones IP activas que encuentre. Dado que el DNS puede ser lento incluso con el solucionador de código auxiliar paralelo integrado de Nmap, esta opción puede reducir los tiempos de escaneo.

`-R` (resolución DNS para todos los objetivos)
Le dice a Nmap que siempre realice una resolución DNS inversa en las direcciones IP de destino. Normalmente, el DNS inverso sólo se realiza contra hosts receptivos (en línea).

`--resolve-all` (Escanea cada dirección resuelta)
Si un destino de nombre de host se resuelve en más de una dirección, escanéelas todas. El comportamiento predeterminado es escanear solo la primera dirección resuelta. De todos modos, solo se escanearán las direcciones de la familia de direcciones adecuada: IPv4 de forma predeterminada, IPv6 con `-6`.

`--unique` (Escanea cada dirección solo una vez)
Escanee cada dirección IP solo una vez. El comportamiento predeterminado es escanear cada dirección tantas veces como se especifica en la lista de objetivos, como cuando los rangos de red se superponen o diferentes nombres de host se resuelven en la misma dirección.

`--system-dns` (Usa el solucionador de DNS del sistema)
De forma predeterminada, Nmap resuelve inversamente las direcciones IP enviando consultas directamente a los servidores de nombres configurados en su host y luego escuchando las respuestas. Se realizan muchas solicitudes (a menudo docenas) en paralelo para mejorar el rendimiento. Especifique esta opción para utilizar el solucionador de su sistema (una IP a la vez mediante la llamada `getnameinfo`). Esto es más lento y rara vez es útil a menos que encuentre un error en el solucionador paralelo de Nmap (avísenos si lo encuentra). El solucionador del sistema siempre se utiliza para búsquedas directas (obtener una dirección IP de un nombre de host).

`--dns-servers <servidor1>[,<servidor2>[,...]]` (Servidores a utilizar para consultas DNS inversas)
De forma predeterminada, Nmap determina sus servidores DNS (para resolución rDNS) a partir de su archivo resolv.conf (Unix) o el Registro (Win32). Alternativamente, puede utilizar esta opción para especificar servidores alternativos. Esta opción no se acepta si está utilizando `--system-dns`. El uso de varios servidores DNS suele ser más rápido, especialmente si elige servidores autorizados para su espacio IP de destino. Esta opción también puede mejorar el sigilo, ya que sus solicitudes pueden rebotar en casi cualquier servidor DNS recursivo de Internet.

Esta opción también resulta útil al escanear redes privadas. A veces, sólo unos pocos servidores de nombres proporcionan información rDNS adecuada y es posible que ni siquiera sepas dónde están. Puede escanear la red en busca del puerto 53 (quizás con detección de versión), luego intente escanear la lista Nmap (`-sL`) especificando cada servidor de nombres uno a la vez con `--dns-servers` hasta que encuentre uno que funcione.

Es posible que esta opción no se respete si la respuesta DNS excede el tamaño de un paquete UDP. En tal situación, nuestro solucionador de DNS hará el mejor esfuerzo para extraer una respuesta del paquete truncado y, si no tiene éxito, recurrirá al solucionador del sistema. Además, las respuestas que contengan alias CNAME recurrirán al solucionador del sistema.
