#Práctica 4: Comprobar el rendimiento de servidores web
###Javier Bolívar Valverde

##Apache Benchmark (ab)
Para mostrar los resultados de apache benchmark de forma gráfica he añadido los resultados de cada petición de ab en un archivo results.tsv y he usado el programa gnuplot con los parámetros del archivo adjunto gnu.p. De esta forma conseguimos visualizar más fácilmente las diferencias entre cargas y entre ejecuciones. He adjuntado las imágenes de las ejecuciones con el nombre ab[nombremaquina]_[numeroejecucion].png

###Comando usado en Apache Benchmark:
```
ab -n 1000 -c 100 -g results.tsv http://10.0.1.101/f.php
```

###Comando usado para dibujar la gráfica:
```
gnuplot gnu.p
```

###Resultados obtenidos con Apache Benchmark en maquina1:
####Prueba1:
```
Concurrency Level:      100
Time taken for tests:   45.437 seconds
Complete requests:      1000
Failed requests:        105
   (Connect: 0, Receive: 0, Length: 105, Exceptions: 0)
Total transferred:      209889 bytes
HTML transferred:       22889 bytes
Requests per second:    22.01 [#/sec] (mean)
Time per request:       4543.735 [ms] (mean)
Time per request:       45.437 [ms] (mean, across all concurrent requests)
Transfer rate:          4.51 [Kbytes/sec] received
```

####Prueba2:
```
Concurrency Level:      100
Time taken for tests:   44.742 seconds
Complete requests:      1000
Failed requests:        902
   (Connect: 0, Receive: 0, Length: 902, Exceptions: 0)
Total transferred:      209886 bytes
HTML transferred:       22886 bytes
Requests per second:    22.35 [#/sec] (mean)
Time per request:       4474.165 [ms] (mean)
Time per request:       44.742 [ms] (mean, across all concurrent requests)
Transfer rate:          4.58 [Kbytes/sec] received
```

####Prueba3:
```
Concurrency Level:      100
Time taken for tests:   45.132 seconds
Complete requests:      1000
Failed requests:        110
   (Connect: 0, Receive: 0, Length: 110, Exceptions: 0)
Total transferred:      209879 bytes
HTML transferred:       22879 bytes
Requests per second:    22.16 [#/sec] (mean)
Time per request:       4513.174 [ms] (mean)
Time per request:       45.132 [ms] (mean, across all concurrent requests)
Transfer rate:          4.54 [Kbytes/sec] received
```

####Prueba4:
```
Concurrency Level:      100
Time taken for tests:   2.809 seconds
Complete requests:      58
Failed requests:        6
   (Connect: 0, Receive: 0, Length: 6, Exceptions: 0)
Total transferred:      12174 bytes
HTML transferred:       1328 bytes
Requests per second:    20.65 [#/sec] (mean)
Time per request:       4842.340 [ms] (mean)
Time per request:       48.423 [ms] (mean, across all concurrent requests)
Transfer rate:          4.23 [Kbytes/sec] received
```

####Prueba5:
```
Concurrency Level:      100
Time taken for tests:   44.950 seconds
Complete requests:      1000
Failed requests:        116
   (Connect: 0, Receive: 0, Length: 116, Exceptions: 0)
Total transferred:      209870 bytes
HTML transferred:       22870 bytes
Requests per second:    22.25 [#/sec] (mean)
Time per request:       4495.039 [ms] (mean)
Time per request:       44.950 [ms] (mean, across all concurrent requests)
Transfer rate:          4.56 [Kbytes/sec] received
```


###Resultados obtenidos con Apache Benchmark en nginx:
####Prueba1:
```
Concurrency Level:      100
Time taken for tests:   23.831 seconds
Complete requests:      1000
Failed requests:        110
   (Connect: 0, Receive: 0, Length: 110, Exceptions: 0)
Total transferred:      199872 bytes
HTML transferred:       22872 bytes
Requests per second:    41.96 [#/sec] (mean)
Time per request:       2383.144 [ms] (mean)
Time per request:       23.831 [ms] (mean, across all concurrent requests)
Transfer rate:          8.19 [Kbytes/sec] received
```

