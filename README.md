# Trabajo Final Integrador - Backend

![Ansible](https://img.shields.io/badge/config-Ansible-black?logo=ansible&logoColor=white)

## Introducción
Este repositorio contiene la configuración de la Raspberry Pi y los archivos para deploy de los jobs del trabajo final **Análisis, visibilidad de tráfico y seguridad para usuarios finales en redes hogareñas**.

## Instalación
### Prerequisitos
* Una Raspberry Pi 
* Un teclado, un mouse y un monitor para instalar el sistema operativo en la Raspberry
* Una computadora para la descarga de la imagen del sistema operativo de la Raspberry (Nota: para este caso, se usará una MacBook Pro).
* Una tarjeta microSD
* Un adaptador para microSD
* Un cable HDMI y un cable Ethernet
* Un celular con Telegram
* Permisos sobre los repositorios de código de Github

### Configuración inicial
#### Instalación del SO en la Raspberry Pi
1. Descargar Raspberry Pi Imager de la página oficial de Raspberry Pi https://www.raspberrypi.com/software/, un software que permite descargar imágenes y llevarlas a tarjetas SD.
2. Ejecutar la aplicación.
3. Conectar la tarjeta microSD en el adaptador y a la computadora.
4. Apretar el botón Elegir dispositivo y seleccionar Raspberry Pi 4.
5. Apretar el botón Elegir SO, y seleccionar la imagen Raspberry Pi OS (32-bit).
6. Apretar el botón Elegir almacenamiento y elegir la opción de la tarjeta que aparece reconocida por la propia computadora.
7. Apretar Siguiente y luego Yes. En este momento, la imagen del sistema operativo será copiada a la tarjeta microSD. 

Conectar a la Raspberry Pi:
* La microSD.
* El teclado y el mouse en puertos USB.
* El monitor con el conector HDMI.
* La fuente de alimentación.
* Cuando el dispositivo se conecte a la fuente de alimentación, se levantará la imagen desde la memoria microSD y aparecerá la imagen del escritorio del sistema operativo en la pantalla.

#### Contraseña y consola
1. En el menú que aparece en pantalla, seleccionar la locación Argentina.
2. Configurar usuario y contraseña.
    * Usuario: Pi.
    * Contraseña: elegida por el usuario que esté configurando el dispositivo.
3. No es necesario configurar ninguna red WiFi.
4. Hacer clic en Restart.
5. En el menú principal, buscar Configuración de Raspberry Pi.
6. En la pestaña Sistema, Iniciar en seleccionar Consola para evitar que se lance el escritorio cuando se inicia sesión y que sólo sea la terminal.
7. Hacer clic en Ok para cerrar la ventana.

#### Habilitar acceso SSH
1. En el menú principal, buscar Configuración de Raspberry Pi.
2. Ir a la pestaña Interfaces.
3. En la fila de SSH, seleccionar Activado.
4. Cerrar la ventana haciendo clic en Ok.

#### Configuración de acceso remoto
* En la computadora
1. Crear un par de llaves: público (compartida con el servidor) y privado (que se mantiene en la computadora local).
    * Ejecutar el siguiente comando en una terminal.
        ````
        $ ssh-keygen -t rsa
        ```
    * Indicar el nombre del archivo.
    * Se puede indicar una passphrase. En este caso, se omite este paso para que no se deba indicar ese código cada vez que se autentica el usuario por SSH.
    * Para este punto ya se tienen dos archivos ```rpi_rsa``` y ```rpi_rsa.pub ```
2. Conectar la Raspberry Pi con el cable Ethernet al router del proveedor.
3. Instalar nmap. ```$ brew install nmap```
4. Sabiendo la red de tu wifi, correr el siguiente comando para listar los dispositivos presentes en la red. (Puede tardar algunos minutos) ```$ sudo nmap -O 192.168.0.0/24```
5. A partir de este comando se tendrá una lista de dispositivos con sus sistemas operativos detectados. Buscar el que corresponda a la Raspberry Pi.
6. Con la IP obtenida (para este caso, 192.168.0.13), correr el siguiente comando para conectarse a la Raspberry por SSH con el usuario pi e ingresar la contraseña establecida en la sección Configuración inicial > Usuario, contraseña y boot. 
```$ ssh pi@192.168.0.13```
7. Una vez verificada la conexión, desde la computadora local copiar la llave pública que se creó en el paso 1 ejecutando el siguiente comando.
```$ ssh-copy-id -i ~/.ssh/rpi_rsa.pub pi@192.168.0.13```
8. Probar el acceso sin contraseña, usando la llave privada.
