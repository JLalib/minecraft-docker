# Servidor Minecraft en Docker

![Minecraft Logo](https://www.minecraft.net/etc.clientlibs/minecraft/clientlibs/main/resources/img/minecraft-logo.png)

Monta tu propio servidor de Minecraft con Docker Compose en minutos, sin complicaciones.

## 🎮 ¿Por qué usar Docker para Minecraft?

- **Fácil de configurar**: No necesitas instalar Java ni dependencias adicionales.
- **Aislado**: El servidor se ejecuta en un contenedor, separado del resto de tu sistema.
- **Fácil de actualizar**: Simplemente actualiza la imagen de Docker y reinicia el contenedor.
- **Persistencia de datos**: Los mundos y la configuración se guardan en un volumen de Docker.
- **Multiplataforma**: Funciona en cualquier sistema que soporte Docker (Linux, Windows, macOS).

## 📋 Características principales

- **Servidor oficial de Minecraft**: Usa la imagen oficial de `itzg/minecraft-server`.
- **Configurable**: Ajusta la versión, memoria, dificultad, modo de juego y más mediante variables de entorno.
- **Persistente**: Los datos del mundo se guardan en `./data`.
- **Fácil de gestionar**: Comandos simples para iniciar, detener y actualizar el servidor.
- **Compatible con mods y plugins**: Puedes cambiar el tipo de servidor a Paper, Spigot, Forge, etc.

## 📋 Requisitos del sistema

- Docker y Docker Compose instalados en tu servidor.
- Al menos 2 GB de RAM disponibles (más si planeas usar mods o muchos jugadores).
- Espacio en disco suficiente para tus mundos y plugins.

## 🚀 Instalación rápida

1. **Crear directorio y archivo compose**

```bash
mkdir -p ~/minecraft
cd ~/minecraft
nano docker-compose.yml
```

2. **Configurar docker-compose.yml**

Pega el siguiente contenido:

```yaml
version: '3.8'

services:
  minecraft:
    image: itzg/minecraft-server:latest
    container_name: minecraft-server
    ports:
      - "25565:25565"
    environment:
      EULA: "TRUE"
      TYPE: "VANILLA"
      VERSION: "LATEST"
      MEMORY: "2G"
      SERVER_NAME: "Mi Servidor Docker"
      MOTD: "Bienvenido a mi servidor Minecraft"
      MAX_PLAYERS: 20
      DIFFICULTY: "normal"
      MODE: "survival"
      VIEW_DISTANCE: 10
    volumes:
      - ./data:/data
    restart: unless-stopped
    tty: true
    stdin_open: true
```

> **Nota**: Ajusta las variables de entorno según tus necesidades:
> - `VERSION`: Especifica la versión de Minecraft (ej: "1.20.1", "LATEST").
> - `TYPE`: Tipo de servidor (VANILLA, PAPER, SPIGOT, FORGE, etc.).
> - `MEMORY`: Cantidad de RAM asignada (ej: "2G", "4G").
> - `SERVER_NAME`: Nombre que verán los jugadores en el servidor.
> - `MOTD`: Mensaje del día que aparece en la lista de servidores.
> - `MAX_PLAYERS`: Número máximo de jugadores.
> - `DIFFICULTY`: Dificultad (peaceful, easy, normal, hard).
> - `MODE`: Modo de juego (survival, creative, adventure, spectator).
> - `VIEW_DISTANCE`: Distancia de renderizado de chunks.

3. **Iniciar el contenedor**

```bash
docker compose up -d
```

El primer inicio puede tardar unos minutos mientras se descarga la imagen y se genera el mundo.

4. **Verificar el estado**

```bash
docker compose logs -f minecraft-server
docker compose ps
```

5. **Conectar al servidor**

Abre tu cliente de Minecraft y conéctate a `tu-ip:25565` (o `localhost:25565` si estás en la misma máquina).

## ⚙️ Configuración avanzada

### Cambiar el tipo de servidor (Paper, Spigot, Forge, etc.)

Modifica la variable `TYPE` en el archivo `docker-compose.yml`:

```yaml
environment:
  - TYPE=PAPER  # o SPIGOT, FORGE, FABRIC, etc.
```

Luego reinicia el contenedor:

```bash
docker compose up -d
```

### Aumentar la memoria asignada

Ajusta la variable `MEMORY`:

```yaml
environment:
  - MEMORY=4G  # o 8G, según tus necesidades
```

### Habilitar RCON

Para habilitar el acceso remoto por consola (RCON):

```yaml
environment:
  - ENABLE_RCON=TRUE
  - RCON_PASSWORD=tu_contraseña_segura
  - RCON.PORT=25575
```

Luego puedes conectarte a `tu-ip:25575` con un cliente RCON usando la contraseña que estableciste.

### Añadir mods o plugins

- **Para mods (Fabric/Forge)**: Colócalos en la carpeta `./data/mods`.
- **Para plugins (Paper/Spigot)**: Colócalos en la carpeta `./data/plugins`.

Después de agregar o modificar mods/plugins, reinicia el contenedor.

## 🔧 Gestión del contenedor

### Ver logs en tiempo real

```bash
docker compose logs -f minecraft-server
```

### Reiniciar el contenedor

```bash
docker compose restart minecraft-server
```

### Detener y eliminar

```bash
docker compose down
```

### Actualizar a la última versión

```bash
docker compose pull
docker compose up -d
```

### Respaldar tu mundo

Los datos del mundo se encuentran en la carpeta `./data`. Para hacer una copia de seguridad:

```bash
docker compose down
tar -czf minecraft-backup-$(date +%Y%m%d).tar.gz ./data
docker compose up -d
```

### Restaurar desde una copia de seguridad

```bash
docker compose down
tar -xzf minecraft-backup-20260419.tar.gz
docker compose up -d
```

## 💡 Consejos y trucos

- **Monitoriza el uso de recursos**: Usa `docker stats` para ver el consumo de CPU y memoria.
- **Optimiza el rendimiento**: Ajusta `VIEW_DISTANCE` y `MAX_TICK_TIME` según tu hardware.
- **Seguridad**: No expongas el puerto 25565 directamente a Internet sin un firewall o VPN si no es necesario.
- **Actualizaciones regulares**: Mantén actualizada la imagen de Docker para obtener las últimas características y parches de seguridad.
- **Copia de seguridad automática**: Considera usar un contenedor adicional o un script cron para hacer copias de seguridad periódicas.

## 🙏 Créditos

- Imagen Docker: [itzg/minecraft-server](https://hub.docker.com/r/itzg/minecraft-server)
- Tutorial de instalación y configuración: [Genbyte Blog](https://genbyte.blogspot.com/2026/04/como-instalar-y-configurar-servidor.html) por Genbyte
- Servidor oficial de Minecraft: [minecraft.net](https://www.minecraft.net/)

---

¡Disfruta de tu servidor de Minecraft en Docker! Si tienes alguna pregunta o sugerencia, no dudes en abrir un issue en este repositorio.