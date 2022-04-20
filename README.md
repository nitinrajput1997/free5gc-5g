# free5gc-5g

### Install free5GC Core Network
```bash
git clone --recursive https://github.com/free5gc/free5gc.git
```

### Setting free5GC
In this step, we need to update the ngapIp in amfcfg.yaml and endpoints in smfcfg.yaml and gtpuIp in upfcfg.yaml with the IP of your server on which you are deploying free5gc. 
```bash
cd free5gc/config
```

### Run WebConsole
Install Node.js and Yarn
```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install -y nodejs yarn
```

To build WebConsole
```bash
cd free5gc
make webconsole
```

Use WebConsole to Add an UE
```bash
cd ~/free5gc/webconsole
go run server.go
```
 Open your web browser from your host machine, and enter the URL http://your_server_ip:5000
**Note:** On the login page, enter username admin and password free5gc

### Port NatForwarding
```bash
sudo sysctl -w net.ipv4.ip_forward=1
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo systemctl stop ufw
sudo iptables -I FORWARD 1 -j ACCEPT
```

make sure you have make proper changes to the free5GC configuration files, then run ./run.sh
```bash
cd ~/free5gc
./run.sh
```

### Install UERANSIM

```bash
# install cmake and other packages
sudo apt update
sudo apt upgrade
sudo apt install iproute2
sudo snap install cmake --classic
sudo apt install gcc
sudo apt install g++
sudo apt install libsctp-dev
sudo apt install make
```
```bash
# clone ueransim
git clone https://github.com/aligungr/UERANSIM
cd UERANSIM
make
```
### Setup gNB
We have to do some changes to the gNB config files located in UERANSIM/config/free5gc-gnb.yaml. Update the "linkIp", "ngapIp", "gtpIp" field with local ip (server ip) and "amfConfigs: address" field with amf ip.

```bash
# start gnb with open5gc-gnb.yaml config file
sudo ./build/nr-gnb -c config/free5gc-gnb.yaml
```

### Setup UE
We have to do some changes to the UE config files located in UERANSIM/config/free5gc-ue.yaml. Update the "gnbSearchList" with the IP address of the server.

```bash
# start gnb with open5gc-ue.yaml config file
sudo ./build/nr-ue -c config/free5gc-ue.yaml
```
