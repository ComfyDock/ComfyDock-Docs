# Common Issues

### Slow Model Loading (Windows Users)

- If you have your model checkpoints currently saved on a windows drive (C:\, D:\, etc.), you may notice increased loading times when using the environment manager. This is due to the container running in WSL and needing to transfer data from Windows to Linux during read operations.
- You can greatly speed up model loading times by creating a ComfyUI installation inside of WSL (you can have the manager perform this [automatically for you](/usage/#creating-a-new-environment) during environment creation), and then moving or copying models from Windows to the WSL install location.
- You can navigate to a WSL location from Windows File Explorer by using the prefix path: `\\wsl.localhost\<distro name>\<path>\<to>\<directory>`
    - e.g. `\\wsl.localhost\Ubuntu\home\akatz\ComfyUI\models`
    - If you don’t know which distro you have installed, start by pasting `\\wsl.localhost` into your file explorer path bar and navigating using the GUI from there.

### Default+ Mode Fails To Install All Custom Nodes

- Due to environmental and OS differences, using Default+ mode when creating a new ComfyUI environment may not always correctly install all custom nodes in an existing install location.
- My current recommendation is to instead try creating a new Default environment, load up the workflows that you normally use, and re-install missing custom nodes through the manager. Once you have a solid working environment, you can clone it whenever making updates or installing new nodes, and if this breaks things you can always delete & clone another from the original environment.

### Developing Custom Nodes In Containers

- I recommend installing the [“HotReloadHack”](https://github.com/logtd/ComfyUI-HotReloadHack) custom node when developing with ComfyUI to help speed up development

### Unknown runtime specified nvidia

- docker: Error response from daemon: Unknown runtime specified nvidia.:
    - need to install nvidia-container-toolkit in linux:
    - [nvidia-container-toolkit install guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)