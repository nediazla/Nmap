Nmap (Network Mapper) es una herramienta de código abierto utilizada para descubrir hosts y servicios en una red informática mediante el envío de paquetes y el análisis de sus respuestas. Es muy útil para realizar auditorías de seguridad y descubrir posibles vulnerabilidades en los sistemas. Sin embargo, los firewalls y sistemas de detección de intrusiones (IDS) pueden bloquear o detectar escaneos realizados con Nmap. En este post, exploraremos diversas técnicas para evadir firewalls utilizando Nmap y proporcionaremos ejemplos de cómo aplicarlas en la práctica.

### ¿Por qué Evasión de Firewalls?

Los firewalls están diseñados para filtrar tráfico no deseado y bloquear intentos de acceso no autorizados a una red. Los administradores de red configuran reglas para permitir o denegar tráfico basado en varios parámetros como la dirección IP de origen, el puerto de destino, el tipo de protocolo, entre otros. La evasión de firewalls es crucial para los pentesters y auditores de seguridad para evaluar la efectividad de las reglas del firewall y descubrir vulnerabilidades que podrían ser explotadas por atacantes.

### Técnicas de Evasión de Firewalls con Nmap

#### 1. Fragmentación de Paquetes

La fragmentación de paquetes es una técnica que divide un paquete grande en varios paquetes más pequeños. Algunos firewalls no manejan correctamente los paquetes fragmentados, lo que permite que los escaneos pasen desapercibidos.

**Comando:**

```bash
nmap -f ejemplo.com
```

El parámetro `-f` fragmenta los paquetes enviados por Nmap. Esto puede ayudar a evadir ciertos firewalls que no pueden reconstruir o analizar correctamente los paquetes fragmentados.

#### 2. Modificación del Tamaño del Paquete

Al ajustar el tamaño del paquete, puedes hacer que los escaneos sean menos detectables por los firewalls.

**Comando:**

```bash
nmap --mtu 16 ejemplo.com
```

El parámetro `-f` fragmenta los paquetes enviados por Nmap. Esto puede ayudar a evadir ciertos firewalls que no pueden reconstruir o analizar correctamente los paquetes fragmentados.

#### 2. Modificación del Tamaño del Paquete

Al ajustar el tamaño del paquete, puedes hacer que los escaneos sean menos detectables por los firewalls.

**Comando:**

```bash
nmap --mtu 16 ejemplo.com
```

El parámetro `--mtu` ajusta el tamaño máximo de unidad de transmisión (MTU). Cambiar este valor puede confundir algunos firewalls y IDS.

#### 3. Uso de Pausas (Timing)

Ajustar el tiempo entre cada paquete enviado puede ayudar a evadir los mecanismos de detección de intrusos que monitorean la velocidad de los escaneos.

**Comando:**

```bash
nmap -T2 ejemplo.com
```

El parámetro `-T` ajusta la velocidad del escaneo. Los valores van de `T0` (muy lento) a `T5` (muy rápido). Un escaneo más lento (`T2`) puede ser menos detectable.

#### 4. Escaneos TCP Null, FIN, y Xmas

Estos tipos de escaneos manipulan los flags TCP para evadir firewalls que inspeccionan conexiones TCP estándar.

**Comandos:**

```bash
nmap -sN ejemplo.com # Null scan

nmap -sF ejemplo.com # FIN scan

nmap -sX ejemplo.com # Xmas scan
```

- `-sN`: Envía paquetes sin flags (Null scan).
- `-sF`: Envía paquetes con el flag FIN.
- `-sX`: Envía paquetes con los flags FIN, PSH, y URG (Xmas scan).

Estos escaneos pueden evadir firewalls que no filtran correctamente paquetes TCP no estándar.

#### 5. Uso de Direcciones IP Falsas

Utilizar direcciones IP falsas como direcciones de origen puede confundir los firewalls que dependen de la dirección IP para las reglas de filtrado.

**Comando:**

```bash
nmap –D RND:10 ejemplo.com
```

El parámetro `-D` permite usar direcciones IP señuelo. `RND:10` genera 10 direcciones IP señuelo aleatorias, haciendo más difícil para los firewalls y IDS identificar la verdadera dirección IP del escáner.

#### 6. Evitar Ping

Algunos firewalls bloquean los pings (ICMP Echo Requests). Usar el parámetro `-Pn` evita el envío de pings y realiza el escaneo directamente.

**Comando:**

nmap -Pn ejemplo.com

El parámetro `-Pn` le dice a Nmap que no realice pings y asuma que los hosts están activos. Esto puede ayudar a evadir firewalls que bloquean pings.

#### 7. Escaneo con Spoofing de Mac

Cambiar la dirección MAC del escáner puede ayudar a evadir sistemas de seguridad que filtran tráfico basado en direcciones MAC.

**Comando:**

```bash
nmap --spoof-mac 00:11:22:33:44:55 ejemplo.com
```

El parámetro `--spoof-mac` permite especificar una dirección MAC falsa, haciendo más difícil rastrear el origen del escaneo.

### Ejemplo Práctico

Imagina que quieres escanear el dominio `ejemplo.com` y sabes que su firewall es bastante estricto. Podrías combinar varias técnicas de evasión para mejorar tus posibilidades de éxito.

**Comando Combinado:**

```bash
nmap -f --mtu 32 -T2 -sN -D RND:5 -Pn --spoof-mac 00:11:22:33:44:55 ejemplo.com
```

Este comando fragmenta los paquetes, ajusta el MTU a 32 bytes, usa una velocidad de escaneo lenta (`-T2`), realiza un Null scan (`-sN`), utiliza 5 direcciones IP señuelo aleatorias (`-D RND:5`), evita el ping (`-Pn`), y spoofea la dirección MAC (`--spoof-mac`).

Evadir firewalls con Nmap requiere una combinación de técnicas y un buen entendimiento de cómo funcionan los mecanismos de filtrado y detección. Las técnicas discutidas en este post ofrecen una variedad de opciones para superar barreras de seguridad y realizar auditorías de red más efectivas. Recuerda siempre realizar estas actividades de forma legal y ética, con el permiso adecuado del propietario de la red