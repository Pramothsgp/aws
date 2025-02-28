### **Docker Compose Basics**  

Docker Compose is a tool for defining and managing multi-container Docker applications using a YAML file (`.yml` or `.yaml`). It allows you to configure services, networks, and volumes in a structured way.

### **Key Points**
- The `.yml` file is **indentation-based** and follows YAML syntax.
- It defines services, networks, and volumes declaratively.

### **Common Commands**
1. **Starting Containers in Detached Mode:**  
   ```bash
   docker compose up -d
   ```
   - `up` starts the containers based on the `docker-compose.yml` configuration.
   - `-d` runs the containers in the background (detached mode).

2. **Stopping and Removing Containers:**  
   ```bash
   docker compose down
   ```
   - Stops and removes all containers, networks, and volumes defined in `docker-compose.yml`.

3. **Scaling Services (e.g., Running Multiple Instances of a Service):**  
   ```bash
   docker compose up --scale frontend=3 -d
   ```
   - Scales the `frontend` service to **3 instances** and runs them in detached mode.

### **Additional Useful Commands**
- **Viewing Running Containers:**  
  ```bash
  docker compose ps
  ```
- **Restarting Services:**  
  ```bash
  docker compose restart
  ```
- **Viewing Logs:**  
  ```bash
  docker compose logs -f
  ```
  - `-f` follows the logs in real time.

Here are the commands along with their explanations:  

### **1. Add the EC2 user to the Docker group**  
```bash
sudo usermod -aG docker ec2-user
```
- Adds `ec2-user` to the `docker` group, allowing the user to run Docker commands without `sudo`.  
- The `-aG` flag appends the user to the group without removing existing groups.  

---

### **2. Download Docker Compose**  
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
```
- Downloads the latest version of **Docker Compose** for **Linux (x86_64 architecture)**.  
- `-L` follows redirects to get the latest release.  
- The file is saved as `/usr/local/bin/docker-compose` (a system-wide executable location).  

---

### **3. Make Docker Compose executable**  
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
- Grants execution permissions to the `docker-compose` binary, allowing it to be run as a command.  

---

### **4. Verify the Docker Compose installation**  
```bash
docker-compose version
```
- Checks if Docker Compose is installed correctly by displaying the installed version.  

---

### **5. Enable Docker to start on boot**  
```bash
sudo systemctl enable docker
```
- Ensures that the **Docker service** starts automatically whenever the system boots.  

### **6. Create a docker-compose.yml file**

- Write the yml content to execute it the example file added to the repo

``` bash
docker-compose up -d
```