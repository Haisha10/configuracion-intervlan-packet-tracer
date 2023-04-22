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


## 2. ROUTER: SUBINTERFACES

Habilitar la *interfaz* del **router** que se conecte con el **switch**

```
interface G0/0
no shutdown
```

Habilitar la *interfaz* de la **VLAN** para que tenga *enlace troncal* con la *encapsulación dot1q*.

> El comando encapsulation dot1q es el protocolo que permite que el router tenga enlace troncal.
>> Un enlace troncal es un enlace punto a punto entre dos dispositivos de red que lleva más de una VLAN.

* VLAN 10
```
interface G0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

* VLAN 20
```
interface G0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

* VLAN 99 *(Por defecto o de Management & Native)*
```
interface G0/0.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
```
