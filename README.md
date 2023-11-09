 # Balanceador de carga con haproxy y Datadog
![balanceador](balanceador.png)

## Descripción.
Este proyecto muestra cómo configurar un balanceador de cargas con HAProxy y tres máquinas de Ubuntu para mejorar la disponibilidad y escalabilidad de una aplicación web. Además, se utiliza Datadog para monitorear el rendimiento de una de las maquinas en este caso haproxy.

## Requisitos previos
Antes de comenzar con la configuración del balanceador de cargas, se deben cumplir los siguientes requisitos:

1. Tener tres máquinas virtuales de Ubuntu instaladas y configuradas en la misma red.
2. Tener privilegios de superusuario en las tres máquinas.
3. Tener instalado HAProxy en la máquina que actuará como balanceador de cargas.  
## Creacion del archivo Vagrantfile
Se debe ejecutar el comando `Vagrant init` para crear el archivo y se configura asi en cualqier editor de texto.

## Vagrantfile
 encoding: UTF-8
 -- mode: ruby --
 vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define :haproxy do |haproxy|  
   haproxy.vm.box = "bento/ubuntu-20.04"  
   haproxy.vm.network :private_network, ip: "192.168.100.5"  
   haproxy.vm.hostname = "haproxy"  
   end

config.vm.define :backend1 do |backend1|  
backend1.vm.box = "bento/ubuntu-20.04"  
 backend1.vm.network :private_network, ip: "192.168.100.6"  
 backend1.vm.hostname = "backend1"  
 end  
 
 
 config.vm.define :backend2 do |backend2|  
 backend2.vm.box = "bento/ubuntu-20.04"  
 backend2.vm.network :private_network, ip: "192.168.100.7"  
 backend2.vm.hostname = "backend2"  
 end  
 end  
 Una vez configurado el Vagranfile se debe ejecutar el comando `vagrant up` para la creacion de las tres maquinas.



# Configuración.
## Configuración de las máquinas virtuales.
1. Configuración de la primera máquina virtual

* Nombre de la maquina: haproxy
* IP: 192.168.100.5
* Sistema operativo: Ubuntu
* Servidor  instalado: haproxy y datadog-agent  
Se debe instalar el servicio de haproxy ejecutando el siguiente comando: `apt install haproxy`, y se inicia el servicio con  el comando: `sudo systemctl start haproxy`.  
tambien instalamos el agente de datadog con el comando ofrecido para la instalacion del agente en el cual se incluye la API KEY de la cuenta creada el cual nos enviara datos de las metricas monitoriadas en la interfaz grafica del dashboard realizado en datadog

2. Configuración de la segunda máquina virtual
* Nombre de la maquina: backend1
* IP: 192.168.100.6
* Sistema operativo: Ubuntu
* Servidor web instalado: Apache  
Se debe instalar el servicio de apache2 con el siguiente comando: `apt install apache2`, una vez instalado se debe crear un archivo index.html el cual se crea en la ruta `var/www/html` y por ultimo se Ejecuta el siguiente comando para iniciar el servicio de Apache: `sudo systemctl start apache2`.
 

3. Configuración de la tercera máquina virtual
* Nombre de la maquina: backend2
* IP: 192.168.100.7
* Sistema operativo: Ubuntu
* Servidor web instalado: Apache  
Se debe instalar el servicio de apache2 con el siguiente comando: `apt install apache2` una vez instalado se debe crear un archivo index.html el cual se crea en la ruta `var/www/html` y por ultimo se Ejecuta el siguiente comando para iniciar el servicio de Apache: `sudo service apache2 start`.


# Configuración del balanceador de cargas

1. Instalar HAProxy en la máquina que actuará como balanceador de cargas en este caso es la maquina con el nombre de haproxy.
2. Crear un archivo de configuración para HAProxy en `/etc/haproxy/haproxy.cfg`.
3. Configurar el archivo de configuración de HAProxy de la siguiente manera:

![haproxy.cfg](haproxy.cfg.jpg)
4. Reiniciar HAProxy para aplicar los cambios.

# Uso
Para usar el balanceador de cargas con HAProxy y tres máquinas de Ubuntu, sigue los siguientes pasos:

1. Accede al balanceador de cargas a través de su dirección IP en este caso es la maquina con el nombre de haproxy.
2. La solicitud será dirigida a uno de los dos servidores web de manera aleatoria asi:
![backend1](backend1.jpg)
![backend2](backend2.jpg)


# Configuración de Datadog

1. Crear la cuenta en datadog dando click en prueba gratuita.
![datadog-cuenta](datadog-cuenta.jpg)

2. luego llenas tus datos personales.
3. Te pidiran que instales el agente de datadog asi:
![agente.datadog](agente.datadog.jpg)
lo cual utilizas tu API KEY y utilizas el comando en la maquina en la que deseas instalar el agente datadog.
4. Una vez configurado el agente de datadog en la maquina damos click en infraestructura y despues en  mapa de anfitrion nos debe dejar observar el host asi:
![mapainfraestructura](mapainfraestrucrura.jpg)
5. luego vamos al apartado de dashboards y damos click en new dashboard y creamos.
![creacion.dashboard](creacion.dashboard.jpg)
6. Una vez creado el dashboard damos click en add widgets or Powerpacks.
![agregarmetricas](agregarmetricas.jpg)
7. Nos mostrara la siguiente ventana donde podemos escoger el tipo de grafico deseado.
![graficos](graficos.jpg)
8. Una vez se escoja el grafico se procede a configurar las metricas que se quieren visualizar y se dan click en save.
![configuracion](configuracion.jpg)
9. luego podemos observar el grafico ya creado en nuestro dashboard.
![grafico1](grafico1.jpg)  
Y asi podemos seguir creando los graficos deseados para nuestro dashboard.
# Contacto
Si tienes preguntas o sugerencias sobre este proyecto, por favor contáctame en mi correo electrónico: [jhon.lenn@uao.edu.co]:+1:.
