## Chained Command Install for Subnet 27 Miners

###### Allows for quicker install. Have left gaps rather than chaining everything together to allow user to check if the install is working correctly at key points.

### Docker Install:

`sudo apt-get update && sudo apt-get install ca-certificates curl && sudo install -m 0755 -d /etc/apt/keyrings && sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && sudo chmod a+r /etc/apt/keyrings/docker.asc && echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && sudo docker run hello-world`
  
  
### Subtensor Install:

`cd && git clone https://github.com/opentensor/subtensor.git && sudo ufw allow 30333/tcp && cd subtensor && sudo ./scripts/run/subtensor.sh -e docker --network mainnet --node-type lite && docker ps`

### Bittensor Install:

`cd && /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/opentensor/bittensor/master/scripts/install.sh)" && btcli --help`

### (Create/Regen Keypair)

### Subnet 27 Install & Hashcat:

`cd && git clone https://github.com/neuralinternet/Compute-Subnet.git && cd Compute-Subnet && python3 -m pip install -r requirements.txt && python3 -m pip install -e . && sudo apt -y install ocl-icd-libopencl1 pocl-opencl-icd && wget https://hashcat.net/files/hashcat-6.2.6.tar.gz && tar xzvf hashcat-6.2.6.tar.gz && cd hashcat-6.2.6/ && sudo make && sudo make install && export PATH=$PATH:/usr/local/bin/ && echo "export PATH=$PATH">>~/.bashrc && hashcat --version`

### Install NVIDIA CUDA Toolkit & Drivers:

`cd && sudo apt install build-essential && sudo apt install software-properties-common && sudo add-apt-repository ppa:ubuntu-toolchain-r/test && sudo apt update && sudo apt upgrade && sudo apt install gcc-12 g++-12 gcc-13 g++-13 -y && sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12 --slave /usr/bin/g++ g++ /usr/bin/g++-12 && sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 13 --slave /usr/bin/g++ g++ /usr/bin/g++-13 && sudo update-alternatives --config gcc && wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin && sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600 && wget https://developer.download.nvidia.com/compute/cuda/12.3.1/local_installers/cuda-repo-ubuntu2204-12-3-local_12.3.1-545.23.08-1_amd64.deb && sudo dpkg -i cuda-repo-ubuntu2204-12-3-local_12.3.1-545.23.08-1_amd64.deb && sudo cp /var/cuda-repo-ubuntu2204-12-3-local/cuda-*-keyring.gpg /usr/share/keyrings/ && sudo apt-get update && sudo apt-get -y install cuda-toolkit-12-3 && sudo apt-get install -y nvidia-kernel-open-545 && sudo apt-get install -y cuda-drivers-545 && export CUDA_VERSION=cuda-12.3 && export PATH=$PATH:/usr/local/$CUDA_VERSION/bin && export LD_LIBRARY_PATH=/usr/local/$CUDA_VERSION/lib64 && echo "">>~/.bashrc && echo "PATH=$PATH">>~/.bashrc && echo "LD_LIBRARY_PATH=$LD_LIBRARY_PATH">>~/.bashrc && sudo reboot`

`nvidia-smi && nvcc --version`

### Install PM2:

`cd && sudo apt update && sudo apt install npm && sudo npm install pm2 -g && pm2 ls`


### Start Docker service in Compute-Subnet:

`cd ~/Compute-Subnet && sudo systemctl start docker && sudo apt install at && sudo service docker status`

### Setting up firewall for miner:

`cd ~/Compute-Subnet && sudo apt update && sudo apt install ufw && sudo ufw allow 2345:4532/tcp && sudo ufw allow 22/tcp && sudo ufw enable && sudo ufw status`

### (Register miner to subnet)


### Start Miner script:

`cd ~/Compute-Subnet && pm2 start ./neurons/miner.py --name MINER --interpreter python3 -- --netuid 27 --subtensor.network local --wallet.name COLDKEYNAME --wallet.hotkey HOTKEYNAME --axon.port 2345 --logging.debug --miner.blacklist.force_validator_permit --auto_update yes && pm2 logs`