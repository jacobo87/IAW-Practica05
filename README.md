# IAW - Práctica 5
>IES Celia Viñas (Almería) - Curso 2020/2021 
>Módulo: IAW - Implantación de Aplicaciones Web 
>Ciclo: CFGS Administración de Sistemas Informáticos en Red 

## Balanceador de carga con Apache

En esta práctica deberá automatizar la instalación y configuración de una aplicación web LAMP en cuatro máquinas virtuales **EC2** de **Amazon Web Services (AWS)**.  En esta práctica vamos a usar una máquina virtual con Apache HTTP Server como un proxy inverso para hacer de balanceador de carga. 
El objetivo de esta práctica es crear una arquitectura de alta disponibilidad que sea escalable y redundante, de modo que podamos balancear la carga entre todos los frontales web.

La arquitectura estará formada por:

- Un balanceador de carga, implementado con un Apache HTTP Server configurado como proxy inverso.
- Una capa de front-end, formada por dos servidores web con Apache HTTP Server.
- Una capa de back-end, formada por un servidor MySQL.

Necesitará crear cuatro máquinas virtuales:

- Balanceador.
- Frontal Web 1.
- Frontal Web 2.
- Servidor de Base de Datos MySQL.

- [Enlace repositorio con herramientas necesarias.][GitHub]

### 1.1 Arquitectura típica de proxy inverso con Apache
![image1](images/índice.png "índice")
#### Activamos los siguientes módulos:
```bash
a2enmod proxy
a2enmod proxy_http
a2enmod proxy_ajp
a2enmod rewrite
a2enmod deflate
a2enmod headers
a2enmod proxy_balancer
a2enmod proxy_connect
a2enmod proxy_html
a2enmod lbmethod_byrequests
```
#### 1.3 Configuración de Apache para trabajar como balanceador de carga
Editamos el archivo 000-default.conf que está en el directorio /etc/apache2/sites-enabled:
```bash
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
Añadimos las directivas Proxy y ProxyPass dentro de VirtualHost.
```bash
<VirtualHost *:80>
    # Dejamos la configuración del VirtualHost como estaba
    # sólo hay que añadir las siguiente directivas: Proxy y ProxyPass

    <Proxy balancer://mycluster>
        # Server 1
        BalancerMember http://IP-HTTP-SERVER-1

        # Server 2
        BalancerMember http://IP-HTTP-SERVER-2
    </Proxy>

    ProxyPass / balancer://mycluster/
</VirtualHost>
```
### 1.3.1 Reiniciamos el servicio de Apache
Una vez aplicados los cambios reiniciamos el servicio de Apache:
```bash
sudo /etc/init.d/apache2 restart
```
## Referencias
- https://josejuansanchez.org/iaw/practica-05/index.html

[GitHub]: https://github.com/jacobo87/IAW-Practica04
[Repo]: https://github.com/josejuansanchez/iaw-practica-lamp