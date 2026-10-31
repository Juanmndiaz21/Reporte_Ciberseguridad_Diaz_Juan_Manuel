# Reporte de Práctica: Hardening Inicial y Configuración de Entorno

| **Materia** | Ciberseguridad |
| :--- | :--- |
| **Alumno** | Juan Manuel Diaz |
| **Comisión** | 105720 |

---

##  Objetivo
Documentar el aprovisionamiento, configuración de red, gestión de privilegios y medidas preliminares de bastionado (*hardening*) sobre una máquina virtual en VirtualBox, estableciendo un entorno controlado y seguro para prácticas de ciberseguridad.

---

##  Tabla de Contenidos
1. [Configuración de Red en VirtualBox](#1-configuración-de-red-en-virtualbox)
2. [Gestión de Usuarios y Principio de Menor Privilegio](#2-gestión-de-usuarios-y-principio-de-menor-privilegio)
3. [Permisos en Linux y Actualización de Paquetes](#3-permisos-en-linux-y-actualización-de-paquetes)
   - [3.1. Inspección de Permisos en el Sistema de Archivos](#31-inspección-de-permisos-en-el-sistema-de-archivos)
   - [3.2. Actualización de Paquetes y Parches](#32-actualización-de-paquetes-y-parches)
4. [Snapshot Inicial (Punto de Restauración)](#4-snapshot-inicial-punto-de-restauración)

---

## 1. Configuración de Red en VirtualBox

Para iniciar la preparación del laboratorio, se modificó la interfaz de red de la máquina virtual en VirtualBox sustituyendo el modo por defecto (*NAT*) por **Adaptador puente** (*Bridged Adapter*).

- **Direccionamiento directo:** Permite que la máquina virtual adquiera su propia dirección IP dentro de la misma subred que el host físico, facilitando pruebas de conectividad bidireccional y análisis directo de tráfico.
- **Modo Promiscuo (`Permitir todo`):** Habilita a la interfaz a capturar la totalidad de los paquetes que circulen por el segmento de red, capacidad esencial para tareas de monitoreo, análisis de protocolos e inspección de tráfico con herramientas como Wireshark o tcpdump.

<div align="center">
  <img width="811" height="514" alt="Configuración de Adaptador Puente y Modo Promiscuo en VirtualBox" src="https://github.com/user-attachments/assets/7b6d46b4-ef71-4351-bc86-4b504690980d" />
  <p><em>Figura 1: Configuración de red en modo Adaptador puente y activación de Modo Promiscuo.</em></p>
</div>

---

## 2. Gestión de Usuarios y Principio de Menor Privilegio

Operar cotidianamente como usuario `root` representa un riesgo crítico de seguridad. Para adherir al principio de menor privilegio, se creó un usuario estándar sin privilegios administrativos para las tareas operativas habituales:

```bash
sudo adduser usuarioComun
```

Tras ingresar los parámetros requeridos, se verificó la persistencia y atributos del nuevo usuario inspeccionando las entradas finales del archivo `/etc/passwd`:

```bash
tail -n 5 /etc/passwd
```

<div align="center">
  <img width="643" height="516" alt="Creación de usuario sin privilegios y verificación en /etc/passwd" src="https://github.com/user-attachments/assets/7a8ed5c9-e721-4aa1-af47-fe3685480c2c" />
  <p><em>Figura 2: Creación del usuario estándar y verificación en /etc/passwd.</em></p>
</div>

---

## 3. Permisos en Linux y Actualización de Paquetes

### 3.1. Inspección de Permisos en el Sistema de Archivos
Para verificar el modelo de permisos discrecionales (DAC) en Linux, se creó un archivo de prueba y se auditaron sus metadatos con `ls -l`:

```bash
touch reporte_seguridad.txt
ls -l reporte_seguridad.txt
```

La salida detalla los permisos asignados según la máscara (*umask*), observándose permisos de lectura y escritura para el propietario y grupo, y de solo lectura para otros (`-rw-rw-r--`).

<div align="center">
  <img width="642" height="515" alt="Verificación de permisos de archivo en Linux" src="https://github.com/user-attachments/assets/1903cb16-5f8a-410e-b0cb-8f30f951f519" />
  <p><em>Figura 3: Comprobación de atributos y permisos con touch y ls -l.</em></p>
</div>

### 3.2. Actualización de Paquetes y Parches
Mantener el sistema actualizado es una práctica fundamental de bastionado para mitigar vulnerabilidades conocidas. Se sincronizaron los repositorios y se aplicaron los últimos parches de seguridad disponibles:

```bash
sudo apt update && sudo apt upgrade -y
```

<div align="center">
  <img width="647" height="517" alt="Actualización de paquetes y repositorios" src="https://github.com/user-attachments/assets/822f4d2d-42c9-4445-b8e3-74f568ede0dd" />
  <p><em>Figura 4: Actualización integral del sistema operativo mediante apt.</em></p>
</div>

---

## 4. Snapshot Inicial (Punto de Restauración)

Una vez completadas las configuraciones de red, la creación del usuario sin privilegios y la actualización integral de paquetes, se capturó una instantánea (*snapshot*) del estado de la máquina virtual bajo la etiqueta **"Hardening Inicial"** (o *"Clean Install - Hardening applied"*).


<div align="center">
  <img width="954" height="749" alt="Snapshot inicial del sistema en VirtualBox" src="https://github.com/user-attachments/assets/81cca208-57b9-4796-9625-0a4358b1cd2c" />
  <p><em>Figura 5: Registro del snapshot de respaldo en VirtualBox.</em></p>
</div>
