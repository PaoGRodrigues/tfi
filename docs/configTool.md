# Trabajo Final Integrador - Raspberry Pi
## Instalación
### Instalación de la herramienta de monitoreo
#### Instalación de ntopng

Para instalar la versión de la herramienta que se utiliza en el presente trabajo (6.1.240628), seguir los siguientes pasos:

1. Ingresar a la Raspberry por SSH usando la clave privada.
2. Cambiar a usuario root ejecutando ```$ sudo su -```
3. Agregar los repositorios desde donde se descargan los paquetes y hacer una actualización de los mismos.

    ```
    $ cat >> /etc/apt/sources.list.d/bullseye.list <<EOF
    $ deb http://deb.debian.org/debian bullseye main contrib non-free
    $ deb-src http://deb.debian.org/debian bullseye main contrib non-free
    EOF
    $ wget https://packages.ntop.org/RaspberryPI/apt-ntop.deb 
    $ dpkg -i apt-ntop.deb
    $ apt-get update 
    ```
4. Instalar los paquetes iniciales.
    ```$ apt-get install libssl1.1 libldap-2.4.2 nprobe```
5. Descargar los paquetes de la herramienta con wget.
    ```
    $ wget -O ntopng.deb https://packages.ntop.org/RaspberryPI/bullseye_pi/arm64/ntopng/ntopng_6.1.240628-23672_arm64.deb
    $ wget -O ntopng-data.deb https://packages.ntop.org/RaspberryPI/bullseye_pi/all/ntopng-data/ntopng-data_6.1.240628_all.deb
    $ wget -O ndpi.deb https://packages.ntop.org/RaspberryPI/bullseye_pi/arm64/ndpi/ndpi_4.9.0-4846_arm64.deb
    ```
6. Instalar los paquetes descargados.
    ```
    $ dpkg -i ndpi
    $ dpkg -i ntopng-data
    $ dpkg -i ntopng
    ```
7. Para iniciar la herramienta y comenzar el análisis, ejecutar
```$ sudo ntopng --community -i wlan0 -w 192.168.0.13:3000```

Este último comando iniciará la herramienta en versión _Community_, haciendo análisis sobre el tráfico que pase por la interfaz **wlan0** (la interfaz configurada como _access point_ en la Raspberry) y dejará disponible el _frontend web_ de la propia herramienta en la dirección IP de la Raspberry Pi en el puerto **3000**. Si bien este _frontend_ está disponible, no se usará de manera activa en la solución planteada. 

Por cuestiones de seguridad, la primera vez que se accede a la herramienta obliga a cambiar la contraseña por defecto del usuario administrador. Esta función no está disponible vía Restful API, por lo que se debe acceder por única vez a la IP donde está localizada la herramienta para poder realizar el cambio.
Si no se cambia la contraseña por defecto, los endpoints de la API de la herramienta no podrán utilizarse.

1. Acceder desde un navegador a la IP de la herramienta.
2. Ingresar usuario **admin** y contraseña **admin**, contraseña por defecto del usuario administrador.
3. Ingresar la nueva contraseña, que se usará luego en los parámetros del _backend_.
4. Hacer clic en _Change Password_.

#### Geolocalización
Se configuró la geolocalización de **ntopng** según la [documentación](https://github.com/ntop/ntopng/blob/dev/doc/README.geolocation.md) de la herramienta.

1. Crear una cuenta en [MaxMind](https://www.maxmind.com/) de donde se descargarán las bases de datos de IPs geolocalizadas.
2. Iniciar sesión y en el _My Account > Managed License Keys_.
3. Crear una nueva llave. Agregar una descripción y hacer clic en _Confirm_.
4. Copiar la llave.
5. Ejecutar los siguientes comandos, reemplazando el **YOUR_ACCOUNT_ID** y el **YOUR_LICENSE_KEY** por los datos obtenidos anteriormente.

    * Descargar la base de datos de ASN
    ```$ curl -O -J -L -u YOUR_ACCOUNT_ID:YOUR_LICENSE_KEY 'https://download.maxmind.com/geoip/databases/GeoLite2-ASN/download?suffix=tar.gz'```

    * Descargar la base de datos de países
    ```$ curl -O -J -L -u YOUR_ACCOUNT_ID:YOUR_LICENSE_KEY 'https://download.maxmind.com/geoip/databases/GeoLite2-Country/download?suffix=tar.gz'```

    * Descargar la base de datos de ciudades
    ```$ curl -O -J -L -u YOUR_ACCOUNT_ID:YOUR_LICENSE_KEY 'https://download.maxmind.com/geoip/databases/GeoLite2-City/download?suffix=tar.gz'```
6. Descomprimir los archivos descargados con el comando.
```$ tar -xvzf NombreDelArchivo```
7. Crear un directorio y hacer _backup_ de las bases de datos que trae la herramienta instaladas por defecto. Ejecutar los siguientes comandos.
    ```
    $ cd /usr/share/ntopng/httpdocs/geoip/
    $ sudo mkdir backup
    $ sudo mv *mmdb backup
    ```
8. Mover los tres archivos de los tres directorios (ASN, Países y Ciudades) al directorio creado en el paso anterior.
9. Detener y volver a iniciar la herramienta.
    ```
    $ sudo systemctl stop ntopng
    $ sudo ntopng --community -i wlan0 -w 192.168.0.13:3000
    ```
    
    Cambiar la IP de la Raspberry Pi según sea necesario.