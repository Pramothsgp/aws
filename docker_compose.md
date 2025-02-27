### **Docker Compose Basics (Without `-`)**  

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
