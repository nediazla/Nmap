# Descriccion
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