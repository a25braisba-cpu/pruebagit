# Guía Práctica de Bastionado Básico en Servidores Linux

Esta guía técnica está diseñada para administradores de sistemas y usuarios con conocimientos básicos que necesiten asegurar un servidor Linux recien instalado. El objetivo es aplicar las directivas de seguridad informática fundamentales para reducir la superficie de ataque del sistema.

![Servidor Linux Seguro](https://images.unsplash.com/photo-1618401471353-b98afee0b2eb?q=80&w=1000&auto=format&fit=crop)

---

## Tabla de Contenidos

- [Requisitos Previos](#requisitos-previos)
1. [Actualización y Mantenimiento del Sistema](#1-actualización-y-mantenimiento-del-sistema)
- [2. Gestión de Usuarios y Permisos](#2-gestión-de-usuarios-y-permisos)
- [3. Configuración Segura del Servidor SSH](#3-configuración-segura-del-servidor-ssh)
- [4. Configuración del Cortafuegos (UWF)](#4-configuración-del-cortafuegos-uwf)
- [5. Resumen de Comandos Principales](#5-resumen-de-comandos-principales)
- [Recursos y Referencias](#recursos-y-referencias)

---

## Requisitos Previos

Antes de comenzar la configuración, asegúrate de contar con los siguientes elementos:

- Un servidor con sistema operativo **Ubuntu Server 22.04 LTS** o superior.
- Acceso con privilegios de superusuario (`root`) o un usuario con permisos `sudo`.
- Conexión a Internet activa.

---

## 1. Actualización y Mantenimiento del Sistema

Mantener los paquetes del sistema actualizados es la primera línea de defensa frente a vulnerabilidades conocidas.

Ejecuta los siguientes comandos para actualizar el índice de paquetes y aplicar las actualizaciones de seguridad disponibles:

```bash
# Actualizar el listado de repositorios
sudo apt update

# Actualizar los paquetes instalados
sudo apt upgrade -y

# Eliminar paquetes obsoletos no requeridos
sudo apt autoremove -y
```

## 2. Gestión de Usuarios y Permisos

```bash
#Para creación de usuarios
sudo adduser operador

#Para asignar permisos administrativos
sudo usermod -aG sudo operador

#Para verificar la asignación de grupos
groups operador
```

## 3. Configuración Segura del Servidor SSH

El servicio SSH es uno de los vectores de ataque mas expuestos. Aplicaremos cambios en la configuracion central ubicada en /etc/ssh/sshd_config

![Conexión Segura SSH](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?q=80&w=1000&auto=format&fit=crop)

¡¡IMPORTANTE!!
Antes de modificar el archivo de configuracion, realiza una copia de seguridad del original:

```bash
#El comando seria tal que así:
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

## 4. Configuración del Cortafuegos (UWF)

El cortafuegos permite bloquear todo el tráfico entrante no deseado, autorizando únicamente los puertos strictly necesarios.

### Pasos para la configuración

#### Establecer las políticas por defecto

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 2222/tcp comment 'Puerto SSH personalizado'
```

Habilitar tráfico web estándar(si procede)

```bash
sudo ufw allow 80/tcp comment 'HTTP'
sudo ufw allow 443/tcp comment 'HTTPS'

sudo ufw enable
sudo ufw status verbose
```

## 5. Resumen de Comandos Principales

| Categoría | Comando | Descripción |
|---|---|---|
| Paquetes | `sudo apt update && sudo apt upgrade` | Actualiza los repositorios y el sistema. |
| Usuarios | `sudo usermod -aG sudo` | Concede permisos de superusuario. |
| Servicios | `sudo systemctl restart ssh` | Aplica la nueva configuración del servidor SSH. |
| Seguridad | `sudo ufw status verbose` | Muestra el estado y las reglas activas del cortafuegos. |


## Recomendaciones de seguridad

1. No subir credenciales, claves API ni contraseñas al repositorio.
2. Mantener actualizado el archivo `.gitignore` para omitir archivos sensibles y temporales.
3. Verificar los cambios mediante `git diff` antes de confirmar cualquier commit.

## Información del proyecto
Esta documentación se ha elaborado utilizando Markdown.
El control de versiones se realiza mediante Git
y el repositorio remoto se encuentra alojado en GitHub.

## Próximas mejoras
- Primera mejora sugerida
- Segunda mejora sugerida
- Tercera mejora sugerida
