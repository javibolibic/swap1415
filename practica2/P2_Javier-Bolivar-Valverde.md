#PRACTICA 2: 

##1. HABILITAR USUARIO ROOT

Para poder autenticarnos como root en **maquina1** y **maquina2** debemos usar los siguientes comandos:

*ajbolivar@maquina1:~$ sudo su*

*[sudo] password for ajbolivar:*

*root@maquina1:~# passwd root*

*Introduzca la nueva contraseña de UNIX:*

*Vuelva a escribir la nueva contraseña de UNIX:*

*passwd: password updated successfully*


##2. CREAR UN FICHERO TAR EN UN EQUIPO REMOTO

En **maquina1** he usado el siguiente comando para copiar el directorio */var/www/html* comprimido en el directorio de usuario de **maquina2**:

*root@maquina1:~# tar -czf - /var/www/html/ | ssh 10.0.1.102 'cat > ~/tar.tgz'*

*tar: Eliminando la `/' inicial de los nombres*


Verificamos que en la otra máquina se ha creado con:

*root@maquina2:~# ls*

*tar.tgz*



##3. SINCRONIZAR UNA CARPETA EN DOS SERVIDORES

En **maquina1**, dentro del directorio /var/www/html/ tenemos los archivos hola.html y index.html. Queremos sincronizarlos para que se copien en **maquina2**. Para ello usaremos la herramienta rsync. Esta herramienta permite que solo se copien los archivos que se han modificado, reduciendo así el ancho de banda y el uso de CPU y discos duros. Usaremos el comando:

**rsync -avz -e ssh origen destino**


En nuestro caso:

*root@maquina1:~# rsync -avz -e ssh /var/www/ root@10.0.1.102:/var/www/*

*sending incremental file list*

*./*

*html/*

*html/hola.html*

*html/index.html*


*sent 3,493 bytes  received 65 bytes  2,372.00 bytes/sec*

*total size is 11,561  speedup is 3.25*



Verificamos que se han creado los archivos en **maquina2**:

*root@maquina2:~# tree /var/www/*

*/var/www/*

└── html

    ├── hola.html
    
    └── index.html
    


##4. ACCESO SIN CONTRASEÑA PARA SSH

Para conseguir autenticar por ssh sin contraseña he elegido el **tipo de cifrado rsa**, que tarda más en generarse que el tipo dsa, pero para la verificación tarda menos, por lo que es más apropiado.

*root@maquina1:~# ssh-keygen*

*Generating public/private rsa key pair.*

*Enter file in which to save the key (/root/.ssh/id_rsa):* 

*/root/.ssh/id_rsa already exists.*

*Overwrite (y/n)? y*

*Enter passphrase (empty for no passphrase):* 

*Enter same passphrase again:*

*Your identification has been saved in /root/.ssh/id_rsa.*

*Your public key has been saved in /root/.ssh/id_rsa.pub.*

*The key fingerprint is:*

*dc:06:9f:16:71:a6:bb:1f:49:c4:2d:e0:18:e9:0b:fa root@maquina1*

*The key's randomart image is:*

*+--[ RSA 2048]----+*

*|        ..o o    |*

*|        .+ B .   |*

*|       .o + + .  |*

*|      ...+ = .   |*

*|     . .S.B .    |*

*|    .   .o o .   |*

*|     .    . o    |*

*|      E    . .   |*

*|            .    |*

*+-----------------+*


Ahora añadimos nuestra clave al equipo **maquina2** para añadirlo a su **authorized_keys**:

*root@maquina1:~# ssh-copy-id 10.0.1.102*

*/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed*

*/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys*

*root@10.0.1.102's password:*


*Number of key(s) added: 1*


*Now try logging into the machine, with:   "ssh '10.0.1.102'"*

*and check to make sure that only the key(s) you wanted were added.*



Podemos copiar la clave al equipo **maquina2** para que sea la misma que se ha generado por **maquina1**:

*root@maquina1:~# scp .ssh/id_rs* 10.0.1.102:/root/.ssh/*

*id_rsa                                                    100% 1675     1.6KB/s   00:00*

*id_rsa.pub                                                100%  407     0.4KB/s   00:00*


Procedemos a añadir la clave de **maquina1** a su propio **authorized_keys** para poder acceder desde **maquina2** a **maquina1**:

*root@maquina:~/.ssh# ssh-copy-id 10.0.1.101*

*/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed*

*/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys*

*root@10.0.1.101's password:* 


*Number of key(s) added: 1*


*Now try logging into the machine, with:   "ssh '10.0.1.101'"*

*and check to make sure that only the key(s) you wanted were added.*


##5. PROGRAMAR TAREAS CON CRONTAB

Para que se sincronicen los archivos del directorio **/var/www** cada hora entre las dos máquinas debemos modificar el archivo **/etc/crontab**

Añadimos las siguientes líneas en **maquina1** de la siguiente forma:

*0 * * * * root rsync -avz -e ssh --delete --exclude=**/error /var/www/ root@10.0.1.102:/var/www/ >> /var/log/rsync.log*

Si necesitamos **modificar archivos** deberemos hacerlo en **maquina1**, si lo hacemos en maquina2 cuando se ejecute el comando los archivos modificados se borrarán y se sustituirán por los que hay en maquina1.

##REFERENCIAS:
*https://linuxcode.wordpress.com/2009/08/08/autentificacion-mediante-claves-publicas-en-ssh/*
