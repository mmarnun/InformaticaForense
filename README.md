# InformaticaForense

La informática forense es el conjunto de técnicas que nos permite obtener la máxima información posible tras un incidente o delito informático.

En esta práctica, realizarás la fase de toma de evidencias y análisis de las mismas sobre una máquina Linux y otra Windows. Supondremos que pillamos al delincuente in fraganti y las máquinas se encontraban encendidas. Opcionalmente, podéis realizar el análisis de un dispositivo Android.

Sobre cada una de las máquinas debes realizar un volcado de memoria y otro de disco duro, tomando las medidas necesarias para certificar posteriormente la cadena de custodia.

Debes tratar de obtener las siguientes informaciones:

## Apartado A) Máquina Windows.

### Volcado de memoria
En la máquina que vamos analizar, le introduciremos un dispositivo USB con el programa `AccessData FTK Imager` con el que vamos a realizar los volcados necesarios para poder analizar esta máquina.

Una vez ejecutado no haremos click sobre `File` y a continuación sobre `Capture Memory...`

![](imagenes/Pasted%20image%2020240207174001.png)


Se nos abrirá una pequeña ventana donde elegiremos una ruta de destino donde queremos volcar la memoria, asimismo indicaremos un nombre para el archivo de destino, y también tenemos la opción de permitir incluir el archivo de paginación de memoria en la imagen, que es una parte del disco usada para almacenar memoria virtual en caso de que la memoria física esté llena.

![](imagenes/Pasted%20image%2020240207174130.png)


Capturando la memoria.

![](imagenes/Pasted%20image%2020240207174151.png)


Y finalmente ha terminado.

![](imagenes/Pasted%20image%2020240207174402.png)


### Volcado de disco duro

Para el volcado del disco igualmente vamos `File` y esta vez sobre `Create Disk Image...`

![](imagenes/Pasted%20image%2020240207174451.png)


Seleccionaremos el origen de la evidencia que vamos a volcar, que va a ser una unidad física, en este caso el disco duro.

![](imagenes/Pasted%20image%2020240207174511.png)

A continuación seleccionamos el disco que vamos a volcar.

![](imagenes/Pasted%20image%2020240207174545.png)


En esta ventana deberemos hacer click sobre `Add...`  para añadir una imagen de destino.

![](imagenes/Pasted%20image%2020240207174653.png)


Seleccionaremos la opción que creará una imagen sin procesar de la unidad seleccionada antes, sin procesar se refiere a una copia completa que incluirá datos incluso eliminados.

![](imagenes/Pasted%20image%2020240207174704.png)


Podemos incluir información adicional a la evidencia.

![](imagenes/Pasted%20image%2020240207174930.png)


Indicaremos la carpeta de destino donde se volcará la imagen del disco, asimismo le colocamos un nombre para identificarlos, en el tamaño del fragmente de imagen especifcamos 0 pues es sin compresión.

![](imagenes/Pasted%20image%2020240207175044.png)


Una vez creada la imagen de destino podemos empezar con el volcado, marcaremos las casillas que verificarán la imagen después de crearla y creará también un directorio donde almacenará una lista de los archivo de imagen creados.

![](imagenes/Pasted%20image%2020240207175102.png)


Mientras se realiza el volcado nos aparecerán varias ventanas de carga.

![](imagenes/Pasted%20image%2020240207181038.png)

![](imagenes/Pasted%20image%2020240207181403.png)

![](imagenes/Pasted%20image%2020240207182648.png)


Hasta que finalmente se ha volcado.

![](imagenes/Pasted%20image%2020240207182708.png)

### Volcado de registros

De igual manera que los anteriores pero con la opción `Obtain Protected Files...`.
![](imagenes/Pasted%20image%2020240207200008.png)


Indicaremos la carpeta de destino para el volcado, y marcaremos la opción que recupera todos los archivos del sistema que podrían ser útiles para la investigación de los archivos de registro.

