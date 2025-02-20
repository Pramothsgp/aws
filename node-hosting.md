To host a Vite React application with React Router on AWS EC2 using Apache HTTPD, follow these steps:

### Prerequisites:
1. **AWS EC2 instance** running a Linux-based OS (e.g., Amazon Linux 2, Ubuntu).
2. **Node.js** installed on your EC2 instance.
3. **Vite React Application** ready to be deployed.
4. **Apache HTTPD** (httpd) installed on the EC2 instance.
5. **Security Group** allowing HTTP (port 80) and HTTPS (port 443) access.

### Steps:

#### 1. **Set up your EC2 instance**  
- Launch an EC2 instance (preferably Amazon Linux 2 or Ubuntu).
- Ensure that the security group allows traffic on ports `80` (HTTP) and `443` (HTTPS).
- SSH into the EC2 instance.

#### 2. **Install Apache HTTPD**  
If Apache HTTPD is not already installed, install it using the following commands based on your distribution:

- **For Amazon Linux 2 / CentOS:**
  ```bash
  sudo yum update -y
  sudo yum install httpd -y
  sudo systemctl start httpd
  sudo systemctl enable httpd
  ```

- **For Ubuntu/Debian:**
  ```bash
  sudo apt update
  sudo apt install apache2 -y
  sudo systemctl start apache2
  sudo systemctl enable apache2
  ```

#### 3. **Install Node.js and npm (if not installed)**  
If Node.js is not already installed, you can install it via:

