# Conceptos básicos de escaneo de puertos
Si bien Nmap ha crecido en funcionalidad a lo largo de los años, comenzó como un escáner de puertos eficiente y esa sigue siendo su función principal. El comando simple nmap <destino> escanea 1000 puertos TCP en el host <destino>. Si bien muchos escáneres de puertos tradicionalmente han agrupado todos los puertos en estados abiertos o cerrados, Nmap es mucho más granular. Divide los puertos en seis estados: abierto, cerrado, filtrado, sin filtrar, abierto|filtrado o cerrado|filtrado.

Estos estados no son propiedades intrínsecas del puerto en sí, sino que describen cómo los ve Nmap. Por ejemplo, un escaneo de Nmap desde la misma red que el objetivo puede mostrar el puerto 135/tcp como abierto, mientras que un escaneo al mismo tiempo con las mismas opciones desde Internet puede mostrar ese puerto como filtrado.

Los seis estados portuarios reconocidos por Nmap
#### abierto
Una aplicación acepta activamente conexiones TCP, datagramas UDP o asociaciones SCTP en este puerto. Encontrarlos suele ser el objetivo principal del escaneo de puertos. Las personas preocupadas por la seguridad saben que cada puerto abierto es una vía de ataque. Los atacantes y los evaluadores quieren explotar los puertos abiertos, mientras que los administradores intentan cerrarlos o protegerlos con firewalls sin frustrar a los usuarios legítimos. Los puertos abiertos también son interesantes para análisis no relacionados con la seguridad porque muestran los servicios disponibles para su uso en la red.

#### cerrado
Se puede acceder a un puerto cerrado (recibe y responde a paquetes de sonda Nmap), pero no hay ninguna aplicación escuchando en él. Pueden ser útiles para mostrar que un host tiene una dirección IP activa (descubrimiento de host o escaneo de ping) y como parte de la detección del sistema operativo. Debido a que se puede acceder a los puertos cerrados, puede que valga la pena escanearlos más tarde en caso de que alguno se abra. Es posible que los administradores quieran considerar bloquear dichos puertos con un firewall. Luego aparecerían en el estado filtrado, que se analiza a continuación.

#### filtrado
Nmap no puede determinar si el puerto está abierto porque el filtrado de paquetes impide que sus sondas lleguen al puerto. El filtrado podría realizarse desde un dispositivo de firewall dedicado, reglas de enrutador o software de firewall basado en host. Estos puertos frustran a los atacantes porque proporcionan muy poca información. A veces responden con mensajes de error ICMP como el código 13 de tipo 3 (destino inalcanzable: comunicación prohibida administrativamente), pero los filtros que simplemente descartan sondas sin responder son mucho más comunes. Esto obliga a Nmap a reintentar varias veces en caso de que la sonda se abandonara debido a la congestión de la red en lugar de al filtrado. Esto ralentiza drásticamente el escaneo.

#### sin filtrar
El estado sin filtrar significa que se puede acceder a un puerto, pero Nmap no puede determinar si está abierto o cerrado. Sólo el análisis ACK, que se utiliza para asignar conjuntos de reglas de firewall, clasifica los puertos en este estado. Escanear puertos sin filtrar con otros tipos de escaneo, como escaneo de Windows, escaneo SYN o escaneo FIN, puede ayudar a determinar si el puerto está abierto.

#### abierto|filtrado
Nmap coloca los puertos en este estado cuando no puede determinar si un puerto está abierto o filtrado. Esto ocurre para tipos de escaneo en los que los puertos abiertos no dan respuesta. La falta de respuesta también podría significar que un filtro de paquetes abandonó la sonda o cualquier respuesta que provocara. Por lo tanto, Nmap no sabe con certeza si el puerto está abierto o está siendo filtrado. Los escaneos UDP, protocolo IP, FIN, NULL y Xmas clasifican los puertos de esta manera.

#### cerrado|filtrado
Este estado se utiliza cuando Nmap no puede determinar si un puerto está cerrado o filtrado. Sólo se utiliza para el análisis inactivo de ID de IP.