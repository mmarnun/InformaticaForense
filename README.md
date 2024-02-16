# InformaticaForense

La informática forense es el conjunto de técnicas que nos permite obtener la máxima información posible tras un incidente o delito informático.

En esta práctica, realizarás la fase de toma de evidencias y análisis de las mismas sobre una máquina Linux y otra Windows. Supondremos que pillamos al delincuente in fraganti y las máquinas se encontraban encendidas. Opcionalmente, podéis realizar el análisis de un dispositivo Android.

Sobre cada una de las máquinas debes realizar un volcado de memoria y otro de disco duro, tomando las medidas necesarias para certificar posteriormente la cadena de custodia.

Debes tratar de obtener las siguientes informaciones:

## Apartado A) Máquina Windows.

### Volcado de memoria
![](imagenes/Pasted%20image%2020240207174001.png)

![](imagenes/Pasted%20image%2020240207174130.png)

![](imagenes/Pasted%20image%2020240207174151.png)

![](imagenes/Pasted%20image%2020240207174402.png)


### Volcado de disco duro

![](imagenes/Pasted%20image%2020240207174451.png)

![](imagenes/Pasted%20image%2020240207174511.png)


![](imagenes/Pasted%20image%2020240207174545.png)

![](imagenes/Pasted%20image%2020240207174653.png)

![](imagenes/Pasted%20image%2020240207174704.png)

![](imagenes/Pasted%20image%2020240207174930.png)

![](imagenes/Pasted%20image%2020240207175044.png)


![](imagenes/Pasted%20image%2020240207175102.png)

![](imagenes/Pasted%20image%2020240207181038.png)

![](imagenes/Pasted%20image%2020240207181403.png)

![](imagenes/Pasted%20image%2020240207182648.png)

![](imagenes/Pasted%20image%2020240207182708.png)

### Volcado de registros
![](imagenes/Pasted%20image%2020240207200008.png)

![](imagenes/Pasted%20image%2020240207200045.png)


![](imagenes/Pasted%20image%2020240207200101.png)


### Autopsy

![](imagenes/Pasted%20image%2020240215202452.png)

![](imagenes/Pasted%20image%2020240215202756.png)

![](imagenes/Pasted%20image%2020240215203034.png)

![](imagenes/Pasted%20image%2020240215203059.png)


![](imagenes/Pasted%20image%2020240215203122.png)


![](imagenes/Pasted%20image%2020240215203146.png)


![](imagenes/Pasted%20image%2020240215203317.png)


![](imagenes/Pasted%20image%2020240215203606.png)






#### 1. Procesos en ejecución.
```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.pslist.PsList
```

![](imagenes/Pasted%20image%2020240216211118.png)
#### 2. Servicios en ejecución.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.getservicesids.GetServiceSIDs
```

![](imagenes/Pasted%20image%2020240216211328.png)
#### 3. Puertos abiertos.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.netstat.NetStat
```

![](imagenes/Pasted%20image%2020240216211500.png)
#### 4. Conexiones establecidas por la máquina.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.netscan.NetScan
```

![](imagenes/Pasted%20image%2020240216211637.png)
#### 5. Sesiones de usuario establecidas remotamente.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.sessions.Sessions
```

![](imagenes/Pasted%20image%2020240216211839.png)
#### 6. Ficheros transferidos recientemente por NetBios.

#### 7. Contenido de la caché DNS.

#### 8. Variables de entorno.

```bash
python vol.py -f "/mnt/forense/windows/memoria/memdump.mem" windows.envars.Envars
```

![](imagenes/Pasted%20image%2020240216213402.png)
#### 9. Dispositivos USB conectados

![](imagenes/Pasted%20image%2020240216191124.png)

#### 10. Redes wifi utilizadas recientemente.

#### 11. Configuración del firewall de nodo.

#### 12. Programas que se ejecutan en el Inicio.

#### 13. Asociación de extensiones de ficheros y aplicaciones.

![](imagenes/Pasted%20image%2020240216191213.png)

![](imagenes/Pasted%20image%2020240216191343.png)


![](imagenes/Pasted%20image%2020240216191529.png)

![](imagenes/Pasted%20image%2020240216191634.png)


![](imagenes/Pasted%20image%2020240216191749.png)



#### 14. Aplicaciones usadas recientemente.

![](imagenes/Pasted%20image%2020240216191904.png)

![](imagenes/Pasted%20image%2020240216192052.png)

#### 15. Ficheros abiertos recientemente.

![](imagenes/Pasted%20image%2020240216192247.png)

![](imagenes/Pasted%20image%2020240216193038.png)

#### 16. Software Instalado.

![](imagenes/Pasted%20image%2020240216193131.png)



#### 17. Contraseñas guardadas.
![](imagenes/Pasted%20image%2020240216214047.png)

#### 18. Cuentas de Usuario

![](imagenes/Pasted%20image%2020240216193645.png)



#### 19. Historial de navegación y descargas. Cookies.

![](imagenes/Pasted%20image%2020240216193755.png)


![](imagenes/Pasted%20image%2020240216193828.png)

![](imagenes/Pasted%20image%2020240216193925.png)

![](imagenes/Pasted%20image%2020240216194022.png)

#### 20. Volúmenes cifrados

![](imagenes/Pasted%20image%2020240216194851.png)

#### 21. Archivos con extensión cambiada.

![](imagenes/Pasted%20image%2020240216194054.png)

![](imagenes/Pasted%20image%2020240216194219.png)

#### 22. Archivos eliminados.

![](imagenes/Pasted%20image%2020240216194339.png)

![](imagenes/Pasted%20image%2020240216194546.png)

#### 23. Archivos Ocultos.

![](imagenes/Pasted%20image%2020240216214223.png)

#### 24. Archivos que contienen una cadena determinada.

#### 25. Búsqueda de imágenes por ubicación.

![](imagenes/Pasted%20image%2020240216201149.png)

#### 26. Búsqueda de archivos por autor.

![](imagenes/Pasted%20image%2020240216201325.png)

## Apartado B) Máquina Linux.

Intenta realizar las mismas operaciones en una máquina Linux para aquellos apartados que tengan sentido y no se realicen de manera idéntica a Windows.