# Troubleshooting

### Slow Model Loading

- If you have your model checkpoints currently saved on a windows drive (C:\, D:\, etc.), you may notice increased loading times when using the environment manager. This is due to the container running in WSL and needing to transfer data from Windows to Linux during read operations.
- You can greatly speed up model loading times by creating a ComfyUI installation inside of WSL (you can have the manager perform this automatically for you), and then moving or copying models from Windows to the WSL install location.
- You can navigate to a WSL location from Windows File Explorer by using the prefix path: `\\wsl.localhost\<distro name>\<path>\<to>\<directory>`
    - e.g. `\\wsl.localhost\Ubuntu\home\akatz\ComfyUI\models`
    - If you don’t know which distro you have installed, start by pasting `\\wsl.localhost` into your file explorer path bar and navigating using the GUI from there.

### Common Filepaths & Permissions Issues

When running Docker in WSL on Windows, you’re essentially working across two file systems:

- **Windows File System:** Uses familiar drive letters like `C:\` or `D:\`. Files and folders saved here can be accessed from standard Windows applications and file explorers.
- **WSL File System:** Runs within the Linux environment. File paths look like `/home/user/...` or `/mnt/c/...` (for accessing Windows files from within WSL).

**Key Differences:**

- WSL paths are case-sensitive (`/ComfyUI` is different from `/comfyui`).
- File access between Windows and WSL can sometimes be slower due to file system translation.

### **How to Find WSL Filepaths from Windows**

You can access WSL files directly from Windows Explorer:

1. Open File Explorer.
2. Enter the following path in the address bar:
    
    ```
    \\wsl.localhost\<distro-name>\<path-to-directory>
    ```
    
    Example: `\\wsl.localhost\Ubuntu\home\akatz\ComfyUI\models`
    
3. Replace `<distro-name>` with the name of your installed WSL distribution (e.g., `Ubuntu` or `Debian`).
4. Navigate through the folders as needed.

---

### **Common Issues and Fixes**

### **1. Permission Denied Errors**

**Problem:** You see an error like `Permission denied` when trying to access or modify a file or folder inside the container or WSL.

**Solution:**

1. **Check Ownership and Permissions:**
    - Open a WSL terminal.
    - Navigate to the problematic folder using `cd /path/to/folder`.
    - Run:
        
        ```
        ls -l
        ```
        
        This shows the owner and permissions of files in the directory.
        
    - If the owner is not your user (e.g., `root`), change ownership with:
        
        ```
        sudo chown -R $USER:$USER /path/to/folder
        ```
        
2. **Ensure Write Permissions:**
    - To grant write access, run:
        
        ```
        chmod -R u+w /path/to/folder
        ```
        

### **2. Files Not Showing Up in Containers**

**Problem:** Files or directories mounted from the host are missing inside the container.

**Solution:**

- **Check Mount Configurations:** Ensure the directory you’re trying to mount is correctly specified in the environment’s mount settings.
- **Verify File Paths:** Ensure the specified path exists on your host machine and is accessible.
- **Restart the Environment:** Sometimes, restarting the Docker container resolves mounting issues.

### **3. Slow File Access or Model Loading**

**Problem:** Files stored on your Windows file system load slowly in WSL or the container.

**Solution:**

- **Move Files to WSL:**
    - Copy the files to your WSL installation directory for faster access. For example:
        
        ```
        cp /mnt/c/Users/akatz/ComfyUI/models /home/akatz/ComfyUI/models
        ```
        
    - Update your environment’s settings to point to the new WSL location.
- **Use WSL’s File Explorer Path:** Use `\\wsl.localhost` as described above for direct access.

### **4. Unable to Locate WSL Distro**

**Problem:** You can’t find your WSL installation or files in File Explorer.

**Solution:**

1. Open a terminal and list available WSL distros:
    
    ```
    wsl -l
    ```
    
    This will display a list of installed distros (e.g., `Ubuntu`, `Debian`).
    
2. Ensure the distro is running:
    
    ```
    wsl -d <distro-name>
    ```
    
3. Use the correct path in File Explorer:
    
    ```
    \\wsl.localhost\<distro-name>\<path-to-directory>
    ```
    

### **5. Files Saved in WSL Are Missing in Windows**

**Problem:** You saved files in WSL, but they’re not visible from Windows.

**Solution:**

- **Use Windows Explorer:** Access the WSL directory using the `\\wsl.localhost` path.
- **Copy Files to Windows:**
    - Use the `cp` command in WSL to copy files to a Windows-accessible directory:
        
        ```
        cp /home/akatz/ComfyUI/output /mnt/c/Users/akatz/Documents/ComfyUI_Output
        ```
        

---

### **Tips for Avoiding Path and Permissions Issues**

1. **Use Consistent File Naming:** Stick to lowercase letters and avoid spaces or special characters in filenames to prevent case sensitivity and parsing errors.
2. **Verify Mount Points:** Double-check mount configurations when creating or updating environments to ensure the correct paths are specified.
3. **Run Commands as Your User:** Avoid using `sudo` unless necessary, as it may cause files to be owned by `root`, leading to permission issues.
4. **Keep WSL Updated:** Ensure WSL and Docker are up-to-date to minimize compatibility issues.

### Default+ Mode Fails To Install All Custom Nodes

- Due to environmental and OS differences, using Default+ mode when creating a new ComfyUI environment may not always correctly install all custom nodes in an existing install location.
- My current recommendation is to instead try creating a new Default environment, load up the workflows that you normally use, and re-install missing custom nodes through the manager. Once you have a solid working environment, you can clone it whenever making updates or installing new nodes, and if this breaks things you can always delete & clone another from the original environment.

### Adding More RAM and Processors to WSL

- If you find that ComfyUI is not detecting all of the available RAM on your system, you likely need to increase the amount of RAM available to WSL.
- Here’s a short guide on how to do so:

[
Guide: How to Grant WSL More RAM or Processors by Editing `.wslconfig`](https://www.notion.so/Guide-How-to-Grant-WSL-More-RAM-or-Processors-by-Editing-wslconfig-161fd5b1ca3b808d9675cfd0e97407d9?pvs=21)

### Installing Custom Nodes Manually

- You can manually install custom nodes using “git clone” by using the “exec” feature of docker.
- Click the settings button on the environment you’d like to install nodes in, and find the “container name” in the About section.
- In docker desktop find a running container with the matching container name.
    
    ![Screenshot 2024-12-07 211217.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/c86b2078-a63b-4eef-9630-50caa0191921/Screenshot_2024-12-07_211217.png)
    
- After clicking on the container in docker desktop, navigate to the “exec” tab:
    
    ![Screenshot 2024-12-07 211359.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/94bb7d1a-5511-4526-893d-e91f30c7cb57/Screenshot_2024-12-07_211359.png)
    
- In the terminal you can run commands that will execute within the running container.
- Navigate to custom nodes directory by inputting the command:

```bash
cd /app/ComfyUI/custom_nodes
```

- Then run the following commands, replacing <custom_node_repo> with the URL of the github custom node repo you’d like to install (e.g. https://github.com/akatz-ai/ComfyUI-AKatz-Nodes):

```bash
 git clone <custom_node_repo>
 cd <custom_node_directory>/
 pip install -r requirements.txt
