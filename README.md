# REPASO DE PROCEDIMIENTO DE CONFIGURACIÓN CISCO PACKET TRACER

REDES Y COMUNICACIONES DE DATOS

## 1. SEGURIDAD BÁSICA

Esta es la configuración de seguridad básica para los dispositivos de red como los **switches** y **los routers**.

`Switch>`

```kotlin
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

`Router(config)#`

```kotlin
interface G0/0
no shutdown
```

Habilitar las *subinterfaces* de las **VLANs** para que tenga *enlace troncal* con la *encapsulación dot1q* y asignarles una *dirección IP*.

> El comando encapsulation dot1q es el protocolo que permite que el router tenga enlace troncal.
>> Un enlace troncal es un enlace punto a punto entre dos dispositivos de red que lleva más de una VLAN.

`Router(config)#`

* VLAN 10 *(Marketing)*

```kotlin
interface G0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

* VLAN 20 *(Ventas)*

```kotlin
interface G0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

* VLAN 99 *(Management & Native)*

```kotlin
interface G0/0.99
encapsulation dot1Q 99 native
ip address 192.168.99.1 255.255.255.0
```

### 2.1 MULTILAYER SWITCH

Si se usa un **switch multicapa** se puede obviar la configuración del router, ya que el **switch multicapa** se encargará de *gestionar la interVLAN*. Su configuración es similar al del router. Se seleccionan los puertos que se conectarán a los **switches** y solo se les aplica el *modo de enlace truncal*, no es necesario especificar la *encapsulación dot1q* en modelos como **3560** y **2950** al ser más modernos por defecto usan *encapsulación dot1q*. Esto se puede [verficar](https://community.cisco.com/t5/switching/multilayer-switch-rejects-the-command-quot-switchport-trunk/td-p/4663969) usando el comando: `MLS# show interface G1/0/11 switchport`

`MLS(config)#`

```kotlin
ip routing
VLAN 10
name MKT
VLAN 20
name VTAS
VLAN 99
name Management&Native
exit
interface Vlan10
ip address 192.168.10.1 255.255.255.0
interface Vlan20
ip address 192.168.20.1 255.255.255.0
interface Vlan99
ip address 192.168.99.1 255.255.255.0
interface G1/0/11
switchport mode trunk
switchport trunk native vlan 99

switchport trunk encapsulation dot1q
```

[Más Información](https://www.comparitech.com/net-admin/inter-vlan-routing-configuration/)

## 3. SWITCH: VLANS

Habilitar las **VLANs** en los **switches** y asignarles un nombre.

`Switch(config)#`

```kotlin
VLAN 10
name MKT
VLAN 20
name VTAS
VLAN 99
name Management&Native
```

## 4. SWITCH: PUERTOS VLANS

Habilitar las **interfaces (puertos)** donde se conectarán los **dispositivos finales** (PC, laptop, impresora, etc.) para que tenga un *switchport en modo de acceso*.

> El comando `switchport mode access` fuerza al puerto a convertirse en un puerto de acceso para que cualquier dispositivo conectado a este puerto solo pueda comunicarse con otros dispositivos que esten en la misma VLAN.

`Switch(config)#`

* VLAN 10

```kotlin
interface range f0/2, f0/4, f0/6, f0/8, f0/10
switchport mode access
switchport access vlan 10
```

* VLAN 20

```kotlin
interface range f0/3, f0/5, f0/7, f0/9, f0/11
switchport mode access
switchport access vlan 20
```

## 5. SWITCH: TRUNKING

Habilitar las **interfaces (puertos)** donde se conectarán otros **dispositivos de red** (switch y router) para que tenga un *switchport en modo truncal*.

> El comando `switchport mode trunk` coloca la interfaz en modo de enlace troncal permanente y negocia para convertir el enlace en un enlace troncal.
>> Los puertos de enlace troncal son los enlaces entre switches que admiten la transmisión de tráfico asociado a más de una VLAN.

`Switch(config)#`

* Switch 1

```kotlin
interface range G0/1, f0/1-2
switchport mode trunk
switchport trunk native vlan 99
```

* Switch 2

```kotlin
interface f0/1
switchport mode trunk
switchport trunk native vlan 99
```

* Switch 3

```kotlin
interface f0/1
switchport mode trunk
switchport trunk native vlan 99
```

## 6. SWITCH: SVI

Habilitar las **interfaces virtuales** de las **VLANs** y asignarles una dirección IP a los **switches** y **puerta de enlace predeterminada** en la **VLAN**

> Una SVI es una interfaz virtual configurada en un switch multicapa, como se muestra en la ilustración. Se puede crear una SVI para cualquier VLAN que exista en el switch.
> ![SVI](https://i.imgur.com/IxO07EM.png)

`Switch(config)#`

* Switch 1

```kotlin
interface vlan 99
ip address 192.168.99.11 255.255.255.0
exit
ip default-gateway 192.168.99.1
```

* Switch 2

```kotlin
interface vlan 99
ip address 192.168.99.12 255.255.255.0
exit
ip default-gateway 192.168.99.1
```

* Switch 3

```kotlin
interface vlan 99
ip address 192.168.99.13 255.255.255.0
exit
ip default-gateway 192.168.99.1
```

## 7. ROUTER: DHCP

Se puede habilitar el servicio de aginación automática de IPs **DHCP** en el **router**, se pueden excluir rangos de direcciones para que no se asignen dentro de dicho rango y tambíen puede asignar direcciones IP dependiendo de la **VLAN**

`Router(config)#`

* VLAN 10

```kotlin
ip dhcp excluded-address 192.168.10.1 192.168.10.50
ip dhcp pool POOL-VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.1.254
```

* VLAN 20

```kotlin
ip dhcp excluded-address 192.168.20.1 192.168.20.50
ip dhcp pool POOL-VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.1.254
```

Creado por [Alvaro @2023](https://github.com/Haisha10)
