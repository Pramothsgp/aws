
## **🟢 Starting & Managing Applications**
### 1️⃣ **Start an Application**
```sh
pm2 start app.js
```
or with `npm start`:
```sh
pm2 start npm --name myapp -- start
```
- Runs `app.js` in the background.
- Names the process `myapp`.

---

### 2️⃣ **Start an Application with Arguments**
```sh
pm2 start app.js -- --port=5000
```
- Passes `--port=5000` as an argument.

---

### 3️⃣ **Start an Application with Environment Variables**
```sh
pm2 start app.js --env production
```
- Runs in **production mode**.

---

### 4️⃣ **Start a Clustered App (Multi-core Mode)**
```sh
pm2 start app.js -i max
```
- Uses **all CPU cores** for better performance.

---

### 5️⃣ **Stop an Application**
```sh
pm2 stop myapp
```
or by process ID:
```sh
pm2 stop 0
```

---

### 6️⃣ **Restart an Application**
```sh
pm2 restart myapp
```
or
```sh
pm2 restart 0
```
- Useful when code changes need to be applied.

---

### 7️⃣ **Delete an Application**
```sh
pm2 delete myapp
```
or
```sh
pm2 delete 0
```
- Completely removes it from PM2.

---

## **🟡 Monitoring & Logs**
### 8️⃣ **View Running Processes**
```sh
pm2 list
```
- Shows all running apps.

---

### 9️⃣ **View Real-time Logs**
```sh
pm2 logs
```
or for a specific app:
```sh
pm2 logs myapp
```

---

### 🔟 **View Last 100 Log Entries**
```sh
pm2 logs --lines 100
```

---

### 1️⃣1️⃣ **Monitor CPU & Memory Usage**
```sh
pm2 monit
```
- Shows **real-time** CPU & RAM usage.

---

## **🔵 Auto-Restart & Startup Configuration**
### 1️⃣2️⃣ **Enable Auto-Restart on Reboot**
```sh
pm2 startup
```
It will show a command like:
```sh
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u ec2-user --hp /home/ec2-user
```
- Run this command to **enable auto-restart** on system reboots.

---

### 1️⃣3️⃣ **Save the Process List**
```sh
pm2 save
```
- Saves running apps so they restart after reboot.

---

### 1️⃣4️⃣ **Reload All Applications Without Downtime**
```sh
pm2 reload all
```
- Useful for **zero-downtime deployment**.

---

### 1️⃣5️⃣ **Unregister Auto-Restart**
```sh
pm2 unstartup
```
- Removes PM2 from system startup.

---

## **🟣 Process Management & Debugging**
### 1️⃣6️⃣ **Show Details About an App**
```sh
pm2 show myapp
```
- Displays uptime, memory usage, and more.

---

### 1️⃣7️⃣ **Kill PM2 Daemon**
```sh
pm2 kill
```
- Stops **all** PM2 processes.

---

### 1️⃣8️⃣ **Flush Logs (Clear Old Logs)**
```sh
pm2 flush
```
- Clears all logs.

---

### 1️⃣9️⃣ **Delete All Applications**
```sh
pm2 delete all
```
- Removes **all** apps from PM2.

---

## **🟠 Load Balancing & Advanced Usage**
### 2️⃣0️⃣ **Run App in Cluster Mode (Multi-core Support)**
```sh
pm2 start app.js -i 4
```
- Runs **4 instances** of the app.

---

### 2️⃣1️⃣ **Restart an App on File Change (Like `nodemon`)**
```sh
pm2 start app.js --watch
```

---

### 2️⃣2️⃣ **Set Memory Limit for an App**
```sh
pm2 start app.js --max-memory-restart 300M
```
- Restarts the app if memory exceeds **300MB**.

---

### 2️⃣3️⃣ **Start a Cron Job with PM2**
```sh
pm2 start app.js --cron "0 */6 * * *"
```
- Runs the app **every 6 hours**.

---

### 2️⃣4️⃣ **Start an App as a Service**
```sh
pm2 start app.js --name myservice
pm2 save
pm2 startup
```
- Keeps it running **forever**.

---

## **🟢 PM2 Ecosystem (Multiple Apps)**
### 2️⃣5️⃣ **Create `ecosystem.config.js` for Multiple Apps**
```sh
pm2 init
```
- Generates a **configuration file**.

Example `ecosystem.config.js`:
```js
module.exports = {
  apps: [
    {
      name: "myapp",
      script: "app.js",
      instances: "max",
      env: {
        NODE_ENV: "production",
        PORT: 5000,
      },
    },
  ],
};
```
### 2️⃣6️⃣ **Start All Apps in `ecosystem.config.js`**
```sh
pm2 start ecosystem.config.js
```

---

### 2️⃣7️⃣ **Restart Apps from `ecosystem.config.js`**
```sh
pm2 restart ecosystem.config.js
```

---

### 2️⃣8️⃣ **Stop All Apps in `ecosystem.config.js`**
```sh
pm2 stop ecosystem.config.js
```

---

## **🟡 Export & Import Process List**
### 2️⃣9️⃣ **Export Running Processes**
```sh
pm2 save
```
- Saves all running apps.

### 3️⃣0️⃣ **Restore Saved Processes**
```sh
pm2 resurrect
```
- Restores previously running apps.

---

## **🔴 PM2 Web Interface (Monitor from Browser)**
### 3️⃣1️⃣ **Install & Start PM2 Web Dashboard**
```sh
pm2 install pm2-web
pm2-web
```
- Access it via **`http://your-ec2-ip:8080`**.

---

## **🟢 Summary of Most Used Commands**
| Command | Description |
|---------|-------------|
| `pm2 start app.js` | Start an application |
| `pm2 start npm -- start` | Start an app using `npm start` |
| `pm2 list` | Show all running processes |
| `pm2 logs` | View logs |
| `pm2 restart myapp` | Restart a specific app |
| `pm2 stop myapp` | Stop an app |
| `pm2 delete myapp` | Remove an app |
| `pm2 save` | Save the running apps for auto-restart |
| `pm2 startup` | Auto-start PM2 on reboot |
| `pm2 monit` | Real-time monitoring |

---

## **💡 Best Practices for PM2 on AWS EC2**
1️⃣ **Always use `pm2 save`** after making changes.  
2️⃣ **Ensure PM2 auto-restarts apps on reboot** using:  
   ```sh
   pm2 startup
   ```  
3️⃣ **Use `pm2 logs`** to debug any issues.  
4️⃣ **Use `pm2 reload all`** for zero-downtime deployment.  

---

Now you have a complete **PM2 cheat sheet**! 🚀 Let me know if you need more details. 😃