
### 1. **Install Docker**  
If you haven't installed Docker yet, follow these steps for a typical Linux (CentOS/RHEL) installation:

#### 1.1 Install Docker

- **Update package index**:
  ```bash
  sudo yum update -y
  ```



- **Install Docker**:
  ```bash
  sudo yum install -y docker
  ```

#### 1.2 Start Docker and enable it to start on boot
- **Start Docker service**:
  ```bash
  sudo systemctl start docker
  ```

- **Enable Docker to start on boot**:
  ```bash
  sudo systemctl enable docker
  ```

- **Verify Docker is running**:
  ```bash
  sudo systemctl status docker
  ```

- **Verify Docker installation**:
  ```bash
  docker --version
  ```

### 2. **Run a Docker Container**  
For demonstration, let's pull and run an Ubuntu container:

- **Pull the Ubuntu image**:
  ```bash
  docker pull ubuntu
  ```

- **Run the Ubuntu container**:
  ```bash
  docker run -it --name ubuntu-container ubuntu /bin/bash
  ```

This will start the container in interactive mode and drop you into the Ubuntu shell.

### 3. **Create a Custom Bridge Network**  
Now, create a custom bridge network to connect multiple containers.

#### 3.1 Create a new network

- **Create a custom bridge network**:
  ```bash
  docker network create app_bridge
  ```

- **Verify the network is created**:
  ```bash
  docker network ls
  ```

#### 3.2 Inspect the network

- **Inspect the custom network**:
  ```bash
  docker network inspect app_bridge
  ```

The output will show you details about the network, including connected containers.

### 4. **Run Another Container and Connect to the Network**  
Now, let's run another container and connect both containers to the custom network (`app_bridge`).

#### 4.1 Run the second container

- **Run a second container** (let's call it `ubuntu-container-2`):
  ```bash
  docker run -it --name ubuntu-container-2 ubuntu /bin/bash
  ```

#### 4.2 Connect the containers to the custom network

- **Connect the first container to the network**:
  ```bash
  docker network connect app_bridge ubuntu-container
  ```

- **Connect the second container to the network**:
  ```bash
  docker network connect app_bridge ubuntu-container-2
  ```

### 5. **Verify the Containers are Connected to the Network**

Now, inspect the custom network again to confirm that both containers are connected to it.

- **Inspect the `app_bridge` network**:
  ```bash
  docker network inspect app_bridge
  ```

This should show a list of containers attached to the network under the `"Containers"` section.

### 6. **Inspect Containers' Logs**

If you're having issues with containers, you can check their logs for any errors.

- **View the logs for a container**:
  ```bash
  docker logs ubuntu-container
  docker logs ubuntu-container-2
  ```

This can provide helpful information in case the containers aren't behaving as expected.

### 7. **Stop and Remove Containers**  

When you're done, you can stop and remove the containers:

- **Stop containers**:
  ```bash
  docker stop ubuntu-container
  docker stop ubuntu-container-2
  ```

- **Remove containers**:
  ```bash
  docker rm ubuntu-container
  docker rm ubuntu-container-2
  ```

---

### **The `-dit` Flag Explanation**

When you run a Docker container using the `docker run` command, the `-dit` flag is commonly used to run the container in the background while still allowing interaction with it. Here's what each flag means:

1. **`-d`**: **Detached mode**
   - This option tells Docker to run the container in the background (detached mode) rather than in the foreground (interactive mode).
   - When you use `-d`, Docker runs the container in the background and immediately returns the container ID, so you can continue working in your terminal without being attached to the container's output.

2. **`-i`**: **Interactive mode**
   - This keeps the standard input (stdin) open even if not attached. It is typically used when you want to interact with the container, such as running a shell or executing commands inside the container.
   - It's often used in combination with `-t` to allow interactive terminal sessions.

3. **`-t`**: **Allocate a pseudo-TTY**
   - This option allocates a terminal (TTY) for the container, which is useful when you want to interact with the container's shell (e.g., running a bash shell).
   - This makes the container behave like it has a terminal, allowing you to see prompts and run commands interactively, making it more user-friendly when running interactive programs.

### Example Usage:
You can use the `-dit` flag together to run a container in the background with interactive access, like this:

```bash
docker run -dit --name my-container ubuntu /bin/bash
```

- **`-d`**: The container runs in detached mode (background).
- **`-i`**: Keeps the input open for interactive use.
- **`-t`**: Allocates a terminal for interactive use.

After running this, you can still interact with the container later by attaching to it or executing commands inside it:

```bash
docker exec -it my-container bash
```

This command opens a new interactive shell session inside the running container.

### Summary of Commands:

```bash
# 1. Install Docker
sudo yum update -y
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce
sudo systemctl start docker
sudo systemctl enable docker
docker --version

# 2. Run a Container
docker pull ubuntu
docker run -it --name ubuntu-container ubuntu /bin/bash

# 3. Create and Inspect Custom Network
docker network create app_bridge
docker network ls
docker network inspect app_bridge

# 4. Run Another Container and Connect to Network
docker run -it --name ubuntu-container-2 ubuntu /bin/bash
docker network connect app_bridge ubuntu-container
docker network connect app_bridge ubuntu-container-2

# 5. Inspect Network Connection
docker network inspect app_bridge

# 6. View Logs
docker logs ubuntu-container
docker logs ubuntu-container-2

# 7. Stop and Remove Containers
docker stop ubuntu-container
docker stop ubuntu-container-2
docker rm ubuntu-container
docker rm ubuntu-container-2
```

---

These steps guide you from installing Docker, running containers, creating and connecting a custom network, and understanding the `-dit` flag for 