![](imagenes/Pasted%20image%2020240207200045.png)


![](imagenes/Pasted%20image%2020240207200101.png)


### Autopsy

En la máquina desde la que vamos a realizar el análisis ejecutaremos el programa y abriremos un caso nuevo.

![](imagenes/Pasted%20image%2020240215202452.png)


Le daremos nombre al caso e indicaremos un directorio base donde se crearán los archivos necesarios.

![](imagenes/Pasted%20image%2020240215202756.png)


A continuación nos aparece una nueva ventana donde vamos a indicarle que se generará un nombre para el origen de la evidencia.

![](imagenes/Pasted%20image%2020240215203034.png)


Especificamos que es una imagen de disco.

![](imagenes/Pasted%20image%2020240215203059.png)


Le indicamos la ruta en donde se encuentra el dump del disco.

![](imagenes/Pasted%20image%2020240215203122.png)


Aquí podemos modificar los módulos que queremos que se analizen.

![](imagenes/Pasted%20image%2020240215203146.png)


Al acabar ya comenzará añadir los datos al caso.

![](imagenes/Pasted%20image%2020240215203317.png)

Y finalmente inicia el análisis del disco, a la izquierda irán apareciendo todos los `Data Artifacts`.

![](imagenes/Pasted%20image%2020240215203606.png)

### Volatility3

Para instalar el programa deberemos de clonar el repositorio oficial.

```bash
git clone https://github.com/volatilityfoundation/volatility3.git
```

Crearemos una entorno con python y lo activaremos.

```bash
python3 -m venv vol
source vol/bin/activate
```

Dentro de la carpeta del repositorio clonado instalaremos los `requirements` y "contruiremos" y finalmente instalaremos Volatility3.

```bash
pip3 install -r requirements.txt
python3 setup.py build
python3 setup.py install
```


#### 1. Procesos en ejecución.

Con `PsList` enumeramos los procesos del sistema en el momento que se tomó el volcado de la memoria.
Cada fila representa un proceso en el sistema, mostrando su ID, la ID del proceso padre, nombre de archivo, sesión, etc.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.pslist.PsList
```

![](imagenes/Pasted%20image%2020240216211118.png)
#### 2. Servicios en ejecución.

Mostraremos una lista de servicios en el sistema, el indicador y el nombre del servicio.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.getservicesids.GetServiceSIDs
```

![](imagenes/Pasted%20image%2020240216211328.png)
#### 3. Puertos abiertos.

Lista de conexiones de red activas por lo tanto también muestra los puertos que utilizan, direcciones IP local y remota, PID, y fechas y hora.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.netstat.NetStat
```

![](imagenes/Pasted%20image%2020240216211500.png)
#### 4. Conexiones establecidas por la máquina.

Lista de conexiones establecidas por la máquina, muestra, protocolos, direcciones IP, puertos, el estado de la conexión, propietario, ID, etc.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.netscan.NetScan
```

![](imagenes/Pasted%20image%2020240216211637.png)
#### 5. Sesiones de usuario establecidas remotamente.

Muestra las sesiones activas en el sistema en el momento del volcado, muestra la ID de la sesión, tipo de la sesión, ID del proceso, nombre del proceso, nombre de usuario y fecha y hora de la sesión.

Podemos ver que muestra una sesión de ssh con el usuario alex.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.sessions.Sessions
```

![](imagenes/Pasted%20image%2020240216211839.png)
#### 6. Ficheros transferidos recientemente por NetBios.

Para ver los ficheros tranferidos por NetBios, no he encontrado opciones ni en Autopsy ni en Volatility.

Entonces en la misma máquina ejecutaremos el comando `nbtstat -n` que mostrará la tabla de NetBIOS local en los adaptadores de la máquina.

![](imagenes/Pasted%20image%2020240217151857.png)
#### 7. Contenido de la caché DNS.

El comando `ipconfig /displaydns` mostrará los nombres de dominio y hosts resueltos recientemente y las direcciones IP correspondientes.

![](imagenes/Pasted%20image%2020240217151121.png)

#### 8. Variables de entorno.

Mostraremos las variables de entorno, junto a su ID el nombre del proceso, el bloque en la memoria, la variable y el valor.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.envars.Envars
```

