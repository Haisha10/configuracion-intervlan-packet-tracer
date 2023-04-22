# REPASO DE PROCEDIMIENTO DE CONFIGURACIÓN CISCO PACKET TRACER
REDES Y COMUNICACIONESD DE DATOS


## 1. SEGURIDAD BÁSICA

Esta es la configuración de seguridad básica para los dispositivos de red como los **switches** y **los routers**.

```
enable
configure terminal
hostname S1
enable secret upc123
line console 0
password upc321
login
exit
line vty 0 15
password upc321
login
exit
banner motd $ *** SOLO PERSONAL AUTORIZADO *** $
do write
do write memory
do copy run start
```