####Prueba2:
```
Concurrency Level:      100
Time taken for tests:   23.876 seconds
Complete requests:      1000
Failed requests:        96
   (Connect: 0, Receive: 0, Length: 96, Exceptions: 0)
Total transferred:      199889 bytes
HTML transferred:       22889 bytes
Requests per second:    41.88 [#/sec] (mean)
Time per request:       2387.611 [ms] (mean)
Time per request:       23.876 [ms] (mean, across all concurrent requests)
Transfer rate:          8.18 [Kbytes/sec] received
```

####Prueba3:
```
Concurrency Level:      100
Time taken for tests:   23.827 seconds
Complete requests:      1000
Failed requests:        93
   (Connect: 0, Receive: 0, Length: 93, Exceptions: 0)
Total transferred:      199896 bytes
HTML transferred:       22896 bytes
Requests per second:    41.97 [#/sec] (mean)
Time per request:       2382.658 [ms] (mean)
Time per request:       23.827 [ms] (mean, across all concurrent requests)
Transfer rate:          8.19 [Kbytes/sec] received
```

####Prueba4:
```
Concurrency Level:      100
Time taken for tests:   23.767 seconds
Complete requests:      1000
Failed requests:        915
   (Connect: 0, Receive: 0, Length: 915, Exceptions: 0)
Total transferred:      199899 bytes
HTML transferred:       22899 bytes
Requests per second:    42.08 [#/sec] (mean)
Time per request:       2376.667 [ms] (mean)
Time per request:       23.767 [ms] (mean, across all concurrent requests)
Transfer rate:          8.21 [Kbytes/sec] received
```

####Prueba5:
```
Concurrency Level:      100
Time taken for tests:   23.871 seconds
Complete requests:      1000
Failed requests:        95
   (Connect: 0, Receive: 0, Length: 95, Exceptions: 0)
Total transferred:      199897 bytes
HTML transferred:       22897 bytes
Requests per second:    41.89 [#/sec] (mean)
Time per request:       2387.060 [ms] (mean)
Time per request:       23.871 [ms] (mean, across all concurrent requests)
Transfer rate:          8.18 [Kbytes/sec] received
```


###Resultados obtenidos con Apache Benchmark en haproxy:
####Prueba1:
```
Concurrency Level:      100
Time taken for tests:   23.725 seconds
Complete requests:      1000
Failed requests:        114
   (Connect: 0, Receive: 0, Length: 114, Exceptions: 0)
Total transferred:      209876 bytes
HTML transferred:       22876 bytes
Requests per second:    42.15 [#/sec] (mean)
Time per request:       2372.516 [ms] (mean)
Time per request:       23.725 [ms] (mean, across all concurrent requests)
Transfer rate:          8.64 [Kbytes/sec] received
```

####Prueba2:
```
Concurrency Level:      100
Time taken for tests:   23.829 seconds
Complete requests:      1000
Failed requests:        104
   (Connect: 0, Receive: 0, Length: 104, Exceptions: 0)
Total transferred:      209886 bytes
HTML transferred:       22886 bytes
Requests per second:    41.97 [#/sec] (mean)
Time per request:       2382.898 [ms] (mean)
Time per request:       23.829 [ms] (mean, across all concurrent requests)
Transfer rate:          8.60 [Kbytes/sec] received
```

####Prueba3:
```
Concurrency Level:      100
Time taken for tests:   23.355 seconds
Complete requests:      1000
Failed requests:        84
   (Connect: 0, Receive: 0, Length: 84, Exceptions: 0)
Total transferred:      209910 bytes
HTML transferred:       22910 bytes
Requests per second:    42.82 [#/sec] (mean)
Time per request:       2335.456 [ms] (mean)
Time per request:       23.355 [ms] (mean, across all concurrent requests)
Transfer rate:          8.78 [Kbytes/sec] received
```

