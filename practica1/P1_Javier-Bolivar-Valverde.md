#Práctica 1
Realizada por Javier Bolívar Valverde


Los resultados de ejecutar los comandos *apache2 -v* y *ps aux | grep apache* son:

*ajbolivar@ubuntu:~$ apache2 -v*

*Server version: Apache/2.4.7 (Ubuntu)*

*Server built:   Jul 22 2014 14:36:38*


*ajbolivar@ubuntu:~$ ps aux | grep apache*

*root       1365  0.0  4.4 276256 21568 ?        Ss   22:28   0:00 /usr/sbin/apache2 -k start*

*www-data   1429  0.0  1.5 276328  7348 ?        S    22:28   0:00 /usr/sbin/apache2 -k start*

*www-data   1430  0.0  1.5 276280  7348 ?        S    22:28   0:00 /usr/sbin/apache2 -k start*

*www-data   1431  0.0  1.5 276280  7348 ?        S    22:28   0:00 /usr/sbin/apache2 -k start*

*www-data   1432  0.0  1.7 276320  8336 ?        S    22:28   0:00 /usr/sbin/apache2 -k start*

*www-data   1433  0.0  1.7 276328  8344 ?        S    22:28   0:00 /usr/sbin/apache2 -k start*

*ajboliv+   1783  0.0  0.4  14288  2192 pts/0    S+   22:43   0:00 grep --color=auto apache*

