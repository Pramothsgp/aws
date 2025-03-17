Here's a **multi-stage Dockerfile** for your Spring Boot backend. It builds the application in the first stage and runs it in a lightweight container in the second stage.  

### **Dockerfile**  
```dockerfile
# Stage 1: Build
FROM maven:3.9.5-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### **Explanation**  
- **Stage 1 (Builder Stage)**:  
  - Uses `maven:3.9.5-eclipse-temurin-17` for Java 17 and Maven.  
  - Caches dependencies (`mvn dependency:go-offline`) for faster builds.  
  - Copies the project files and builds the JAR file.  
- **Stage 2 (Runtime Stage)**:  
  - Uses a lightweight `eclipse-temurin:17-jdk-alpine` image.  
  - Copies the JAR file from the build stage.  
  - Exposes port `8080` and starts the application with `java -jar app.jar`.  

### **How to Build & Run**  
```sh
docker build -t my-springboot-app .
docker run -p 8080:8080 my-springboot-app
```



### **Steps to Push Your Spring Boot Docker Image to Docker Hub**

#### **1️⃣ Build the Docker Image**
```sh
docker build -t <your-dockerhub-username>/my-springboot-app .
```
Example:
```sh
docker build -t pramothsgp/my-springboot-app .
```

---

#### **2️⃣ Log in to Docker Hub**
```sh
docker login
```
- Enter your **Docker Hub username** and **password** when prompted.

---

#### **3️⃣ Tag the Image (Optional)**
If you want to use a specific version instead of `latest`, tag it like this:
```sh
docker tag <your-dockerhub-username>/my-springboot-app <your-dockerhub-username>/my-springboot-app:v1
```
Example:
```sh
docker tag pramothsgp/my-springboot-app pramothsgp/my-springboot-app:v1
```

---

#### **4️⃣ Push the Image to Docker Hub**
```sh
docker push <your-dockerhub-username>/my-springboot-app
```
Example:
```sh
docker push pramothsgp/my-springboot-app
```

If you tagged a specific version (`v1`), push that version:
```sh
docker push pramothsgp/my-springboot-app:v1
```

---

#### **5️⃣ Pull & Run from Docker Hub (Test on Another Machine)**
On another machine (or after clearing local images), you can pull and run the image like this:
```sh
docker pull pramothsgp/my-springboot-app
docker run -p 8080:8080 pramothsgp/my-springboot-app
```

---

### **🚀 Summary of Commands**
```sh
docker build -t pramothsgp/my-springboot-app .
docker login
docker tag pramothsgp/my-springboot-app pramothsgp/my-springboot-app:v1
docker push pramothsgp/my-springboot-app
docker push pramothsgp/my-springboot-app:v1
docker pull pramothsgp/my-springboot-app
docker run -p 8080:8080 pramothsgp/my-springboot-app
```