![](imagenes/Pasted%20image%2020240216213402.png)
#### 9. Dispositivos USB conectados

Desde autopsy con `USB Device Attached` podemos ver los dispisitvos extraibles USB que se han introducido en la máquina.

![](imagenes/Pasted%20image%2020240216191124.png)

#### 10. Redes wifi utilizadas recientemente.

Con la herramienta Registry Viewer en el archivo software y en la ruta `software\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles` , veremos que la clave del registro que se muestra en la imagen contiene información sobre las redes Wi-Fi a las que se ha conectado el equipo. El valor `ProfileName` indica el nombre de la red Wi-Fi. El tipo de dato `REG_SZ` indica que el valor es una cadena de caracteres. Los datos `Red` indican que el perfil es para una red Wi-Fi.
aa

![](imagenes/Pasted%20image%2020240217152605.png)

#### 11. Configuración del firewall de nodo.

Mostraremos la configuración de las reglas del firewall, por ejemplo la destacada:
- **Valor:** v2.30jAction Allow Active=TRUE(Dir=Out Protocol=6 Profile=Domain Profile=Private App=%SystemRoot%\system32\svchost.exe/Svc=CDPSvc)
El valor indica que la regla del firewall está configurada para permitir el tráfico saliente en el protocolo TCP a través del puerto 7680 para el servicio CDPSvc.
Los demás valores de la tabla muestran información similar sobre otras reglas del firewall.

Lo encontraremos abriendo la herramienta con el archivo de system en la ruta `system\ControlSet001\Services\SharedAccess\Defaults\FirewallPolicy\FirewallRules`

![](imagenes/Pasted%20image%2020240217150706.png)

#### 12. Programas que se ejecutan en el Inicio.

De nuevo abrimos el archivo software en la ruta `software\Microsoft\Windows\CurrentVersion\Run` y veremos los programas que se ejecutan al inicio, en este caso solo se ejecuta un antivirus que tiene integrado el sistema.

![](imagenes/Pasted%20image%2020240217150824.png)

#### 13. Asociación de extensiones de ficheros y aplicaciones.

En el desplegable `File Types`, veremos más donde encontraremos las diferentes extensiones.

![](imagenes/Pasted%20image%2020240216191213.png)

Mostramos los archivos con extensión de imágenes.

![](imagenes/Pasted%20image%2020240216191343.png)

Con extensión para PDFs.

![](imagenes/Pasted%20image%2020240216191529.png)

De HTML.

![](imagenes/Pasted%20image%2020240216191634.png)

Y de ejecutables con .exe

![](imagenes/Pasted%20image%2020240216191749.png)


#### 14. Aplicaciones usadas recientemente.

Para ver las aplicaciones usadas, podemos dirigirnos al artefacto `Run Programs`.

![](imagenes/Pasted%20image%2020240216191904.png)

O también al mismo archivo dump del disco en la pestaña de `Summary` y en `User Activity` ahí también podremos ver los programas recientes.

![](imagenes/Pasted%20image%2020240216192052.png)

#### 15. Ficheros abiertos recientemente.

Podemos verlos en `Recent Documents`.

![](imagenes/Pasted%20image%2020240216192247.png)

O asimismo en la pestaña de `Recent Files` desde `Summary`.

![](imagenes/Pasted%20image%2020240216193038.png)

#### 16. Software Instalado.

Podemos ver el software instalado en el artefacto `Installed Programs`.

![](imagenes/Pasted%20image%2020240216193131.png)


#### 17. Contraseñas guardadas.

