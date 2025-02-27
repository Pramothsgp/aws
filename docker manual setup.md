If you want to **manually set up and run Docker containers** (without using `docker compose`), including **cloning code into the container**, follow these detailed steps:

---

# **🚀 Manual Docker Setup (Without Compose)**  
This guide will cover:
1. **Installing Docker**
2. **Cloning the Code from GitHub**
3. **Building a Docker Image**
4. **Creating a Docker Network**
5. **Running Containers Manually**
6. **Scaling Services (Multiple Instances)**
7. **Managing Containers (Stop, Remove, Logs, etc.)**
8. **Cleaning Up**

---

## **🛠 1. Install Docker (If Not Installed)**
Before proceeding, ensure Docker is installed on your system.  

🔹 **To install Docker:**  
- **Linux**: [Follow this guide](https://docs.docker.com/engine/install/)  
- **Windows/Mac**: Download from [Docker's official website](https://www.docker.com/get-started)

Check if Docker is installed:
```bash
docker --version
```

---

## **📂 2. Clone the Code into Your Local Machine**
```bash
git clone https://github.com/your-repo/project.git
cd project
```
Replace `https://github.com/your-repo/project.git` with your actual repository URL.

---

## **🐳 3. Create a `Dockerfile`**
In the **project folder**, create a `Dockerfile` (without extension) to define how to build the image.

**Example for a Node.js Backend:**
```dockerfile
# Use an official Node.js image
FROM node:18

# Set the working directory inside the container
WORKDIR /app

# Copy all files from the current directory to the container
COPY . .

# Install dependencies
RUN npm install

# Expose a port (e.g., 5000)
EXPOSE 5000

# Command to run when the container starts
CMD ["node", "server.js"]
```

---

## **🔨 4. Build a Docker Image**
Run the following command in the project directory (where `Dockerfile` is located):
```bash
docker build -t my-backend-image .
```
- `-t my-backend-image` → Names the image as **my-backend-image**  
- `.` → Means "use the Dockerfile in this directory"

---

## **🌐 5. Create a Docker Network**
To allow communication between different containers (backend, frontend, database), create a custom network:
```bash
docker network create my_network
```

---

## **🚀 6. Run Containers Manually**
Now, start each service manually.

### **🔹 Run the Backend Container**
```bash
docker run -d --name backend --network my_network -p 5000:5000 my-backend-image
```
- `-d` → Run in detached mode  
- `--name backend` → Assigns a name to the container  
- `--network my_network` → Connects the container to the custom network  
- `-p 5000:5000` → Maps port 5000 from the container to the host  

---

### **🔹 Run the Frontend Container**
If your frontend is a React app:
```dockerfile
# Frontend Dockerfile (Dockerfile-frontend)
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
EXPOSE 3000
CMD ["npx", "serve", "-s", "build"]
```
Then build and run it:
```bash
docker build -t my-frontend-image -f Dockerfile-frontend .
docker run -d --name frontend --network my_network -p 3000:3000 my-frontend-image
```

---

### **🔹 Run the Database (MongoDB)**
For MongoDB:
```bash
docker run -d --name database --network my_network -p 27017:27017 mongo
```
For MySQL:
```bash
docker run -d --name mysql-db --network my_network -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql
```

---

## **📌 7. Scale Services Manually**
To scale the frontend (e.g., 3 instances):
```bash
docker run -d --name frontend-1 --network my_network -p 3001:3000 my-frontend-image
docker run -d --name frontend-2 --network my_network -p 3002:3000 my-frontend-image
docker run -d --name frontend-3 --network my_network -p 3003:3000 my-frontend-image
```

---

## **📊 8. Manage Running Containers**
### **Check Running Containers**
```bash
docker ps
```

### **View Logs**
```bash
docker logs -f backend
```

### **Restart a Container**
```bash
docker restart backend
```

### **Stop Containers**
```bash
docker stop backend frontend database
```

### **Remove Containers**
```bash
docker rm backend frontend database
```

### **Remove Images**
```bash
docker rmi my-backend-image my-frontend-image
```

### **Remove Network**
```bash
docker network rm my_network
```

---

## **🧹 Clean Up Everything**
To remove **all** stopped containers, unused networks, and dangling images:
```bash
docker system prune -a
```

---

## **🎯 Final Summary**
| Step | Command |
|------|---------|
| Install Docker | `docker --version` |
| Clone Repository | `git clone <repo-url> && cd project` |
| Build Backend Image | `docker build -t my-backend-image .` |
| Build Frontend Image | `docker build -t my-frontend-image -f Dockerfile-frontend .` |
| Create Network | `docker network create my_network` |
| Run Backend | `docker run -d --name backend --network my_network -p 5000:5000 my-backend-image` |
| Run Frontend | `docker run -d --name frontend --network my_network -p 3000:3000 my-frontend-image` |
| Run Database | `docker run -d --name database --network my_network -p 27017:27017 mongo` |
| Scale Frontend | `docker run -d --name frontend-1 --network my_network -p 3001:3000 my-frontend-image` |
| Stop Containers | `docker stop backend frontend database` |
| Remove Containers | `docker rm backend frontend database` |
| Remove Images | `docker rmi my-backend-image my-frontend-image` |
| Remove Network | `docker network rm my_network` |
| Clean Up | `docker system prune -a` |

---

## **🚀 Why Not Just Use Docker Compose?**
Manually running Docker commands works, but Docker Compose simplifies it:
```bash
docker compose up -d
```
Instead of running multiple commands manually, **Docker Compose** does everything in one step.

Let me know if you need more details! 🚀🔥