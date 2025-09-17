<img width="3216" height="907" alt="image" src="https://github.com/user-attachments/assets/3d5ec09c-88c2-4fa8-8425-cc46489eed42" />

# Pluralis: Node0 event
Node0 is a public training event powered by Protocol Learning's decentralized AI approach. You can join with a 16GB GPU like a 3090 and help train a massive model using your own device.

**Each participant’s contribution is tracking in the dashboard.**

---

## Hardware Requirements
Requires a PC or GPU cloud server with Nvidia GPU and a minimum of the following hardware:
| GPU | RAM     | CPU                     | Disk                      | Exposed port                      |
| :-------- | :------- | :-------------------------------- | :-------------------------------- | :-------------------------------- |
| `16 GB vRAM`      | `32 GB` | `16 Core` | `80-100 GB SSD` | `49200` |

---

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
* Method 1: Use my customized templates with exposed ports:
  * [Ubuntu VM](https://cloud.vast.ai/?ref_id=228875&creator_id=228875&name=Ubuntu%2022.04%20VM%20(Pluralis)) (Compatible with both node's `docker` or `from source` installation)
  * [NVIDIA CUDA](https://cloud.vast.ai/?ref_id=228875&creator_id=228875&name=NVIDIA%20CUDA%20(Pluralis)) (Compatible only with node's `from source` installation)

* Method 2: Manually customize template ports on Vast by following the below picture:

![Screenshot_858 copy](https://github.com/user-attachments/assets/c4de209d-6f35-488f-85bf-c87f5f7f3c8f)

### Step 3. Connect to GPU cloud terminal
1. Ensure you've chosen a GPU cloud with sufficient **GPU vRAM**, **CPU**, **RAM** and **Disk Space**
2. Top-up credits with crypto and rent it.
3. Go to [instances](https://cloud.vast.ai/instances/), refresh the page, click on `key` button.
4. If you don't see a ssh command in `Direct ssh connect:` section, then you have to press on Add SSH Key.
5. Enter the command in **Windows Powershell**, or any terminal client like **Mobaxtem**'s bash terminal and run it.
6. It prompts you for your ssh public key password (if you set before), then your GPU terminal appears and ready for executing next commands.

--

## Find exposed ports
**1. Click on instance IP**

<img width="901" height="146" alt="image" src="https://github.com/user-attachments/assets/0e2f32ab-863c-456f-9a2d-c121a73c69d8" />

**2. Write down  Announce port**
* In below example, `49200`=host port, `56353`=announce port. You need them in **step 5** of [Install and run Pluralis node](#install-and-run-pluralis-node)

<img width="268" height="129" alt="image" src="https://github.com/user-attachments/assets/640435e2-36de-420f-972d-d1e0443960d7" />

---

## Get HuggingFace Access token
**1- Create account in [HuggingFace](https://huggingface.co/)**

**2- Create an Access Token with no specific permissions [here](https://huggingface.co/settings/tokens) and write it down for next steps**

---

## Dependecies
Note: Some GPU clouds may not support `sudo` in commands, so you have to omit it.

**1. Update System Packages**
```bash
sudo apt update && sudo apt upgrade -y
```
**2. Install General Utilities and Tools**
```bash
sudo apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

**3. Install Python and pip**
```bash
sudo apt install -y python3-pip
sudo apt install pip
sudo apt install -y build-essential libssl-dev libffi-dev python3-dev
```

**4. Install Conda**
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```
```
source ~/miniconda3/bin/activate
```
```
conda init --all
```

---

## Install and run Pluralis Node0
### Option 1: `From Source` installation
**1. Open a screen session**
* It helps you to keep your node running in the background.
```
screen -S pluralis
```

**2. Clone repo**
```bash
git clone https://github.com/PluralisResearch/node0
cd node0
```

**3. Create conda environment**
```
conda create -n node0 python=3.11
```
* Enter `a`, then `y` when prompted.

**4. Activate conda environment**
```
conda activate node0
```

**5. Install Node0**
```
pip install .
```
 
**6. Generate `generate_script.py`**
```
python3 generate_script.py --host_port 49200 --announce_port ANNOUNCE_PORT --token HF_TOKEN --email EMAIL_ADDRESS
```
Replace the following variables with the ones you got in previous steps
* `49200`: Node's default host port. No change needed if you don't want to switch the default value.
* `ANNOUNCE_PORT`: A port which some GPU cloud providers (.e.g Vast) map to the host port for external connection.
  * If running on Vast GPU cloud, you got it in step: [Find exposed ports](#find-exposed-ports)
  * If running on WSL or your exposed port is same as your host port, remove this flag `--announce_port ANNOUNCE_PORT`
* `HF_TOKEN`: Your Huggingface access token
* `EMAIL_ADDRESS`: Your email address

After execution, Enter `N` if no need to change anything
```console
# response:
File start_server.sh is generated. Run ./start_server.sh to join the experiment.
```

**7. Start Node0**
```
./start_server.sh
```

<img width="1432" height="145" alt="image" src="https://github.com/user-attachments/assets/a25a97bd-39dd-427a-8a45-8b4914cb5787" />



**8. Screen commands**
* Minimize screen: `Ctrl`+`A`+`D`
* Return to screen: `screen -r ceremony`
* Kill ceremony when inside screen: `Ctrl`+`C`
* Kill screen when inside screen: `Ctrl`+`D`
* Kill screen when outside screen: `screen -XS ceremony quit`
* screens list: `screen -ls`


### Option 2: `Docker` installation
* Skip the whole step if you are running via Option 1: `From Source` installation

**1. Install Docker**
```bash
sudo apt install -y ca-certificates curl gnupg software-properties-common
```
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```bash
sudo systemctl start docker
sudo systemctl enable docker
```
```bash
sudo usermod -aG docker $USER
echo "Log out and back in, or run 'newgrp docker' to apply group changes."
```
```bash
# Verify installation
docker -v
docker compose -v
```

**2. Clone repo**
```bash
git clone https://github.com/PluralisResearch/node0
cd node0
```

**3. Pull docker image**
```
docker build . -t pluralis_node0
```
 
**4. Generate `generate_script.py`**
```
python3 generate_script.py --use_docker --host_port 49200 --announce_port ANNOUNCE_PORT --token HF_TOKEN --email EMAIL_ADDRESS
```
Replace the following variables with the ones you got in previous steps
* `49200`: Node's default host port. No change needed if you don't want to switch the default value.
* `ANNOUNCE_PORT`: A port which some GPU cloud providers (.e.g Vast) map to the host port for external connection.
  * If running on Vast GPU cloud, you got it in step: [Find exposed ports](#find-exposed-ports)
  * If running on WSL or your exposed port is same as your host port, remove this flag `--announce_port ANNOUNCE_PORT`
* `HF_TOKEN`: Your Huggingface access token
* `EMAIL_ADDRESS`: Your email address

After execution, Enter `N` if no need to change anything
```console
# response:
File start_server.sh is generated. Run ./start_server.sh to join the experiment.
```

**5. Start Node0**
```
./start_server.sh
```

<img width="1432" height="145" alt="image" src="https://github.com/user-attachments/assets/a25a97bd-39dd-427a-8a45-8b4914cb5787" />

**6. Check logs**
```
tail -f run.out
```

---

