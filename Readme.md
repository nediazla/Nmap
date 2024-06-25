Nmap (“Network Mapper”) es una herramienta de código abierto para exploración de redes y auditoría de seguridad. Fue diseñado para escanear rápidamente redes grandes, aunque funciona bien contra hosts individuales. Nmap utiliza paquetes IP sin formato de formas novedosas para determinar qué hosts están disponibles en la red, qué servicios (nombre y versión de la aplicación) ofrecen esos hosts, qué sistemas operativos (y versiones de SO) están ejecutando, qué tipo de filtros/firewalls de paquetes están en uso, y docenas de otras características. Si bien Nmap se usa comúnmente para auditorías de seguridad, muchos administradores de sistemas y redes lo encuentran útil para tareas rutinarias como inventario de red, administración de programas de actualización de servicios y monitoreo del tiempo de actividad del host o del servicio.

El resultado de Nmap es una lista de objetivos escaneados, con información complementaria sobre cada uno dependiendo de las opciones utilizadas. La clave entre esa información es la “tabla de puertos interesantes”. Esa tabla enumera el número de puerto y el protocolo, el nombre del servicio y el estado. El estado puede ser abierto, filtrado, cerrado o sin filtrar. Abierto significa que una aplicación en la máquina de destino está escuchando conexiones/paquetes en ese puerto. Filtrado significa que un firewall, filtro u otro obstáculo de la red está bloqueando el puerto de modo que Nmap no puede saber si está abierto o cerrado. Los puertos cerrados no tienen ninguna aplicación escuchando, aunque podrían abrirse en cualquier momento. Los puertos se clasifican como sin filtrar cuando responden a las sondas de Nmap, pero Nmap no puede determinar si están abiertos o cerrados. Nmap informa las combinaciones de estados abierto|filtrado y cerrado|filtrado cuando no puede determinar cuál de los dos estados describe un puerto. La tabla de puertos también puede incluir detalles de la versión del software cuando se ha solicitado la detección de la versión. Cuando se solicita una exploración del protocolo IP (`-sO`), Nmap proporciona información sobre los protocolos IP admitidos en lugar de los puertos de escucha.

Además de la interesante tabla de puertos, Nmap puede proporcionar más información sobre los objetivos, incluidos nombres DNS inversos, conjeturas de sistemas operativos, tipos de dispositivos y direcciones MAC.

Un escaneo representativo de Nmap
```shell-session
# nmap -A -T4 scanme.nmap.org

Nmap scan report for scanme.nmap.org (74.207.244.221)
Host is up (0.029s latency).
rDNS record for 74.207.244.221: li86-221.members.linode.com
Not shown: 995 closed ports
PORT     STATE    SERVICE     VERSION
22/tcp   open     ssh         OpenSSH 5.3p1 Debian 3ubuntu7 (protocol 2.0)
| ssh-hostkey: 1024 8d:60:f1:7c:ca:b7:3d:0a:d6:67:54:9d:69:d9:b9:dd (DSA)
|_2048 79:f8:09:ac:d4:e2:32:42:10:49:d3:bd:20:82:85:ec (RSA)
80/tcp   open     http        Apache httpd 2.2.14 ((Ubuntu))
|_http-title: Go ahead and ScanMe!
646/tcp  filtered ldp
1720/tcp filtered H.323/Q.931
9929/tcp open     nping-echo  Nping echo
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6.39
OS details: Linux 2.6.39
Network Distance: 11 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
[Cut first 10 hops for brevity]
11  17.65 ms li86-221.members.linode.com (74.207.244.221)

Nmap done: 1 IP address (1 host up) scanned in 14.40 seconds
```
### Resumen de opciones
Este resumen de opciones se imprime cuando se ejecuta Nmap sin argumentos y la última versión siempre está disponible en https://svn.nmap.org/nmap/docs/nmap.usage.txt. Ayuda a las personas a recordar las opciones más comunes, pero no sustituye la documentación detallada del resto de este manual. Algunas opciones oscuras ni siquiera están incluidas aquí

```shell-sesion
Nmap 7.93SVN ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports sequentially - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```

### Especificación de destino
Todo lo que hay en la línea de comandos de Nmap que no es una opción (o un argumento de opción) se trata como una especificación del host de destino. El caso más simple es especificar una dirección IP de destino o un nombre de host para escanear.

Cuando se proporciona un nombre de host como destino, se resuelve a través del Sistema de nombres de dominio (DNS) para determinar la dirección IP a escanear. Si el nombre se resuelve en más de una dirección IP, solo se escaneará la primera. Para hacer que Nmap escanee todas las direcciones resueltas en lugar de solo la primera, use la opción --resolve-all.