```

### **Editing Files Inside the Container with VSCode**

Sometimes you need to modify or update files that live solely inside the environment and aren’t mounted from your host machine (meaning you can’t just edit them from your usual file explorer).

**How to Do This Using VSCode:**

1. **Install the “Dev Containers” Extension:**
    
    In VSCode, open the Extensions panel and search for “Dev Containers”. Install it.
    
2. **Attach to the Running Container:**
    
    Make sure your environment’s container is running. In VSCode, press **F1** (or open the Command Palette) and type “Dev Containers: Attach to Running Container” and select it from menu:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/cc3bf05f-63f3-4e8d-bc10-4c018744811b/image.png)
    
    Select the container you want to work with: (DON’T choose /comfy-env-frontend)
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/4f270f92-4069-4f2e-abb1-a2ee4ef1f1ce/image.png)
    
3. **Open & Edit Files:**
    
    Once connected, VSCode will show the container’s file system. Open the “File” menu from the topbar and click “Open Folder”:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/6690389b-ef49-4fb0-b5d1-093a8155db1a/image.png)
    
    Now you can navigate to the location of ComfyUI saved in the container by typing “/app/ComfyUI/ in the provided field:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/1b6ff2f9-630a-4e20-8538-c8f62dbe9d7c/image.png)
    
     Click “OK”. Now in the left sidebar you should see the contents of your ComfyUI environment, and can browse folders, open files, and edit them just like you would locally. You can also drag and drop files from your machine directly into folders in the sidebar, which will upload them to the container. Any changes you make here will apply and be saved directly inside the container.
    
4. Open a Terminal to access the running python environment:
    
    You can perform actions such as “git clone” and “uv pip install” inside of the container environment by opening a new terminal window after connecting:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/6b9c91c2-e20b-4724-ada6-c9c525082f46/image.png)
    
    You should see this terminal open at the bottom of the VSCode window:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/87a864cf-2c82-438c-95eb-7f43c8164838/67040650-3912-4d97-963a-b2d2e5fecf5a/image.png)
    
    Now you can perform all your required installation actions just like you would on a local environment.
    

**Why Do This?**

- Perfect for tweaking custom nodes, environment configs, or any code that isn’t easily accessible from your host machine.
- Great for development and troubleshooting without having to rebuild or re-mount anything.

### Developing Custom Nodes In Containers

- Use the same process as above to access custom nodes saved inside of running containers.
- I recommend installing the “HotReloadHack” custom node when developing with ComfyUI to help speed up development: https://github.com/logtd/ComfyUI-HotReloadHack

### Unknown runtime specified nvidia

- docker: Error response from daemon: Unknown runtime specified nvidia.:
    - need to install nvidia-container-toolkit in linux:
    - https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html