
### **Step 1: Download and Install VirtualBox**
1. Go to the [VirtualBox official website](https://www.virtualbox.org/).
2. Download the latest **VirtualBox** version for your operating system (Windows, macOS, or Linux).
3. Run the installer and follow the setup wizard to install VirtualBox.

---

### **Step 2: Download Ubuntu ISO**
1. Visit the [Ubuntu official website](https://ubuntu.com/download/desktop).
2. Download the latest **Ubuntu ISO** file.

---

### **Step 3: Create a New Virtual Machine**
1. Open **Oracle VirtualBox**.
2. Click on **"New"** to create a new virtual machine.
3. Enter a name (e.g., "Ubuntu VM").
4. Set **Type** to **Linux** and **Version** to **Ubuntu (64-bit)**.
5. Click **Next**.

---

### **Step 4: Configure Memory (RAM)**
1. Allocate at least **2GB (2048MB)** of RAM (recommended **4GB or more** for better performance).
2. Click **Next**.

---

### **Step 5: Create a Virtual Hard Disk**
1. Select **"Create a virtual hard disk now"** → Click **Create**.
2. Choose **VDI (VirtualBox Disk Image)** → Click **Next**.
3. Select **Dynamically allocated** (Recommended) or **Fixed size**.
4. Set at least **20GB** of disk space → Click **Create**.

---

### **Step 6: Load Ubuntu ISO and Start VM**
1. Select your **Ubuntu VM** in VirtualBox.
2. Click **Settings** → Go to **Storage**.
3. Under **Controller: IDE**, click **Empty**.
4. On the right side, click the **CD icon** → Choose **"Choose a disk file"**.
5. Select the **Ubuntu ISO** you downloaded.
6. Click **OK**.
7. Click **Start** to boot the VM.

---

### **Step 7: Install Ubuntu**
1. Once Ubuntu boots, click **"Install Ubuntu"**.
2. Follow the installation prompts:
   - Choose **Keyboard layout**.
   - Select **Normal Installation**.
   - Check **"Install third-party software"** (recommended).
   - Choose **Erase disk and install Ubuntu** (Safe for VMs).
3. Click **Install Now** → Follow the setup (username, password, etc.).
4. Once installed, restart the VM when prompted.
5. Remove the ISO by going to **Devices > Optical Drives > Remove disk from virtual drive**.

---

### **Step 8: Install VirtualBox Guest Additions (Optional but Recommended)**
1. Start your **Ubuntu VM**.
2. Click **Devices > Insert Guest Additions CD Image**.
3. Open **Terminal** and run:
   ```bash
   sudo apt update
   sudo apt install virtualbox-guest-utils
   ```
4. Restart the VM to enable **better performance, fullscreen mode, and clipboard sharing**.