A veces desea escanear una red completa de hosts adyacentes. Para ello, Nmap admite direccionamiento estilo CIDR. Puede agregar /<numbits> a una dirección IP o nombre de host y Nmap escaneará cada dirección IP cuyos primeros <numbits> sean los mismos que los de la IP de referencia o nombre de host proporcionado. Por ejemplo, 192.168.10.0/24 escanearía los 256 hosts entre 192.168.10.0 (binario: 11000000 10101000 00001010 00000000) y 192.168.10.255 (binario: 11000000 10101000 0001010 11111111), inclusive. 192.168.10.40/24 escanearía exactamente los mismos objetivos. Dado que el host scanme.nmap.org está en la dirección IP 64.13.134.52, la especificación scanme.nmap.org/16 escanearía las 65.536 direcciones IP entre 64.13.0.0 y 64.13.255.255. El valor más pequeño permitido es /0, que apunta a todo Internet. El valor más grande para IPv4 es /32, que escanea solo el host nombrado o la dirección IP porque todos los bits de dirección son fijos. El valor más grande para IPv6 es /128, que hace lo mismo.

La notación CIDR es breve pero no siempre lo suficientemente flexible. Por ejemplo, es posible que desee escanear 192.168.0.0/16 pero omitir las IP que terminen en .0 o .255 porque pueden usarse como red de subred y direcciones de transmisión. Nmap admite esto mediante direccionamiento por rango de octetos. En lugar de especificar una dirección IP normal, puede especificar una lista de números o rangos separados por comas para cada octeto. Por ejemplo, 192.168.0-255.1-254 omitirá todas las direcciones en el rango que terminen en .0 o .255, y 192.168.3-5,7.1 escaneará las cuatro direcciones 192.168.3.1, 192.168.4.1, 192.168.5.1 y 192.168.7.1. Se puede omitir cualquiera de los lados de un rango; los valores predeterminados son 0 a la izquierda y 255 a la derecha. Usar - por sí solo es lo mismo que 0-255, pero recuerde usar 0- en el primer octeto para que la especificación de destino no parezca una opción de línea de comandos. No es necesario limitar los rangos a los octetos finales: el especificador 0-255.0-255.13.37 realizará un escaneo en Internet para todas las direcciones IP que terminen en 13.37. Este tipo de muestreo amplio puede resultar útil para encuestas e investigaciones en Internet.

Las direcciones IPv6 se pueden especificar mediante su dirección IPv6 completa o nombre de host o con notación CIDR para subredes. Los rangos de octetos aún no son compatibles con IPv6.

Las direcciones IPv6 con alcance no global deben tener un sufijo de ID de zona. En sistemas Unix, es un signo de porcentaje seguido de un nombre de interfaz; una dirección completa podría ser fe80::a8bb:ccff:fedd:eeff%eth0. En Windows, utilice un número de índice de interfaz en lugar de un nombre de interfaz: fe80::a8bb:ccff:fedd:eeff%1. Puede ver una lista de índices de interfaz ejecutando el comando netsh.exe interface ipv6 show interface.

Nmap acepta múltiples especificaciones de host en la línea de comando y no es necesario que sean del mismo tipo. El comando nmap scanme.nmap.org 192.168.0.0/8 10.0.0,1,3-7.- hace lo que cabría esperar.

Si bien los objetivos generalmente se especifican en las líneas de comando, las siguientes opciones también están disponibles para controlar la selección de objetivos:

-iL <nombre de archivo de entrada> (entrada de la lista)
Lee las especificaciones de destino de <inputfilename>. Pasar una lista enorme de hosts suele resultar incómodo en la línea de comandos, pero es un deseo común. Por ejemplo, su servidor DHCP podría exportar una lista de 10.000 arrendamientos actuales que desea escanear. O tal vez desee escanear todas las direcciones IP excepto aquellas para localizar hosts que utilizan direcciones IP estáticas no autorizadas. Simplemente genere la lista de hosts para escanear y pase ese nombre de archivo a Nmap como argumento para la opción -iL. Las entradas pueden estar en cualquiera de los formatos aceptados por Nmap en la línea de comando (dirección IP, nombre de host, CIDR, IPv6 o rangos de octetos). Cada entrada debe estar separada por uno o más espacios, tabulaciones o nuevas líneas. Puede especificar un guión (-) como nombre de archivo si desea que Nmap lea hosts desde la entrada estándar en lugar de un archivo real.

El archivo de entrada puede contener comentarios que comiencen con # y se extiendan hasta el final de la línea.