####Prueba4:
```
Concurrency Level:      100
Time taken for tests:   24.114 seconds
Complete requests:      1000
Failed requests:        88
   (Connect: 0, Receive: 0, Length: 88, Exceptions: 0)
Total transferred:      209898 bytes
HTML transferred:       22898 bytes
Requests per second:    41.47 [#/sec] (mean)
Time per request:       2411.417 [ms] (mean)
Time per request:       24.114 [ms] (mean, across all concurrent requests)
Transfer rate:          8.50 [Kbytes/sec] received
```

####Prueba5:
```
Concurrency Level:      100
Time taken for tests:   23.404 seconds
Complete requests:      1000
Failed requests:        108
   (Connect: 0, Receive: 0, Length: 108, Exceptions: 0)
Total transferred:      209884 bytes
HTML transferred:       22884 bytes
Requests per second:    42.73 [#/sec] (mean)
Time per request:       2340.393 [ms] (mean)
Time per request:       23.404 [ms] (mean, across all concurrent requests)
Transfer rate:          8.76 [Kbytes/sec] received
```


##HTTPERF
A alguna gente le han salido exactamente los mismos resultados, pero a mi me ha funcionado sin problemas.

###Comando usado en httperf:
```
httperf --server 10.0.1.101 --port 80 --uri /f.php --rate 150 --num-conn 4500 --num-call 1 --timeout 5
```


###Resultados obtenidos con httperf en maquina1:
####Prueba1:
```
Total: connections 4500 requests 1019 replies 0 test-duration 37.865 s
Request rate: 26.9 req/s (37.2 ms/req)
Errors: total 4500 client-timo 4500 socket-timo 0 connrefused 0 connreset 0
```

####Prueba2:
```
Total: connections 4500 requests 1052 replies 89 test-duration 37.925 s
Request rate: 27.7 req/s (36.1 ms/req)
Errors: total 4411 client-timo 4411 socket-timo 0 connrefused 0 connreset 0
```

####Prueba3:
```
Total: connections 4500 requests 1120 replies 104 test-duration 37.875 s
Request rate: 29.6 req/s (33.8 ms/req)
Errors: total 4396 client-timo 4396 socket-timo 0 connrefused 0 connreset 0
```

####Prueba4:
```
Total: connections 4500 requests 1096 replies 95 test-duration 37.870 s
Request rate: 28.9 req/s (34.6 ms/req)
Errors: total 4405 client-timo 4405 socket-timo 0 connrefused 0 connreset 0
```

####Prueba5:
```
Total: connections 4500 requests 1056 replies 23 test-duration 37.995 s
Request rate: 27.8 req/s (36.0 ms/req)
Errors: total 4477 client-timo 4477 socket-timo 0 connrefused 0 connreset 0
```


###Resultados obtenidos con httperf en nginx:
####Prueba1:
```
Total: connections 4500 requests 4198 replies 761 test-duration 37.069 s
Request rate: 113.2 req/s (8.8 ms/req)
Errors: total 3739 client-timo 3739 socket-timo 0 connrefused 0 connreset 0
```

####Prueba2:
```
Total: connections 4500 requests 3772 replies 245 test-duration 37.317 s
Request rate: 101.1 req/s (9.9 ms/req)
Errors: total 4255 client-timo 4255 socket-timo 0 connrefused 0 connreset 0
```

####Prueba3:
```
Total: connections 4500 requests 3992 replies 481 test-duration 37.330 s
Request rate: 106.9 req/s (9.4 ms/req)
Errors: total 4019 client-timo 4019 socket-timo 0 connrefused 0 connreset 0
```

####Prueba4:
```
Total: connections 4500 requests 4275 replies 828 test-duration 36.842 s
Request rate: 116.0 req/s (8.6 ms/req)
Errors: total 3672 client-timo 3672 socket-timo 0 connrefused 0 connreset 0
```

####Prueba5:
```
Total: connections 4500 requests 4500 replies 1467 test-duration 34.988 s
Request rate: 128.6 req/s (7.8 ms/req)
Errors: total 3033 client-timo 3033 socket-timo 0 connrefused 0 connreset 0
```


###Resultados obtenidos con httperf en haproxy:
####Prueba1:
```
Total: connections 4500 requests 2063 replies 262 test-duration 37.994 s
Request rate: 54.3 req/s (18.4 ms/req)
Errors: total 4238 client-timo 4238 socket-timo 0 connrefused 0 connreset 0
```