Las contraseñas guardadas las podemos ver en `Web Account Type` podremos ver el usuario y la contraseña, normalmente no aparecen porque son mas complejas o están bien encriptadas pero yo usé contraseñas simples y si aparecen.

![](imagenes/Pasted%20image%2020240216214047.png)

#### 18. Cuentas de Usuario

Para ver las cuentas en `OS Accounts`

![](imagenes/Pasted%20image%2020240216193645.png)


#### 19. Historial de navegación y descargas. Cookies.

Para ver el historial de navegación iremos a `Web History`

![](imagenes/Pasted%20image%2020240216193755.png)

Para ver las búsquedas en `Web Search`

![](imagenes/Pasted%20image%2020240216193828.png)

Las descargas en `Web Downloads`

![](imagenes/Pasted%20image%2020240216193925.png)

Y las cookies en `Web Cookies`

![](imagenes/Pasted%20image%2020240216194022.png)

#### 20. Volúmenes cifrados

Podremos ver si está cifrado dirigiéndonos directamente al archivo en cuestión y mirando en la pestaña de `File Metadas`, veremos donde en Flags indicará Encrypted.

![](imagenes/Pasted%20image%2020240216194851.png)

#### 21. Archivos con extensión cambiada.

Para ver los archivos que tienen una extensión cambiada o no adecuada, vamos a `Extension Mismatch Detected` veremos varios archivos.

![](imagenes/Pasted%20image%2020240216194054.png)

Uno de ellos es un .tmp cuando realmente es una imagen.

![](imagenes/Pasted%20image%2020240216194219.png)

#### 22. Archivos eliminados.

Para los archivos eliminados podemos ver un artefacto llamado `Recycle Bin` que vendría siendo la papelera.

![](imagenes/Pasted%20image%2020240216194339.png)

O en `Deleted Files` podremos ver archivos eliminados completamente.

![](imagenes/Pasted%20image%2020240216194546.png)

#### 23. Archivos Ocultos.

Para los archivos ocultos es igual que en los cifrados, deberemos de fijarnos en las Flags, es ahí donde indicará si es `Hidden`.

![](imagenes/Pasted%20image%2020240216214223.png)

#### 24. Archivos que contienen una cadena determinada.

Para encontrar archivos que tienen una cadena deberemos de buscar con `Keyword Search` y introducimos la cadena a buscar.
Podemos ver que ha encontrado un archivo donde existe la cadena.

![](imagenes/Pasted%20image%2020240217152834.png)

#### 25. Búsqueda de imágenes por ubicación.

Para encontrar la ubicación de las imágenes, deberemos ir a `Geolocation`, se abrirá una ventana nueva como la de la imagen con un mapa donde se verán marcadores en ubicaciones y al hacer click podemos ver la imagen, la fecha y hora capturada, coordenadas modelo del dispositivo y creador. Vemos que la imagen se hizo en Mallorca.

![](imagenes/Pasted%20image%2020240216201149.png)

#### 26. Búsqueda de archivos por autor.

Para los archivos por autor vamos al artefacto `Metadata` y podemos ver que aparecen diferentes archivos y podemos ver el autor en el apartado de `Owner`.

![](imagenes/Pasted%20image%2020240216201325.png)

## Apartado B) Máquina Linux.

Intenta realizar las mismas operaciones en una máquina Linux para aquellos apartados que tengan sentido y no se realicen de manera idéntica a Windows.

### Volcado de disco duro

Para el volcado del disco duro usaremos el programa `dd` indicaremos como "input file" el dispositivo de bloques de la primera partición y como "output file" indicaremos el archivo de salida con extensión .001 para que autopsy lo pueda leer, y con "bs" especificaremos que el tamaño del bloque de datos es de 64 kilobytes. Esto significa que `dd` leerá 64 KB de datos de la partición `/dev/vda1` y los escribirá en el archivo `/mnt/forense/diskdump.001` en cada operación de lectura/escritura.

