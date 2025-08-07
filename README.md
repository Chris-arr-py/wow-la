# Arquitectura de vMaNGOS en Oracle Cloud

## 🔧 Descripción general

Este repositorio documenta la infraestructura de un servidor vMaNGOS Classic (1.12.1) alojado en Oracle Cloud utilizando servicios gratuitos y buenas prácticas de seguridad.

## 🧱 Componentes principales

### 1. Network Load Balancer
- Escucha en puerto 3724 TCP (realmd)
- Redirige tráfico a instancia backend sin IP pública

### 2. Subredes
- **Pública**: Load balancer + Internet Gateway
- **Privada**: Nodo de aplicación + NAT Gateway

### 3. Nodo de aplicación (Compute)
- Instancia `Ampere A1` sin IP pública
- Sistema operativo: Ubuntu 22.04
- Corre `LXD` como sistema de contenedores ligeros

### 4. Contenedores LXC
- `realmd`: Servidor de autenticación
- `mangosd`: Servidor del mundo (game server)
- `mysql`: Base de datos MariaDB con acceso local

## 🔄 Flujo de red

1. El usuario se conecta a través del NLB al contenedor `realmd`.
2. Una vez autenticado, se conecta al `mangosd` (puerto 8085, solo red interna).
3. Los contenedores acceden a Internet mediante el NAT Gateway (por actualizaciones o dependencias externas).

## 🔐 Seguridad
- No se exponen IPs públicas
- Acceso solo vía SSH desde túneles o VPN privada
- Firewalls estrictos configurados en VCN y LXD

## 📊 Diagrama

![Diagrama de Arquitectura](./infraestructura-vmangos.png)