####Prueba2:
```
Total: connections 4500 requests 2059 replies 282 test-duration 37.991 s
Request rate: 54.2 req/s (18.5 ms/req)
Errors: total 4218 client-timo 4218 socket-timo 0 connrefused 0 connreset 0
```

####Prueba3:
```
Total: connections 4500 requests 2147 replies 291 test-duration 37.998 s
Request rate: 56.5 req/s (17.7 ms/req)
Errors: total 4209 client-timo 4209 socket-timo 0 connrefused 0 connreset 0
```

####Prueba4:
```
Total: connections 4500 requests 2038 replies 303 test-duration 37.948 s
Request rate: 53.7 req/s (18.6 ms/req)
Errors: total 4197 client-timo 4197 socket-timo 0 connrefused 0 connreset 0
```

####Prueba5:
```
Total: connections 4500 requests 2065 replies 307 test-duration 37.921 s
Request rate: 54.5 req/s (18.4 ms/req)
Errors: total 4193 client-timo 4193 socket-timo 0 connrefused 0 connreset 0
```


##OpenWebLoad

###Comando usado en OpenWebLoad:
```
openload http://10.0.1.101/f.php 10
```


###Resultados obtenidos con OpenWebLoad en maquina1:
####Prueba1:
```
Total TPS:  21.84
Avg. Response time:  0.450 sec.
```

####Prueba2:
```
Total TPS:  21.92
Avg. Response time:  0.447 sec.
```

####Prueba3:
```
Total TPS:  22.25
Avg. Response time:  0.446 sec.
```

####Prueba4:
```
Total TPS:  21.91
Avg. Response time:  0.448 sec.
```

####Prueba5:
```
Total TPS:  22.03
Avg. Response time:  0.445 sec.
```


###Resultados obtenidos con OpenWebLoad en nginx:
####Prueba1:
```
Total TPS:  39.16
Avg. Response time:  0.250 sec.
```

####Prueba2:
```
Total TPS:  39.04
Avg. Response time:  0.250 sec.
```

####Prueba3:
```
Total TPS:  38.25
Avg. Response time:  0.255 sec.
```

####Prueba4:
```
Total TPS:  39.15
Avg. Response time:  0.248 sec.
```

####Prueba5:
```
Total TPS:  38.13
Avg. Response time:  0.256 sec.
```

###Resultados obtenidos con OpenWebLoad en haproxy:
####Prueba1:
```
Total TPS:  39.58
Avg. Response time:  0.248 sec.
```

####Prueba2:
```
Total TPS:  39.52
Avg. Response time:  0.248 sec.
```

####Prueba3:
```
Total TPS:  37.92
Avg. Response time:  0.259 sec.
```

####Prueba4:
```
Total TPS:  39.80
Avg. Response time:  0.247 sec.
```

####Prueba5:
```
Total TPS:  38.80
Avg. Response time:  0.250 sec.
```

##Siege

Este programa es el que he usado como adicional para subir nota en la práctica.

###Comando usado para siege:
```
siege -b -t30s http://10.0.1.101/f.php
```

###Resultados obtenidos con siege en maquina1:
####Prueba1:
```
Transactions:		         641 hits
Availability:		      100.00 %
Elapsed time:		       29.50 secs
Data transferred:	        0.01 MB
Response time:		        0.68 secs
Transaction rate:	       21.73 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.82
Successful transactions:         641
Failed transactions:	           0
Longest transaction:	        0.99
Shortest transaction:	        0.43
```

####Prueba2:
```
Transactions:		         643 hits
Availability:		      100.00 %
Elapsed time:		       29.47 secs
Data transferred:	        0.01 MB
Response time:		        0.68 secs
Transaction rate:	       21.82 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.84
Successful transactions:         643
Failed transactions:	           0
Longest transaction:	        1.14
Shortest transaction:	        0.44
```

