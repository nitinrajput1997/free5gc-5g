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
