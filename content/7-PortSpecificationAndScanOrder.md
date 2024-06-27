# Especificación de puerto y orden de escaneo

Además de todos los métodos de escaneo discutidos anteriormente, Nmap ofrece opciones para especificar qué puertos se escanean y si el orden de escaneo es aleatorio o secuencial. De forma predeterminada, Nmap escanea los 1000 puertos más comunes para cada protocolo.

``-p _`<port ranges>`_`` (Only scan specified ports)

Esta opción especifica qué puertos desea escanear y anula el valor predeterminado. Los números de puerto individuales están bien, al igual que los rangos separados por un guión (por ejemplo, `1-1023`). Los valores inicial y/o final de un rango pueden omitirse, lo que hace que Nmap utilice 1 y 65535, respectivamente. Por lo tanto, puede especificar `-p-` para escanear los puertos del 1 al 65535. Se permite escanear el puerto cero si lo especifica explícitamente. Para el escaneo de protocolo IP (`-sO`), esta opción especifica los números de protocolo que desea escanear (0–255).

Al escanear una combinación de protocolos (por ejemplo, TCP y UDP), puede especificar un protocolo en particular anteponiendo los números de puerto con `T:` para TCP, `U:` para UDP, `S:` para SCTP o `P: ` para protocolo IP. El calificador dura hasta que especifica otro calificador. Por ejemplo, el argumento `-p U:53,111,137,T:21-25,80,139,8080` escanearía los puertos UDP 53, 111 y 137, así como los puertos TCP enumerados. Tenga en cuenta que para escanear tanto UDP como TCP, debe especificar `-sU` y al menos un tipo de escaneo TCP (como `-sS`, `-sF` o `-sT`). Si no se proporciona ningún calificador de protocolo, los números de puerto se agregan a todas las listas de protocolos.

Los puertos también se pueden especificar por nombre de acuerdo con el nombre al que se hace referencia en los ``nmap-services`. Incluso puedes utilizar los comodines `*` y `?` con los nombres. Por ejemplo, para escanear FTP y todos los puertos cuyos nombres comiencen con “http”, utilice `-p ftp,http*`. Tenga cuidado con las expansiones del shell y cite el argumento `-p` si no está seguro.

Los rangos de puertos pueden estar entre corchetes para indicar los puertos dentro de ese rango que aparecen en `nmap-services`. Por ejemplo, lo siguiente escaneará todos los puertos en `nmap-services` iguales o inferiores a 1024: `-p [-1024]`. Tenga cuidado con las expansiones del shell y cite el argumento `-p` si no está seguro.

``--exclude-ports _`<port ranges>`_`` (Exclude the specified ports from scanning)

Esta opción especifica qué puertos desea que Nmap excluya del análisis. Los _`<rangos de puertos>`_ se especifican de manera similar a `-p`. Para el escaneo de protocolo IP (`-sO`), esta opción especifica los números de protocolo que desea excluir (0–255).

Cuando se solicita la exclusión de puertos, se excluyen de todo tipo de análisis (es decir, no se analizarán bajo ninguna circunstancia). Esto también incluye la fase de descubrimiento.

`-F` (Fast (limited port) scan)

Especifica que desea escanear menos puertos que los predeterminados. Normalmente, Nmap escanea los 1000 puertos más comunes para cada protocolo escaneado. Con `-F`, esto se reduce a 100.

Nmap necesita un archivo `nmap-services` con información de frecuencia para saber qué puertos son los más comunes (consulte [la sección llamada “Lista de puertos conocidos: `nmap-services`”](https://nmap.org/ book/nmap-services.html "Lista de puertos conocidos: nmap-services") para obtener más información sobre las frecuencias de los puertos). Si la información de frecuencia de puertos no está disponible, tal vez debido al uso de un archivo personalizado `nmap-services` , Nmap escanea todos los puertos nombrados más los puertos 1-1024. En ese caso, `-F` significa escanear solo los puertos nombrados en el archivo de servicios.

`-r` (Don't randomize ports)

De forma predeterminada, Nmap aleatoriza el orden de los puertos escaneados (excepto que ciertos puertos comúnmente accesibles se mueven cerca del principio por razones de eficiencia). Esta aleatorización normalmente es deseable, pero en su lugar puede especificar `-r` para el escaneo de puertos secuencial (ordenado de menor a mayor).

``--port-ratio _`<ratio>`_<decimal number between 0 and 1>``

Analiza todos los puertos en el archivo `nmap-services` con una proporción mayor que la proporcionada._`<ratio>`_ debe estar entre 0,0 y 1,0.

``--top-ports _`<n>`_``

Analiza los _`<n>`_ puertos con la proporción más alta que se encuentran en el archivo `nmap-services` después de excluir todos los puertos especificados por `--exclude-ports`._`<n>`_ debe ser 1 o mayor.