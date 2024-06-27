## Detección de servicio y versión

Apunte Nmap a una máquina remota y es posible que le indique que los puertos `25/tcp`, `80/tcp` y `53/udp` están abiertos. Utilizando su base de datos ``nmap-services` de aproximadamente 2200 servicios conocidos, Nmap informaría que esos puertos probablemente corresponden a un servidor de correo (SMTP), un servidor web (HTTP) y un servidor de nombres (DNS), respectivamente. Esta búsqueda suele ser precisa: la gran mayoría de los demonios que escuchan en el puerto TCP 25 son, de hecho, servidores de correo. Sin embargo, ¡no deberías apostar tu seguridad por esto! La gente puede ejecutar servicios en puertos extraños, y lo hace.

Incluso si Nmap tiene razón y el servidor hipotético anterior ejecuta servidores SMTP, HTTP y DNS, no es mucha información. Al realizar evaluaciones de vulnerabilidad (o incluso simples inventarios de red) de sus empresas o clientes, realmente desea saber qué servidores y versiones de correo y DNS se están ejecutando. Tener un número de versión preciso ayuda enormemente a determinar a qué exploits es vulnerable un servidor. La detección de versiones le ayuda a obtener esta información.

Después de que se descubren los puertos TCP y/o UDP utilizando uno de los otros métodos de escaneo, la detección de versión interroga esos puertos para determinar más acerca de lo que realmente se está ejecutando. La base de datos `nmap-service-probes` contiene sondas para consultar varios servicios y expresiones coincidentes para reconocer y analizar respuestas. Nmap intenta determinar el protocolo de servicio (por ejemplo, FTP, SSH, Telnet, HTTP), el nombre de la aplicación (por ejemplo, ISC BIND, Apache httpd, Solaris telnetd), el número de versión, el nombre de host, el tipo de dispositivo (por ejemplo, impresora, enrutador), el sistema operativo. familia (por ejemplo, Windows, Linux). Cuando es posible, Nmap también obtiene la representación de esta información en la enumeración de plataforma común (CPE). A veces, están disponibles diversos detalles, como si un servidor X está abierto a conexiones, la versión del protocolo SSH o el nombre de usuario de KaZaA. Por supuesto, la mayoría de los servicios no proporcionan toda esta información. Si Nmap se compiló con soporte OpenSSL, se conectará a servidores SSL para deducir el servicio que escucha detrás de esa capa de cifrado.Algunos puertos UDP se dejan en el estado `abierto|filtrado` después de que un escaneo de puerto UDP no pueda determinar si el puerto está abierto o filtrado. La detección de versión intentará obtener una respuesta de estos puertos (tal como lo hace con los puertos abiertos) y cambiará el estado a abierto si tiene éxito.Los puertos TCP `abiertos|filtrados` se tratan de la misma manera. Tenga en cuenta que la opción Nmap `-A` habilita la detección de versión, entre otras cosas. La detección de versiones se describe en detalle en el [Capítulo 7, _Detección de versiones de servicios y aplicaciones_](https://nmap.org/book/vscan.html "Capítulo 7. Detección de versiones de servicios y aplicaciones_").

Cuando se descubren servicios RPC, el triturador de RPC de Nmap se utiliza automáticamente para determinar el programa RPC y los números de versión. Toma todos los puertos TCP/UDP detectados como RPC y los inunda con comandos NULL del programa SunRPC en un intento de determinar si son puertos RPC y, de ser así, qué programa y número de versión ofrecen. Por lo tanto, puede obtener efectivamente la misma información que **rpcinfo -p** incluso si el mapeador de puertos del objetivo está detrás de un firewall (o protegido por contenedores TCP). Actualmente, los señuelos no funcionan con el escaneo RPC.

Cuando Nmap recibe respuestas de un servicio pero no puede relacionarlas con su base de datos, imprime una huella digital especial y una URL para que la envíe si sabe con seguridad qué se está ejecutando en el puerto. Tómese un par de minutos para realizar el envío para que su hallazgo pueda beneficiar a todos. Gracias a estos envíos, Nmap tiene alrededor de 6.500 coincidencias de patrones para más de 650 protocolos como SMTP, FTP, HTTP, etc.

La detección de versiones se habilita y controla con las siguientes opciones:

`-sV` (Version detection)

Habilita la detección de versiones, como se analizó anteriormente. Alternativamente, puedes usar `-A`, que permite la detección de versiones, entre otras cosas.

`-sR` es un alias de `-sV`. Antes de marzo de 2011, se utilizaba para activar el triturador RPC por separado de la detección de versión, pero ahora estas opciones siempre se combinan.

`--allports` (Don't exclude any ports from version detection)

De forma predeterminada, la detección de la versión de Nmap omite el puerto TCP 9100 porque algunas impresoras simplemente imprimen cualquier cosa enviada a ese puerto, lo que genera docenas de páginas de solicitudes HTTP GET, solicitudes de sesión SSL binaria, etc. Este comportamiento se puede cambiar modificando o eliminando la opción `Excluir. ` directiva en `nmap-service-probes`, o puede especificar `--allports` para escanear todos los puertos independientemente de cualquier directiva `Exclude`.

``--version-intensity _`<intensity>`_`` (Set version scan intensity)

Al realizar un escaneo de versión (`-sV`), Nmap envía una serie de sondas, a cada una de las cuales se le asigna un valor de rareza entre uno y nueve. Las sondas con números más bajos son efectivas contra una amplia variedad de servicios comunes, mientras que las con números más altos rara vez son útiles. El nivel de intensidad especifica qué sondas se deben aplicar. Cuanto mayor sea el número, más probabilidades habrá de que el servicio sea identificado correctamente. Sin embargo, las exploraciones de alta intensidad tardan más. La intensidad debe estar entre 0 y 9. El valor predeterminado es 7. Cuando se registra una sonda en el puerto de destino a través de la directiva `nmap-service-probes` `ports` , esa sonda se prueba independientemente del nivel de intensidad. Esto garantiza que las sondas DNS siempre se intentarán en cualquier puerto abierto 53, la sonda SSL se realizará en el 443, etc.

`--version-light` (Enable light mode)

Este es un alias conveniente para `--version-intensity 2`. Este modo ligero hace que el escaneo de versiones sea mucho más rápido, pero es un poco menos probable que identifique servicios.

`--version-all` (Try every single probe)

Un alias para `--version-intensity 9`, que garantiza que se intente cada sondeo en cada puerto.

`--version-trace` (Trace version scan activity)

Esto hace que Nmap imprima información de depuración extensa sobre qué versión está realizando el escaneo. Es un subconjunto de lo que obtienes con `--packet-trace`.