```bash
dd if=/dev/vda1 of=/mnt/forense/diskdump.001 bs=64K
```

![](imagenes/Pasted%20image%2020240217212655.png)

### Volcado de memoria

Para el volcado de memoria usaremos LiME, en primer lugar clonaremos el repositorio oficial.

```bash
git clone https://github.com/504ensicsLabs/LiME.git
```

Instalamos el paquete `lime-forensics-dkms` que proporciona los controladores de LiME para el kernel de Linux, permitiendo que LiME se compile automáticamente para el kernel.

```bash
sudo apt install lime-forensics-dkms
```

Dentro de la ruta `LiME/src` ejecutaremos el comando `make` para compilar y construir los módulos.

```bash
debian@alex-forense:~/Documentos/LiME/src$ make
make -C /lib/modules/6.1.0-17-amd64/build M="/home/debian/Documentos/LiME/src" modules
make[1]: se entra en el directorio '/usr/src/linux-headers-6.1.0-17-amd64'
  CC [M]  /home/debian/Documentos/LiME/src/tcp.o
  CC [M]  /home/debian/Documentos/LiME/src/disk.o
  CC [M]  /home/debian/Documentos/LiME/src/main.o
  CC [M]  /home/debian/Documentos/LiME/src/hash.o
  CC [M]  /home/debian/Documentos/LiME/src/deflate.o
  LD [M]  /home/debian/Documentos/LiME/src/lime.o
  MODPOST /home/debian/Documentos/LiME/src/Module.symvers
  CC [M]  /home/debian/Documentos/LiME/src/lime.mod.o
  LD [M]  /home/debian/Documentos/LiME/src/lime.ko
  BTF [M] /home/debian/Documentos/LiME/src/lime.ko
Skipping BTF generation for /home/debian/Documentos/LiME/src/lime.ko due to unavailability of vmlinux
make[1]: se sale del directorio '/usr/src/linux-headers-6.1.0-17-amd64'
strip --strip-unneeded lime.ko
mv lime.ko lime-6.1.0-17-amd64.ko
```

Cargamos el módulo del kernel de LiME `lime-6.1.0-17-amd64.ko` en el kernel de Linux. El módulo se carga utilizando `insmod`, que es un programa para insertar módulos en el kernel, además, pasa parámetros al módulo, como el camino al archivo de volcado de memoria `path=/mnt/linux/memoria/memdump.mem` y el formato de salida `format=lime`. Esto indica a LiME que debe crear un volcado de memoria en formato LiME y guardarlo en el archivo especificado.

```bash
root@alex-forense:/home/debian/Documentos/LiME/src# insmod lime-6.1.0-17-amd64.ko "path=/mnt/linux/memoria/memdump.mem format=lime"
```

#### 1. Procesos en ejecución.

Listaremos los procesos en ejecución de manera detallada, cada fila es un proceso, vemos el usuario que lo inició, el PID, el porcentaje de CPU, de memoria, comando, etc.

```bash
ps auxww
```

![](imagenes/Pasted%20image%2020240218181424.png)

![](imagenes/Pasted%20image%2020240218181440.png)

#### 2. Servicios en ejecución.

Lista de servicios en el sistema que muestra sus estado y una descripción.

```bash 
systemctl list-units --type=service
```

![](imagenes/Pasted%20image%2020240218181518.png)

#### 3. Puertos abiertos.

Lista de donde podemos ver los puertos asociados a sockets de red, donde vemos direcciones IP, procesos asociados...

```bash
ss -tuln
```

![](imagenes/Pasted%20image%2020240218181559.png)

#### 4. Conexiones establecidas por la máquina.

Lista de conexiones, detallando el estado, cantidad de datos recibidos, dirección IP, proceso...

```bash
ss -ant
```

![](imagenes/Pasted%20image%2020240218181644.png)

#### 5. Sesiones de usuario establecidas remotamente.

