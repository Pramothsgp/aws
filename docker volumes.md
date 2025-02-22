# Docker Volumes

The purpose of creating docker volume is to create a persisteance storage and to extend the volume of a container

### Types of docker volumes
- **Named volumes**
- **Anonymous volumes**
- **Bind mounts**

## Steps to create a named Volume

- Create a volume and provide a custom name for it
- Launch a container and attach the created volume
- Inspect the volume to verify the mount points

### Properties
- Named volumes are created and maintained by docker
- It is possible to connect a single volume to multiple containers
- mount points are like a directry path to access the volume


#### Mount points for volumes
 ``` bash []
    /var/lib/docker/volume
 ```


## Commands

### 1. **Create a Docker Volume**
```bash
docker volume create <volume name>
```
- This command creates a new named volume. Volumes in Docker are used to persist data outside the container's filesystem, so even if the container is removed or recreated, the data remains intact.
- Example: `docker volume create my-volume` creates a volume named `my-volume`.

### 2. **List Docker Volumes**
```bash
docker volume ls
```
- This command lists all the volumes that have been created on your Docker host. It will show the volume names along with their driver (typically `local`).
- Example: `docker volume ls` might output something like:
  ```
  DRIVER    VOLUME NAME
  local     my-volume
  ```

### 3. **Run a Docker Container with a Volume**
```bash
docker run -d --name <container name> -v <volume name>:<container path> <image name>
```
- container with multiple volume

``` bash
docker run -d --name <container name> -v <volume name>:<container path> -v<volume2 name>:<container path> <image name>
```
- This command runs a container in detached mode (`-d`) and mounts a Docker volume (`-v`).
- `<volume name>` is the volume created earlier (e.g., `my-volume`).
- `<container path>` is the path where the volume will be mounted inside the container (e.g., `/app`).
- `<image name>` is the Docker image you want to run (e.g., `nginx`).
- Example: `docker run -d --name my-container -v my-volume:/app nginx` runs an Nginx container and mounts `my-volume` to the `/app` directory inside the container.

### 4. **Inspect a Docker Volume**
```bash
docker volume inspect <volume name>
```
- This command provides detailed information about a volume, including its mount point, driver, and other metadata.
- Example: `docker volume inspect my-volume` might output:
  ```json
  [
    {
      "CreatedAt": "2025-02-22T12:34:56Z",
      "Driver": "local",
      "Labels": {},
      "Mountpoint": "/var/lib/docker/volumes/my-volume/_data",
      "Name": "my-volume",
      "Options": {},
      "Scope": "local"
    }
  ]
  ```

### 5. **Inspect a Docker Container**
```bash
docker inspect <container name>
```
- This command provides detailed information about a container, including its configuration, volumes, network settings, and more.
- Example: `docker inspect my-container` will return a JSON object with comprehensive details about the container.
  ```json
  [
    {
      "Id": "d9b100f2f636",
      "Created": "2025-02-22T12:34:56Z",
      "Path": "/bin/bash",
      "Args": [],
      "State": {
        "Status": "running",
        "Running": true,
        ...
      },
      "Mounts": [
        {
          "Type": "volume",
          "Name": "my-volume",
          "Source": "/var/lib/docker/volumes/my-volume/_data",
          "Destination": "/app",
          ...
        }
      ]
    }
  ]
  ```

### Key Takeaways:
- **Docker Volumes** are persistent storage that can be used across containers.
- **`docker volume ls`** lists all the volumes created.
- **`docker volume create`** creates a new volume.
- **`docker run -v`** mounts a volume inside the container to a specific directory.
- **`docker volume inspect`** provides detailed information about a volume.
- **`docker inspect`** provides detailed information about a container, including its volumes.
