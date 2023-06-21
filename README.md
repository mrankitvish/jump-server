## Deploy a Jump Server

---
##### Description:
A jump server creates a secure connection between a user's machine and a target machine by establishing a tunnel. It enables users to securely access internal resources without directly exposing them to the public internet.

##### Pre-requisites:
- The setup of a jump server requires a server running OpenSSH (sshd) service to act as the intermediary.
- The jump server must be configured with routing that enables it to connect to both the external network and the internal network where the target machine is located, allowing users to securely connect to the target machine through the jump server.
---

##### Configure jump server:
1. Loging to your jump server machine
2. Edit sshd_config file
```bash
# vim /etc/ssh/sshd_config
PermitTunnel yes
AllowTcpForwarding yes
GatewayPorts yes
```
3. Save the file
4. Restart the sshd service
```bash
# systemctl restart sshd.service
```

##### Configure target machine:
1. Loging to your target machine
2. Edit sshd_config file
```bash
# vim /etc/ssh/sshd_config
AllowTcpForwarding yes
```
3. Save the file
4. Restart the sshd service
```bash
# systemctl restart sshd.service
```
###### `Note`: In some cases, a jump server may work without any configuration on the target machines. However, additional configuration may be required depending on the specific setup and requirements of the network and the resources being accessed.

##### Test jump server:
1. Loging to your machine (client)
2. (Optional) Copy ssh public to your jump server
```bash
# ssh-keygen
```
```bash
# ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@<ip-address-of-jump-server>
```
3. Login the target machine using jump server
```bash
# ssh -J <username>@<ip-address-of-jump-server> <username-of-target-machine>@<ip-address-of-target-machine>
```