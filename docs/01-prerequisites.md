# Kubernetes The Hard Way - 01-prerequisites.md

## Prerrequisitos y Entorno Local

Antes de iniciar el aprovisionamiento, es obligatorio validar que el equipo anfitrión cumple con las capacidades técnicas para levantar máquinas virtuales y que cuenta con las herramientas de orquestación necesarias.

### Comprobación de Virtualización por Hardware
El procesador de tu equipo anfitrión debe soportar virtualización por hardware (Intel VT-x o AMD-V) y esta debe estar habilitada en la BIOS/UEFI. Puedes verificarlo con los siguientes comandos según tu sistema operativo:

En esta guía adaptamos los prerrequisitos oficiales al uso de Multipass como entorno local en lugar de un jumpbox tradicional, manteniendo la lógica paso a paso para desplegar el clúster.

# Comprobacion virtualizacion hardware:

Get-ComputerInfo | Select-Object HyperVRequirement*

PowerShell
multipass version
(Nota: En caso de no tenerlo instalado, se puede obtener desde el sitio oficial de Multipass o mediante Winget ejecutando:
winget install Canonical.Multipass).

### Herramientas y Entorno
Para este despliegue utilizaremos Multipass en tu máquina local para gestionar las máquinas virtuales Ubuntu 24.04, omitiendo la necesidad de configurar conexiones SSH externas complejas gracias a la gestión directa por la shell de Multipass.


Los requisitos del cluster seran en mi caso para un equipo de 8GRAM:

Name	    Description	            CPU	RAM	Storage
controller	Kubernetes server	    1	1.5GB	10GB
worker-0    Kubernetes worker node	1	1GB	10GB
worker-1    Kubernetes worker node	1	1GB	10GB