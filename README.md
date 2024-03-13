### Original Guide:

[Miner Setup Guide for SN 27](https://docs.neuralinternet.ai/products/subnet-27-compute/bittensor-compute-subnet-miner-setup)

## Extra steps following on from subnet27 guide:

### SubTensor Install Extra Steps:

`rm -rf subtensor/`

`git clone https://github.com/opentensor/subtensor.git`

`sudo ufw allow 30333/tcp`

`cd subtensor/`

`sudo ./scripts/run/subtensor.sh -e docker --network mainnet --node-type lite`

Confirm the local subtensor is working with:

`btcli s list --subtensor.chain_endpoint ws://127.0.0.1:9944`

Run Docker to see if it is running for Subtensor with:

`docker ps`

![Docker Subtensor Running](./dockerSubtensor.jpeg "Docker Subtensor Running")




### NVDIA CUDA Drivers install extra steps


`sudo apt install build-essential`

`sudo apt install software-properties-common`

`sudo add-apt-repository ppa:ubuntu-toolchain-r/test`

`sudo apt update`

`sudo apt upgrade`

`sudo apt install gcc-12 g++-12 gcc-13 g++-13 -y`

`sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12 --slave /usr/bin/g++ g++ /usr/bin/g++-12`

`sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 13 --slave /usr/bin/g++ g++ /usr/bin/g++-13`

`sudo update-alternatives --config gcc`

`wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin`

`sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600`

`wget https://developer.download.nvidia.com/compute/cuda/12.3.1/local_installers/cuda-repo-ubuntu2204-12-3-local_12.3.1-545.23.08-1_amd64.deb`

`sudo dpkg -i cuda-repo-ubuntu2204-12-3-local_12.3.1-545.23.08-1_amd64.deb`

`sudo cp /var/cuda-repo-ubuntu2204-12-3-local/cuda-*-keyring.gpg /usr/share/keyrings/`

`sudo apt-get update`

`sudo apt-get -y install cuda-toolkit-12-3`

`sudo apt-get install -y nvidia-kernel-open-545`

`sudo apt-get install -y cuda-drivers-545`

I am not sure if 545 will work or if the new version if 550, this worked for me when I was mining

The above steps should remove the hashcat error when trying to get the miner to run and checking logs using `pm2 logs`:

![HashCatError](./hashcaterror.png "Hashcat Error")


### Extra Steps:

##### To check if miner is getting rewards:

`btcli w overview --wallet.name $cold --subtensor.network finney`

Replacing the $cold with the coldkey

##### To check what ports are open use:

`sudo ufw status`

![Ports Check](./portscheck.png "Ports Check")

Ports which were specified during install (2345:4532) and port 30333 should be open. 

##### To check miner hash speed:

`hashcat -b -m 610`

##### To SSH into remote machine:

`ssh <user>@<ip_address> -i ~/.ssh/id_ed25519`

##### To regenerate coldkey/hotkey:

to recreate coldkey if it gets lost/want to use on another miner:

`btcli w regen_coldkey --mnemonic <words>`

to recreate hotkey:

`btcli w regen_hotkey --mnemonic <words>`

### Other Comments:

* it is best currently to run only one gpu per hotkey
* don't remove staked TAO whilst miner is still running