-iR <num hosts> (Elija objetivos aleatorios)
Para encuestas en Internet y otras investigaciones, es posible que desee elegir objetivos al azar. El argumento <num hosts> le dice a Nmap cuántas IP generar. Las IP no deseadas, como aquellas en ciertos rangos de direcciones privadas, de multidifusión o no asignadas, se omiten automáticamente. El argumento 0 se puede especificar para un escaneo interminable. Tenga en cuenta que algunos administradores de red se enfadan ante los análisis no autorizados de sus redes y pueden quejarse. ¡Utilice esta opción bajo su propio riesgo! Si se aburre mucho una tarde lluviosa, pruebe el comando nmap -Pn -sS -p 80 -iR 0 --open para localizar servidores web aleatorios para navegar.

--exclude <host1>[,<host2>[,...]] (Excluir hosts/redes)
Especifica una lista de objetivos separados por comas que se excluirán del análisis incluso si forman parte del rango de red general que especifique. La lista que usted pasa utiliza la sintaxis normal de Nmap, por lo que puede incluir nombres de host, bloques de red CIDR, rangos de octetos, etc. Esto puede ser útil cuando la red que desea escanear incluye servidores intocables de misión crítica, sistemas que se sabe que reaccionan de manera adversa a escaneos de puertos o subredes administradas por otras personas.

--excludefile <exclude_file> (Excluir lista del archivo)
Esto ofrece la misma funcionalidad que la opción --exclude, excepto que los destinos excluidos se proporcionan en un <exclude_file> delimitado por nueva línea, espacio o tabulaciones en lugar de en la línea de comando.

El archivo de exclusión puede contener comentarios que comiencen con # y se extiendan hasta el final de la línea.

-n (Sin resolución DNS)
Le dice a Nmap que nunca realice una resolución DNS inversa en las direcciones IP activas que encuentre. Dado que el DNS puede ser lento incluso con el solucionador de código auxiliar paralelo integrado de Nmap, esta opción puede reducir los tiempos de escaneo.

-R (resolución DNS para todos los objetivos)
Le dice a Nmap que siempre realice una resolución DNS inversa en las direcciones IP de destino. Normalmente, el DNS inverso sólo se realiza contra hosts receptivos (en línea).

--resolve-all (Escanea cada dirección resuelta)
Si un destino de nombre de host se resuelve en más de una dirección, escanéelas todas. El comportamiento predeterminado es escanear solo la primera dirección resuelta. De todos modos, solo se escanearán las direcciones de la familia de direcciones adecuada: IPv4 de forma predeterminada, IPv6 con -6.

--unique (Escanea cada dirección solo una vez)
Escanee cada dirección IP solo una vez. El comportamiento predeterminado es escanear cada dirección tantas veces como se especifica en la lista de objetivos, como cuando los rangos de red se superponen o diferentes nombres de host se resuelven en la misma dirección.

--system-dns (Usa el solucionador de DNS del sistema)
De forma predeterminada, Nmap resuelve inversamente las direcciones IP enviando consultas directamente a los servidores de nombres configurados en su host y luego escuchando las respuestas. Se realizan muchas solicitudes (a menudo docenas) en paralelo para mejorar el rendimiento. Especifique esta opción para utilizar el solucionador de su sistema (una IP a la vez mediante la llamada getnameinfo). Esto es más lento y rara vez es útil a menos que encuentre un error en el solucionador paralelo de Nmap (avísenos si lo encuentra). El solucionador del sistema siempre se utiliza para búsquedas directas (obtener una dirección IP de un nombre de host).

--dns-servers <servidor1>[,<servidor2>[,...]] (Servidores a utilizar para consultas DNS inversas)
De forma predeterminada, Nmap determina sus servidores DNS (para resolución rDNS) a partir de su archivo resolv.conf (Unix) o el Registro (Win32). Alternativamente, puede utilizar esta opción para especificar servidores alternativos. Esta opción no se acepta si está utilizando --system-dns. El uso de varios servidores DNS suele ser más rápido, especialmente si elige servidores autorizados para su espacio IP de destino. Esta opción también puede mejorar el sigilo, ya que sus solicitudes pueden rebotar en casi cualquier servidor DNS recursivo de Internet.

Esta opción también resulta útil al escanear redes privadas. A veces, sólo unos pocos servidores de nombres proporcionan información rDNS adecuada y es posible que ni siquiera sepas dónde están. Puede escanear la red en busca del puerto 53 (quizás con detección de versión), luego intente escanear la lista Nmap (-sL) especificando cada servidor de nombres uno a la vez con --dns-servers hasta que encuentre uno que funcione.

Es posible que esta opción no se respete si la respuesta DNS excede el tamaño de un paquete UDP. En tal situación, nuestro solucionador de DNS hará el mejor esfuerzo para extraer una respuesta del paquete truncado y, si no tiene éxito, recurrirá al solucionador del sistema. Además, las respuestas que contengan alias CNAME recurrirán al solucionador del sistema.

