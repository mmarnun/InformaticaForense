# InformaticaForense

La informática forense es el conjunto de técnicas que nos permite obtener la máxima información posible tras un incidente o delito informático.

En esta práctica, realizarás la fase de toma de evidencias y análisis de las mismas sobre una máquina Linux y otra Windows. Supondremos que pillamos al delincuente in fraganti y las máquinas se encontraban encendidas. Opcionalmente, podéis realizar el análisis de un dispositivo Android.

Sobre cada una de las máquinas debes realizar un volcado de memoria y otro de disco duro, tomando las medidas necesarias para certificar posteriormente la cadena de custodia.

Debes tratar de obtener las siguientes informaciones:

## Apartado A) Máquina Windows.

### Volcado de memoria
![](Pasted%20image%2020240207174001.png)

![](Pasted%20image%2020240207174130.png)

![](Pasted%20image%2020240207174151.png)

![](Pasted%20image%2020240207174402.png)


### Volcado de disco duro

![](Pasted%20image%2020240207174451.png)

![](Pasted%20image%2020240207174511.png)


![](Pasted%20image%2020240207174545.png)

![](Pasted%20image%2020240207174653.png)

![](Pasted%20image%2020240207174704.png)

![](Pasted%20image%2020240207174930.png)

![](Pasted%20image%2020240207175044.png)


![](Pasted%20image%2020240207175102.png)

![](Pasted%20image%2020240207181038.png)

![](Pasted%20image%2020240207181403.png)

![](Pasted%20image%2020240207182648.png)

![](Pasted%20image%2020240207182708.png)


#### 1. Procesos en ejecución.

2. Servicios en ejecución.

3. Puertos abiertos.

4. Conexiones establecidas por la máquina.

5. Sesiones de usuario establecidas remotamente.

6. Ficheros transferidos recientemente por NetBios.

7. Contenido de la caché DNS.

8. Variables de entorno.

9. Dispositivos USB conectados

10. Redes wifi utilizadas recientemente.

11. Configuración del firewall de nodo.

12. Programas que se ejecutan en el Inicio.

13. Asociación de extensiones de ficheros y aplicaciones.

14. Aplicaciones usadas recientemente.

15. Ficheros abiertos recientemente.

16. Software Instalado.

17. Contraseñas guardadas.

18. Cuentas de Usuario

19. Historial de navegación y descargas. Cookies.

20. Volúmenes cifrados

21. Archivos con extensión cambiada.

22. Archivos eliminados.

23. Archivos Ocultos.

24. Archivos que contienen una cadena determinada.

25. Búsqueda de imágenes por ubicación.

26. Búsqueda de archivos por autor.

Apartado B) Máquina Linux.

Intenta realizar las mismas operaciones en una máquina Linux para aquellos apartados que tengan sentido y no se realicen de manera idéntica a Windows.

Apartado C)

En un dispositivo Android, trata de hacer un volcado de memoria y recuperar información de ubicación, llamadas, mensajes, aplicaciones de mensajería, perfiles en redes sociales, etc...