# Minecraft Server - Self-hosted Minecraft Server with Docker

This repository provides a simple Docker setup for running a **Minecraft Server**. It leverages the popular `itzg/minecraft-server` image, which allows you to quickly deploy a Minecraft server with customizable options.

## Prerequisites

Before using this repository, make sure you have the following installed:

- **Docker** (latest version)
- **Docker Compose** (optional, but recommended)

## Features

- Easily deploy a Minecraft server with Docker.
- Support for various Minecraft versions (configurable).
- Persistent storage for world data and server configuration.
- Configurable through environment variables and volume mounts.
- No need for additional setup - just run the container!

## Installation

Follow these steps to deploy your Minecraft server:

### 1. Clone the repository

First, clone this repository to your local machine:

```bash
git clone https://github.com/NotBeCursed/MinecraftServer.git
cd MinecraftServer
```

### 2. Modify the configuration

The `docker-compose.yml` file contains all the configuration you need to run the Minecraft server. Here are some environment variables you can configure:

- **EULA**: You must agree to Minecraft's End User License Agreement (EULA) by setting `EULA=TRUE`. This is required for the server to start.
- **VERSION**: Optionally, you can specify the version of Minecraft you want to run by uncommenting and setting the `VERSION` environment variable (e.g., `"1.17.1"`).

Example `docker-compose.yml` file:

```yaml
services:
  mc:
    container_name: minecraft-server
    hostname: mc-server
    image: itzg/minecraft-server
    tty: true
    stdin_open: true
    ports:
      - "25665:25565"
    environment:
      EULA: "TRUE"
      # VERSION: "X.X.X" # OPTIONAL
    volumes:
      - ./data:/data
```

In the `docker-compose.yml` file:
- The port `25665:25565` is mapped to allow access to your Minecraft server from your local network or externally (you can modify this if needed).
- The `./data:/data` volume ensures that your server data (worlds, configuration, etc.) persists even when the container is restarted or recreated.

### 3. Start the Minecraft Server

Once the configuration is complete, you can start your Minecraft server by running:

```bash
docker-compose up -d
```

Alternatively, you can run the server without Docker Compose by using the following `docker run` command:

```bash
docker run -d \
  -p 25665:25565 \
  -e EULA=TRUE \
  -v ./data:/data \
  --name minecraft-server \
  itzg/minecraft-server
```

### 4. Accessing the Server

Once the container is running, you can access your Minecraft server from the Minecraft client. Use the following IP and port to connect:

```
Server IP: localhost
Port: 25665
```

If you're hosting the server on a remote machine, use the machine's IP address (e.g., `192.168.1.100`) and the mapped port (`25665`).

### 5. Customizing the Server

You can customize your server by modifying the `./data` folder, which contains:

- Server configuration files (`server.properties`, `eula.txt`, etc.).
- World data (persistent game data like player progress, world maps).
- You can also change server settings by editing the `server.properties` file.

## Troubleshooting

If you encounter any issues, here are some helpful steps:

1. **Check container logs**:
   If the server fails to start or is experiencing issues, you can check the logs with the following command:

   ```bash
   docker logs minecraft-server
   ```

2. **Permissions issues**:
   Make sure that the `./data` folder has the correct file permissions, especially if you are running the server as a non-root user:

   ```bash
   sudo chown -R $USER:$USER ./data
   ```

3. **Server not starting**:
   If the server doesn't start, ensure that the `EULA` is set to `"TRUE"` in the environment variables. This is required by Mojang for the server to start.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
