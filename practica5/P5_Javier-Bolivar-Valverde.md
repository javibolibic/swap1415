#Práctica 5: Replicación de bases de datos MySQL
###Javier Bolívar Valverde

##Creación de la base de datos:
```
root@maquina1:~# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 51
Server version: 5.5.41-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database contactos;
Query OK, 1 row affected (0.00 sec)

mysql> use contactos;
Database changed
mysql> show tables;
Empty set (0.00 sec)

mysql> create table datos(nombre varchar(100),tlf int);
Query OK, 0 rows affected (0.02 sec)

mysql> show tables
    -> ;
+---------------------+
| Tables_in_contactos |
+---------------------+
| datos               |
+---------------------+
1 row in set (0.00 sec)

mysql> insert into datos(nombre,tlf) values ("pepe",95834656);
Query OK, 1 row affected (0.01 sec)

mysql> select * from datos
    -> ;
+--------+----------+
| nombre | tlf      |
+--------+----------+
| pepe   | 95834656 |
+--------+----------+
1 row in set (0.00 sec)

mysql> describe datos;
+--------+--------------+------+-----+---------+-------+
| Field  | Type         | Null | Key | Default | Extra |
+--------+--------------+------+-----+---------+-------+
| nombre | varchar(100) | YES  |     | NULL    |       |
| tlf    | int(11)      | YES  |     | NULL    |       |
+--------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> quit
Bye
```

##Clonado de la base de datos con mysqldump:

Para evitar cambios en la base de datos mientras se está clonando, la bloqueamos:
```
root@maquina1:~# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 52
Server version: 5.5.41-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> FLUSH TABLES WITH READ LOCK;
Query OK, 0 rows affected (0.01 sec)

mysql> quit
Bye
```
Procedemos a exportar la base de datos:
```
root@maquina1:~# mysqldump contactos -u root -p > /root/contactos.sql
Enter password: 
root@maquina1:~# 
```
Desbloqueamos de nuevo la base de datos:
```
root@maquina1:~# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 56
Server version: 5.5.41-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> UNLOCK TABLES;
Query OK, 0 rows affected (0.00 sec)

mysql> quit
Bye
```
Copiamos en la otra máquina la base de datos a la otra máquina y la importamos a mysql:
```
root@maquina2:~# scp root@10.0.1.101:/root/contactos.sql /root/
contactos.sql                                                                      100% 1873     1.8KB/s   00:00    
root@maquina2:~# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 50
Server version: 5.5.41-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE contactos;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| contactos          |
| mysql              |
| performance_schema |
| phpmyadmin         |
+--------------------+
5 rows in set (0.00 sec)

mysql> quit
Bye

root@maquina2:~# mysql -u root -p contactos < /root/contactos.sql 
Enter password: 
root@maquina2:~# 
```
Ya tenemos la base de datos copiada. No obstante, este es un proceso manual y no es cómodo. Lo mejor es usar una arquitectura Maestro-Esclavo o Maestro-Maestro.

##Montando una arquitectura Maestro-Esclavo en MySQL

Antes de empezar debemos tener clonadas las bases de datos en los dos servidores. Para ello podemos configurar esta arquitectura desde el principio o usar mysqldump manualmente.
Para la replicación con arquitectura Maestro-Esclavo empezaremos por modificar la configuración de MySQL del maestro, en el que debemos cambiar las siguientes líneas:
```
#bind-address 127.0.0.1
log_error = /var/log/mysql/error.log
server-id = 1
log_bin = /var/log/mysql/bin.log
```
Guardamos el documento y reiniciamos el servicio con:
```
/etc/init.d/mysql restart
```
Ahora procederemos a la configuración del esclavo, cambiando en versiones superiores a MySQL 5.5 el siguiente parámetro:
```
server-id= 2
```
En el maestro, en MySQL, escribimos lo siguiente:
```
mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo'
    -> ;
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH TABLES;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH TABLES WITH READ LOCK;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW MASTER STATUS
    -> ;
+------------+----------+--------------+------------------+
| File       | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------+----------+--------------+------------------+
| bin.000009 |      492 |              |                  |
+------------+----------+--------------+------------------+
1 row in set (0.00 sec)

mysql> 
```
Volvemos al esclavo y escribimos:
```
mysql> CHANGE MASTER TO MASTER_HOST='10.0.1.101', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='bin.000009', MASTER_LOG_POS=492, MASTER_PORT=3306;
Query OK, 0 rows affected (0.01 sec)

mysql> START SLAVE;
```
Véase que he puesto en *MASTER_LOG_FILE* y en *MASTER_LOG_POS* lo mismo que pone en el apartado *FILE* y *Position*, respectivamente. Ya tenemos configurada la replicación en cuestión.
