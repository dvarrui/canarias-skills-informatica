```
Propuesto por   : IES Puerto de la Cruz
Tiempo estimado : 8 horas
Estado          : EN DESARROLLO
```

#1. Monitorización con Nagios

Vamos a montar una red de equipos según el siguiente esquema.

[ESQUEMA]
* NagiosServer, Router, Cliente1, Cliente2

> El valor XX se sustituye por el número asignado a cada alumno.

#2. Servidor Nagios

* Configuración de la máquina:
    * SO Debian 8 - GNU/Linux
    * Hostname `nagiosXXserver`
    * IP estática 172.19.XX.41/16
    * Incluir en el `/etc/hosts` todas las máquinas de la práctica.
* Instalar el servidor Nagios3.
* Usaremos el protocolo NRPE para comunicar el servidor con los clientes.
* Usuario de Nagios `nagiosadmin` con clave `DNI-del-alumno-sin-letra`

* Crear el directorio `/etc/nagios3/nombre-del-alumno.d`, para
guardar nuestras configuraciones.
* Modificamos fichero de configuración principal de Nagios, para que tengan en
cuenta los ficheros de configuración del directorio `/etc/nagios3/nombre-del-alumno.d`.
* Configurar el servidor Nagios con:
   * Comprobar se están activos `nagiosXXcliente1` y `nagiosXXcliente2`.
   * Crear fichero `/etc/nagios3/nombre-del-alumno.d/gruposXX.cfg`.
   * Hay que definir 3 grupos de hosts: `routersXX`, `servidoresXX` y `clientesXX`.
   * Crear el fichero `/etc/nagios3/nombre-del-alumno.d/grupo-de-routersXX.cfg`
   para incluir las definiciones de las máquinas de tipo router.
   * El router caronteXX tiene como padre a benderXX.
   * Crear el fichero `/etc/nagios3/nombre-del-alumno.d/grupo-de-servidoresXX.cfg`
   para incluir las definiciones de las máquinas de tipo servidor.
   * Crear el fichero `/etc/nagios3/nombre-del-alumno.d/grupo-de-clientesXX.cfg`
   para incluir las definiciones de las máquinas de tipo cliente.
* Monitorizar en cliente1
  * la carga de CPU, y el espacio en disco.
* Monitorizar en cliente2
   * la carga de CPU, y el espacio en disco.

#3. Router
* Configuración de la máquina `nagiosXXrouter`:
    * SO Debian 8 - GNU/Linux para ser monitorizado.
    * Hostname `nagiosXXrouter`
    * IP estática 172.19.XX.42/16
    * Incluir en el `/etc/hosts` todas las máquinas de la práctica.
* Instalar y configurar el cliente Nagios y los paquetes para NRPE.
* Configuración:
    * Se usarán los puertos por defecto.
    * ¿Más de una IP en el cliente? SI
    * Restringir el acceso al cliente Nagios a la IP de `nagiosXXserver`

#4. Nagios Cliente1
* Configuración de la máquina `nagiosXXcliente1`:
    * SO Debian 8 - GNU/Linux para ser monitorizado.
    * Hostname `nagiosXXclient1`
    * IP estática 172.19.XX.42/16
    * Incluir en el `/etc/hosts` todas las máquinas de la práctica.

* Instalar y configurar el cliente Nagios y los paquetes para NRPE.
* Configuración:
    * Se usarán los puertos por defecto.
    * ¿Más de una IP en el cliente?
    * Restringir el acceso al cliente Nagios a la IP de `nagiosXXserver`
* Configurar Comandos
    * alias check_load para obtener la carga de CPU
    * alias check_disk para obtener el espacio disponible en el disco /dev/sda
    y alertar si queda menos de 20% de espacio en alguna partición.

#5. Nagios Cliente2
* Configuración de la máquina `cliente2`:
    * SO Windows 7 para ser monitorizado.
    * Hostname: `nagiosclient2`
    * IP estática 172.19.XX.11/16
    * Incluir en el fichero `c:\Windows\System32\drivers\etc\hosts` todas las máquinas de la práctica.
* Instalar el programa nsclient.
    * Descargar el programa Agente Windows (NSCLient++)
    * Recomendado [http://nsclient.org/nscp/downloads](http://nsclient.org/nscp/downloads).
    * [http://www.nagios.org/download/addons](http://www.nagios.org/download/addons).

> * En este caso hemos elegido NRPE como protocolo de comunicación entre el agente
Windows y el servidor Nagios.

* Usar la configuración del archivo `C:\Program Files\NSClient++\nsclient.ini`
 (o `C:\Archivos de Programas\NSClient++\nsclient.ini`).
* Configurar el cliente con:
    * Desactivar el password
    * Permitimos el acceso sólo al servidor Nagios para las consultas.
    * Usamos protocolo NRPE
    * Habilitar plugins CheckSystem, CheckDisk y CheckExternalScripts
* Configurar comandos
    * Alias check_load para chequear la carga de CPU. Si sobrepasa
    el 80% en un intervalo de 5 minutos, nos alertará.
    * Alias check_disk para chequear el espacio en todos los discos del servidor
