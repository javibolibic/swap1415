#1. INSTALACIÓN Y CONFIGURACIÓN DEL BALANCEADOR NGINX

##1.1. INSTALACIÓN

Lo primero que debemos hacer es **importar la clave** del repositorio de software:

*root@nginx:/tmp# wget http://nginx.org/keys/nginx_signing.key*

*--2015-03-17 18:34:26--  http://nginx.org/keys/nginx_signing.key*

*Resolviendo nginx.org (nginx.org)... 206.251.255.63*

*Conectando con nginx.org (nginx.org)[206.251.255.63]:80... conectado.*

*Petición HTTP enviada, esperando respuesta... 200 OK*

*Longitud: 1559 (1,5K) [text/plain]*

*Grabando a: “nginx_signing.key”*


*100%[========================================================>] 1.559       --.-K/s   en 0s*


*2015-03-17 18:34:26 (5,54 MB/s) - “nginx_signing.key” guardado [1559/1559]*


*root@nginx:/tmp# apt-key add /tmp/nginx_signing.key*

*OK*

*root@nginx:/tmp# rm -f /tmp/nginx_signing.key*

##1.2. CONFIGURACIÓN

Editamos el **archivo de configuración de nginx**:

*root@nginx:~# nano /etc/nginx/conf.d/default.conf*


Este fichero debe quedar así:

*upstream apaches {*

        *server 10.0.1.101;*

        *server 10.0.1.102;*

*}*


*server {*

    *listen       80;*

    *server_name  m3lb;*


    *#charset koi8-r;*

    *access_log  /var/log/nginx/m3lb.access.log;*

    *error_log   /var/log/nginx/m3lb.error.log;*

    *root        /var/www/;*


    *location / {*

        *proxy_pass http://apaches;*

        *proxy_set_header Host $host;*

        *proxy_set_header X-Real-IP $remote_addr;*

        *proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;*

        *proxy_http_version 1.1;*

        *proxy_set_header Connection "";*

    *}*

*}*

##1.3. PRUEBAS

Para comprobar que nginx funciona he parado el servicio **cron** para que no sincronicen las máquinas y he puesto **dos webs distintas** en cada máquina:

*root@maquina1:~# service cron stop*

*cron stop/waiting*


En **maquina1** me muestra en el html el texto *maquina1* y en **maquina2** me muestra el texto *maquina2*.


Ejecutamos las pruebas en la máquina host, que hace de cliente:

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina2*

	*</body>*

*</html>*

##1.4. CONFIGURACIONES ADICIONALES

Ahora supongamos que **maquina1** es más potente que **maquina2**. Haremos que de cada **4** peticiones que se realicen, **3** vayan a **maquina1** y **1** vaya a **maquina2**. Para ello modificamos el archivo de configuración */etc/nginx/conf.d/default.conf* dándole a **maquina1** un *weight=3* y a **maquina2** un *weight=1*:

*upstream apaches {*

        *server 10.0.1.101 weight=3;*

        *server 10.0.1.102 weight=1;*

*}*


Reiniciamos el servicio **nginx**:

*root@nginx:~# service nginx restart*

 *\* Restarting nginx nginx*



Ahora comprobaremos su funcionamiento. Si hacemos **curl** desde el cliente obtenemos:


*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina2*

	*</body>*

*</html>*


Implementamos en la configuración de nginx los parámetros **ip_hash** y **keepalive**, para que siempre nos redireccione la misma ip cliente al mismo servidor y que no cierre las conexiones TCP, respectivamente:


*upstream apaches {*

        *server 10.0.1.101 weight=3;*

        *server 10.0.1.102 weight=1;*

        *keepalive 3;*

        *ip_hash;*

*}*



Reiniciamos **nginx** y probamos de nuevo que funciona y esta vez sólo nos sirve un único servidor:


*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.104/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*



#2. INSTALACIÓN Y CONFIGURACIÓN DEL BALANCEADOR HAPROXY

##2.1. INSTALACIÓN

Para la **instalación de haproxy** usamos el siguiente comando:

*root@haproxy:~# apt-get install haproxy*


##2.2. CONFIGURACIÓN

Como la **configuración que haproxy** tiene por defecto no nos vale, la eliminamos y creamos otro archivo vacío:

*root@haproxy:~# rm /etc/haproxy/haproxy.cfg*

*root@haproxy:~# nano /etc/haproxy/haproxy.cfg*



A este archivo le añadimos la siguiente configuración:


*global*

        *daemon*

        *maxconn 256*


*defaults

        *mode http*

        *contimeout 4000*

        *clitimeout 42000*

        *srvtimeout 43000*


*frontend http-in

        *bind \*:80*

        *default_backend servers*


*backend servers

        *server  m1 10.0.1.101:80 maxconn 32*

        *server  m2 10.0.1.102:80 maxconn 32*


**Lanzamos el servicio haproxy** mediante el comando:

*root@haproxy:~# /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg*


##2.3. PRUEBAS

Ya está todo configurado. Ahora **probamos** que funciona el balanceo:

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina2*

	*</body>*

*</html>*


##2.4. CONFIGURACIONES ADICIONALES

Ahora vamos a añadirle **ponderación** al balanceador. Supongamos **maquina1 más potente que maquina2**. Vamos a configurar haproxy de forma que de cada **4** peticiones **3** vayan a maquina1 y **1** vaya a maquina2. Para ello, en el archivo de configuracion */etc/haproxy/haproxy.cfg* añadimos *weight* a los servidores:

*server  m1 10.0.1.101:80 maxconn 32 weight 3*

*server  m2 10.0.1.102:80 maxconn 32 weight 1*



**Reiniciamos** el servicio haproxy:

*root@haproxy:~# /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg*


Y desde el cliente **probamos** la configuración realizada:


*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina1*

	*</body>*

*</html>*

*jnb@jnb-pcbuntu ~ $ curl http://10.0.1.105/prueba.html*

*<html>*

	*<body>*

		*maquina2*

	*</body>*

*</html>*
