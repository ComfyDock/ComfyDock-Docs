# Installation

ComfyDock can be installed in two different ways: directly using the CLI tool or through the Pinokio app platform. Choose the method that works best for your workflow.

### **Prerequisites**

- A **Windows, Linux, or macOS** machine (macOS is CPU-only)
- **Docker**: ComfyDock requires Docker to be installed on your system
    - For desktop environments: [Docker Desktop](https://www.docker.com/products/docker-desktop/)  for Windows, macOS, or Linux
    - For headless environments: [Docker Engine](https://docs.docker.com/engine/install/) + [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)
- Latest **WSL** (Windows only).
- Latest **NVIDIA drivers**.
- Adequate disk space.

### **Installing Docker Desktop**

1. Download [Docker Desktop](https://www.docker.com/products/docker-desktop/). (AMD64 recommended)
2. Follow installation instructions for your operating system.
3. (Optional step: Windows) Ensure WSL is updated:
    - Open PowerShell as Administrator.
    - Run the following command:
        
        ```
        wsl --update
        ```
        
    - Verify Docker installation by running:
        
        ```
        wsl docker --version
        ```
    
    If successful, you'll see output like: `Docker version 27.3.1, build ce12230`.

## Method 1: CLI Installation

Check out the [GitHub repository](https://github.com/ComfyDock/ComfyDock-CLI) for the latest updates and issues!


### **Step 1: Install the CLI tool**

```bash
pip install comfydock
```
> **Note**: Requires Python 3.12 or higher.

---

### **Step 2: Configure ComfyDock**

```bash
# Set your local ComfyUI path (if you have one)
comfydock config comfyui_path /path/to/your/ComfyUI

# Or use interactive configuration mode
comfydock config
```

---

### **Step 3: Start ComfyDock**

```bash
comfydock up
```
This will start both the backend and frontend servers and open ComfyDock in your browser automatically.

---

### **Step 4: Stop ComfyDock**

```bash
comfydock down

or

ctrl + c in the terminal
```

---

### **Additional Commands**

```bash
# Show main help
comfydock --help

# Show help for a specific command
comfydock up --help

# List all current configuration values
comfydock config --list

# Start only the backend server without the frontend
comfydock up --backend
```

---

### **Updating ComfyDock CLI**

```bash
pip install --upgrade comfydock
```

---

## Method 2: Pinokio Installation

<!-- > **Installation Demo**  
> <video controls>
> <source src="../assets/installDemo_edit1.mp4" type="video/mp4">
> </video> -->


### **Step 1: Install the Pinokio App**

1. [Download Pinokio](https://program.pinokio.computer/#/?id=install)
2. Follow the installation instructions provided on the website.
3. After installation:
    - Open Pinokio and click **Discover** (top-right corner).
    - Select **Download from URL**.
    - Enter the following URL into the first field:<br/>`https://github.com/ComfyDock/ComfyDock-Pinokio`<br/>Leave the second field blank.
    - Click **One-Click Install with Pinokio**.
    - Go through the standard installation process.
    - You will be prompted to download Docker Desktop (see instructions above for installing docker), if docker is already installed you can continue.
    - Finally, click **Install** and set an appropriate name to save the application.

---

### **Step 2: Install ComfyDock**

1. Ensure Pinokio is running.
2. Click the **ComfyDock** app on the home screen.
    - You should see the following menu:
    - ![ComfyDock App](assets/pinokioMenu1.png)
3. Click **Install & Update** to begin setup.
4. Make sure Docker Desktop is running and click **Start**.
   - Look for `Uvicorn running on http://0.0.0.0:5172`.
5. Click **Show Environments** to view the main interface.
   - If empty, wait a bit and click **refresh**.

---