####Prueba3:
```
Transactions:		         643 hits
Availability:		      100.00 %
Elapsed time:		       29.22 secs
Data transferred:	        0.01 MB
Response time:		        0.67 secs
Transaction rate:	       22.01 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.79
Successful transactions:         643
Failed transactions:	           0
Longest transaction:	        1.11
Shortest transaction:	        0.44
```

####Prueba4:
```
Transactions:		         659 hits
Availability:		      100.00 %
Elapsed time:		       29.84 secs
Data transferred:	        0.01 MB
Response time:		        0.67 secs
Transaction rate:	       22.08 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.83
Successful transactions:         659
Failed transactions:	           0
Longest transaction:	        0.77
Shortest transaction:	        0.58
```

####Prueba5:
```
Transactions:		         660 hits
Availability:		      100.00 %
Elapsed time:		       29.99 secs
Data transferred:	        0.01 MB
Response time:		        0.67 secs
Transaction rate:	       22.01 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.79
Successful transactions:         660
Failed transactions:	           0
Longest transaction:	        0.78
Shortest transaction:	        0.57
```

###Resultados obtenidos con siege en nginx:
####Prueba1:
```
Transactions:		        1215 hits
Availability:		      100.00 %
Elapsed time:		       29.79 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.79 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.87
Successful transactions:        1215
Failed transactions:	           0
Longest transaction:	        0.85
Shortest transaction:	        0.04
```

####Prueba2:
```
Transactions:		        1219 hits
Availability:		      100.00 %
Elapsed time:		       29.71 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       41.03 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.86
Successful transactions:        1219
Failed transactions:	           0
Longest transaction:	        0.75
Shortest transaction:	        0.04
```

####Prueba3:
```
Transactions:		        1224 hits
Availability:		      100.00 %
Elapsed time:		       29.47 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       41.53 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.86
Successful transactions:        1224
Failed transactions:	           0
Longest transaction:	        0.73
Shortest transaction:	        0.04
```

####Prueba4:
```
Transactions:		        1194 hits
Availability:		      100.00 %
Elapsed time:		       29.33 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.71 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.84
Successful transactions:        1194
Failed transactions:	           0
Longest transaction:	        0.77
Shortest transaction:	        0.04
```

####Prueba5:
```
Transactions:		        1193 hits
Availability:		      100.00 %
Elapsed time:		       29.33 secs
Data transferred:	        0.03 MB
Response time:		        0.37 secs
Transaction rate:	       40.68 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.85
Successful transactions:        1193
Failed transactions:	           0
Longest transaction:	        0.75
Shortest transaction:	        0.04
```

###Resultados obtenidos con siege en haproxy:
####Prueba1:
```
Transactions:		        1158 hits
Availability:		      100.00 %
Elapsed time:		       29.19 secs
Data transferred:	        0.03 MB
Response time:		        0.37 secs
Transaction rate:	       39.67 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.82
Successful transactions:        1158
Failed transactions:	           0
Longest transaction:	        0.81
Shortest transaction:	        0.04
```

####Prueba2:
```
Transactions:		        1209 hits
Availability:		      100.00 %
Elapsed time:		       29.64 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.79 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.82
Successful transactions:        1209
Failed transactions:	           0
Longest transaction:	        0.75
Shortest transaction:	        0.04
```

####Prueba3:
```
Transactions:		        1205 hits
Availability:		      100.00 %
Elapsed time:		       29.43 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.94 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.81
Successful transactions:        1205
Failed transactions:	           0
Longest transaction:	        0.75
Shortest transaction:	        0.04
```

####Prueba4:
```
Transactions:		        1207 hits
Availability:		      100.00 %
Elapsed time:		       29.65 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.71 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.84
Successful transactions:        1207
Failed transactions:	           0
Longest transaction:	        0.77
Shortest transaction:	        0.04
```

####Prueba5:
```
Transactions:		        1216 hits
Availability:		      100.00 %
Elapsed time:		       29.73 secs
Data transferred:	        0.03 MB
Response time:		        0.36 secs
Transaction rate:	       40.90 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       14.83
Successful transactions:        1216
Failed transactions:	           0
Longest transaction:	        0.74
Shortest transaction:	        0.04
```

