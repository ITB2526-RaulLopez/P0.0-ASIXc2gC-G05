# Arquitectura

## 1. Objetivo
Desplegar la infraestructura para una aplicación multicapa (servidor web, base de datos, SSH, DHCP, DNS y FTP), con un router que conecta tres redes: DMZ, Intranet y NAT.

## 2. Diagrama

![Diagrama de arquitectura](arquitectura.png)

## 3. Visión general
La infraestructura se despliega en AWS, dentro de una VPC. Una única instancia EC2 con Ubuntu 24.04 ejecuta Docker, y los equipos del proyecto son contenedores conectados mediante tres redes Docker. El router R-05 es un contenedor con reenvío IP activado que comunica las tres redes. Se accede a la instancia por SSH desde el PC de clase.

## 4. Capa AWS

| Elemento | Valor |
|---|---|
| VPC | VPC-G05, 10.0.0.0/16 |
| Subred | 10.0.1.0/24 (pública) |
| Security Group | SG-G05 |
| Instancia | EC2 Ubuntu 24.04 con Docker |

## 5. Redes

| Red | Subred | Función |
|---|---|---|
| DMZ | 172.20.1.0/24 | Servicios expuestos (servidor web) |
| Intranet | 172.20.2.0/24 | Servicios internos y clientes |
| NAT | 172.20.3.0/24 | Salida a Internet |

## 6. Equipos

| Hostname | Función | Red |
|---|---|---|
| R-05 | Router con reenvío IP | DMZ, Intranet y NAT |
| W-05 | Servidor web y SSH | DMZ |
| B-05 | Base de datos MySQL | Intranet |
| F-05 | Servidor FTP | Intranet |
| DHCP / DNS | Asignación de direcciones y resolución de nombres (R-05 y R) | Intranet |
| Cliente Linux | Equipo de pruebas | Intranet |
| Cliente Windows | Equipo de pruebas | Intranet |

## 7. Usuario común
Todos los sistemas tienen el usuario `bchecker`, como exige el enunciado.

## 8. Decisiones de diseño
- **Una instancia con Docker en lugar de una instancia por servicio:** reduce el coste, acelera el despliegue y permite reproducir todo con ficheros de configuración guardados en el repositorio.
- **Router como contenedor:** permite simular el enrutamiento entre las tres redes dentro de la misma instancia.
- **Cliente Windows:** Docker en Linux no ejecuta contenedores Windows, por lo que este cliente se despliega como instancia EC2 Windows o máquina virtual aparte.

## 9. Enlaces
- [Red](../red/README.md)
- [Servicios](../servicios/README.md)
- [Clientes](../clientes/README.md)
