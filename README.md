## Hardware Requirements
Requires a PC or GPU cloud server with Nvidia GPU and a minimum of the following hardware:
| GPU | RAM     | CPU                     | Disk                      |
| :-------- | :------- | :-------------------------------- | :-------------------------------- |
| `16 GB vRAM`      | `32 GB` | `16 Core` | `80-100 GB SSD` |


## Enviorement

## Method 1 - Windows Users (Home PC):
If you are a windows user, you may need to install `Ubuntu` on your windows.
* Install Ubuntu on Windows: [Guide](https://github.com/0xmoei/Install-Linux-on-Windows)
* After you installed `Ubuntu` on Windows, Verify you already have `NVIDIA Driver` & `CUDA Toolkit` ready:
```console
# Install NVIDIA Toolkit
sudo apt-get update
sudo apt-get install -y nvidia-cuda-toolkit

# Verify NVIDIA Driver
nvidia-smi

# Verify CUDA Toolkit:
nvcc --version
```

## Method 2 - Rent Cloud GPU:
### Step 1. Rent Vast.ai GPUs
1. Register in [Vast.ai](https://cloud.vast.ai/?ref_id=228875)
2. Create ssh key in your local system (if you don't have already)
*  Follow Step 1 of this [Guide](https://github.com/0xmoei/Rent-and-Config-GPU), then continue here.
*  Create an SSH key in Vast.ai by going to `three-lines > Keys > SSH Keys` [here](https://cloud.vast.ai/manage-keys/)
*  Paste SSH public key created in your local pc in step 2.

### Step 2. Create template with `49200` exposed port
Pluralis node needs `49200` port to be exposed to external connections
* Method 1: Use my customized templates with exposed ports: [Ubuntu VM](https://cloud.vast.ai/?ref_id=228875&creator_id=228875&name=Ubuntu%2022.04%20VM%20(Pluralis))
* Method 2: Manually customize template ports on Vast by following the below picture:

![Screenshot_858 copy](https://github.com/user-attachments/assets/c4de209d-6f35-488f-85bf-c87f5f7f3c8f)

### Step 3. Connect to GPU cloud terminal
1. Ensure you've chosen a GPU cloud with sufficient **GPU vRAM**, **CPU**, **RAM** and **Disk Space**
2. Top-up credits with crypto and rent it.
3. Go to [instances](https://cloud.vast.ai/instances/), refresh the page, click on `key` button.
4. If you don't see a ssh command in `Direct ssh connect:` section, then you have to press on Add SSH Key.
5. Enter the command in **Windows Powershell**, or any terminal client like **Mobaxtem**'s bash terminal and run it.
6. It prompts you for your ssh public key password (if you set before), then your GPU terminal appears.