Muestra información sobre las sesiones de usuario activas en el sistema, ofrece la carga promedio del sistema y el tiempo de actividad...

```bash
w
```

![](imagenes/Pasted%20image%2020240218181738.png)

#### 6. Ficheros transferidos recientemente por NetBios.

Mostrará una lista de las carpetas compartidas en el servidor Samba local, junto con otra información relevante como el nombre de trabajo, el nombre del servidor...

```bash
smbclient -L localhost
```

![](imagenes/Pasted%20image%2020240218181955.png)

#### 7. Contenido de la caché DNS.

Con el siguiente comando extraemos y listaremos todas las cadenas de caracteres que se pueden leer del archivo binario de la ruta indicada.

```bash
strings /var/cache/nscd/hosts
```

![](imagenes/Pasted%20image%2020240218182411.png)

![](imagenes/Pasted%20image%2020240218182453.png)

#### 8. Variables de entorno.

Muestra las variables de entorno actuales del usuario.

```bash
env
```

![](imagenes/Pasted%20image%2020240218182536.png)

#### 9. Dispositivos USB conectados

Lista de todos los dispositivos USB conectados al sistema, incluyendo información sobre el fabricante, el modelo y el ID del dispositivo.

```bash
lsusb
```

![](imagenes/Pasted%20image%2020240218182621.png)

#### 10. Redes wifi utilizadas recientemente.

Información sobre las conexiones de red activas en el sistema, muestra el nombre la UUID, el tipo y el dispositivo.

```bash
nmcli c show --active

nmclo c show
```

![](imagenes/Pasted%20image%2020240218182744.png)
![](imagenes/Pasted%20image%2020240218182830.png)
#### 11. Configuración del firewall de nodo.

![](imagenes/Pasted%20image%2020240218183009.png)

#### 12. Programas que se ejecutan en el Inicio.

Muestra los servicios habilitados en el sistema, nombre de la unidad, estado y si el servicio está habilitado o no.

```bash
systemctl list-unit-files --type=service | grep enabled
```

![](imagenes/Pasted%20image%2020240218211933.png)

#### 13. Asociación de extensiones de ficheros y aplicaciones.

![](imagenes/Pasted%20image%2020240218124202.png)

![](imagenes/Pasted%20image%2020240218203151.png)

![](imagenes/Pasted%20image%2020240218124425.png)

![](imagenes/Pasted%20image%2020240218124532.png)


![](imagenes/Pasted%20image%2020240218124624.png)

#### 14. Aplicaciones usadas recientemente.

#### 15. Ficheros abiertos recientemente.

#### 16. Software Instalado.

![](imagenes/Pasted%20image%2020240218192650.png)

#### 17. Contraseñas guardadas.

![](imagenes/Pasted%20image%2020240218193451.png)

![](imagenes/Pasted%20image%2020240218193746.png)



#### 18. Cuentas de Usuario

![](imagenes/Pasted%20image%2020240218193541.png)

#### 19. Historial de navegación y descargas. Cookies.

![](imagenes/Pasted%20image%2020240218124803.png)

![](imagenes/Pasted%20image%2020240218124823.png)

![](imagenes/Pasted%20image%2020240218202958.png)

![](imagenes/Pasted%20image%2020240218124912.png)

#### 20. Volúmenes cifrados

![](imagenes/Pasted%20image%2020240218194753.png)
#### 21. Archivos con extensión cambiada.

![](imagenes/Pasted%20image%2020240218125249.png)

#### 22. Archivos eliminados.

#### 23. Archivos Ocultos.

![](imagenes/Pasted%20image%2020240218125916.png)

#### 24. Archivos que contienen una cadena determinada.

![](imagenes/Pasted%20image%2020240218130407.png)

#### 25. Búsqueda de imágenes por ubicación.

![](imagenes/Pasted%20image%2020240218130517.png)

#### 26. Búsqueda de archivos por autor.

![](imagenes/Pasted%20image%2020240218130550.png)