```bash
# For Amazon Linux 2
curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
sudo yum install -y nodejs

# For Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verify the installation:
```bash
node -v
npm -v
```

#### 4. **Clone or upload your Vite React Application**  
- If your project is on GitHub, clone it:
  ```bash
  git clone https://github.com/your-username/your-vite-react-app.git
  cd your-vite-react-app
  ```

- Alternatively, upload your project files using `scp`, `FTP`, or another method.

#### 5. **Build your Vite React Application**  
Navigate to your project folder and build the production version of the app:
```bash
npm install
npm run build
```

This will generate a `dist` folder containing the production-ready files.

#### 6. **Configure Apache to Serve Your Vite App**  
- Move the contents of the `dist` folder to the Apache web directory:
  ```bash
  sudo cp -r dist/* /var/www/html/
  ```

- Set the appropriate permissions for Apache to access the files:
  ```bash
  sudo chown -R apache:apache /var/www/html/
  ```


- **Confirm Directory Permissions:** Sometimes Apache might not apply `.htaccess` files due to directory permissions or configuration settings. Make sure your files are readable by Apache:

- If you are using **React Router** (with client-side routing), ensure that Apache serves all routes correctly by redirecting them to `index.html`. To do this, create or edit the `.htaccess` file in `/var/www/html/`:
    ``` bash []
    sudo chmod -R 755 /var/www/html/
    ```
  ```bash
  sudo nano /var/www/html/.htaccess
  ```

  Add the following content to handle React Router properly:
  ```apache []
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /

    # Serve index.html for all requests
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.html [QSA,L]

  </IfModule>
  ```

### AllowOverride(httpd : amazon linux):

1. **Edit Apache Configuration File**:
   Open the Apache configuration file for editing:
   ```bash
   #httpd Amazon Linux
   sudo nano /etc/httpd/conf/httpd.conf
   ```

2. **Change the `AllowOverride` Directive**:
   Find the following block:

   ```apache
   <Directory "/var/www/html">
       AllowOverride None
       Require all granted
   </Directory>
   ```

   Change `AllowOverride None` to `AllowOverride All` so that it looks like:

   ```apache
   <Directory "/var/www/html">
       AllowOverride All
       Require all granted
   </Directory>
   ```
This should allow Apache to read `.htaccess` files, which will enable the URL rewrites needed for React Router.

### AllowOverride(apache2 : Ubuntu):

Make sure that the `mod_rewrite` module is enabled in Apache:
``` bash 
sudo a2enmod rewrite
sudo systemctl restart apache2
```
### Check apache log 
To check for syntax errors in Apache's configuration files, you can use the following command:

1. **Check the syntax of Apache configuration files:**

```bash
sudo apachectl configtest
```

This will check the syntax of the Apache configuration files. If the configuration is correct, you will see:

```
Syntax OK
```

If there are any errors, the output will indicate where the issue is.

2. **Check Apache error logs:**

If the syntax is OK but the issue persists, check Apache's error logs to find more information about any issues:

```bash
sudo tail -f /var/log/httpd/error_log
```

This will show the latest errors or warnings in real-time. You can use this to debug any runtime issues Apache might be having when serving your app.

Let me know if you see any issues with the output from these commands!


#### 7. **Restart Apache**  
After the setup, restart Apache to apply the changes:
```bash
# For Amazon Linux 2 / CentOS
sudo systemctl restart httpd

# For Ubuntu/Debian
sudo systemctl restart apache2
```

#### 8. **Verify the Deployment**  
Open your browser and navigate to your EC2 public IP or domain name:
```
http://your-ec2-public-ip
```

You should see your Vite React application live, and React Router should handle client-side routing correctly.

#### 9. **(Optional) Set Up SSL with Let's Encrypt (HTTPS)**  
To secure your website with HTTPS, you can use Let's Encrypt:

- Install Certbot:
  ```bash
  # For Amazon Linux 2 / CentOS
  sudo yum install -y certbot python3-certbot-apache

  # For Ubuntu/Debian
  sudo apt install certbot python3-certbot-apache
  ```

- Request an SSL certificate:
  ```bash
  sudo certbot --apache
  ```

- Follow the prompts to configure SSL, and Certbot will automatically update your Apache configuration for HTTPS.

#### 10. **(Optional) Set Up a Custom Domain**  
If you have a domain, you can point it to your EC2 instance's public IP by configuring an A record in your domain's DNS settings. Once it's set up, your site will be accessible using your custom domain.

### Troubleshooting:
- If you can't access your site, ensure that the security group of your EC2 instance allows inbound traffic on port 80 (HTTP) and 443 (HTTPS).
- Check Apache logs for any errors:
  ```bash
  sudo tail -f /var/log/httpd/error_log  # Amazon Linux / CentOS
  sudo tail -f /var/log/apache2/error.log  # Ubuntu / Debian
  ```

By following these steps, your Vite React app should be up and running on your AWS EC2 instance with Apache HTTPD!



# Node app Setup Instructions


This providefile s step-by-step instructions to set up the **Node application**, including both the backend and frontend, on an EC2 instance. Follow these steps carefully to ensure everything is configured correctly.

---

## Backend Setup

### Switch to Superuser Mode
```bash
sudo su
```

### Install Required Software
```bash
sudo yum install -y nodejs
sudo yum install -y httpd
sudo yum install -y git
```

### Verify Installations
```bash
npm -v      # Check Node.js version
node -v     # Check npm version
git --version  # Check Git version
```

### Clone the Repository
```bash
cd /tmp
git clone https://github.com/repo-url
```

### Move Backend Code to Home Directory
```bash
cd repo-name
mv backend /home/ec2-user
```

### Install Dependencies and Run the Backend
```bash
cd /home/ec2-user/backend/
npm install
node index.js
```

### Test the Backend
Access the backend API at: [https://checkapi.app/](https://checkapi.app/)

---

## Frontend Setup

### Switch to Superuser Mode
```bash
sudo su
```

### Navigate to Frontend Code Directory
```bash
cd /tmp/repo-name/frontend/
```

### Edit the Axios Base URL
- Open the appropriate file containing the Axios configuration.
- Replace the placeholder URL with your instance's public IP address.

Example:
```javascript
axios.defaults.baseURL = 'http://<YOUR_INSTANCE_IP>/api';
```

### Install Dependencies
```bashcom
npm install
npm install react-scripts
```

### Build the Frontend
```bash
npm run build
```

### Deploy the Build to Apache
```bash
cp -r build/* /var/www/html/
```

### Configure Apache for Proxy
Edit the Apache configuration file:
```bash
nano /etc/httpd/conf/httpd.conf
```
## Add the following configuration at the end of the file: (Optional)
```apache
<VirtualHost *:80>
    ServerName <YOUR_INSTANCE_PUBLIC_IP>

    ProxyPass /api http://localhost:3001/api
    ProxyPassReverse /api http://localhost:3001/api
</VirtualHost>
```
Replace `<YOUR_INSTANCE_IP>` with your instance's public IP address.

### Restart Apache
```bash
sudo systemctl restart httpd
service httpd restart
```

---

## Notes
- Ensure all dependencies are installed and up-to-date.
- Confirm that your instance's security group allows inbound traffic on port 80.
- After setup, access the application through your instance's public IP address in a browser.
