**Get listen ports and PID**
```bash
netstat -tuln -p
```
**Transfer files from netcat**
Sender:
```bash
nc -w 3 ipaddr port < file.txt
```
Receiver:
```bash
nc -lvnp port | pv -e -b -r | cat > file.txt
```
**Capture ICMP traffic on the interface network interface.**
```bash
tcpdump -i <interface> icmp
```
**Create a port forward (also known as SSH tunneling) using SSH**
```bash
ssh -L local_port:destination_host:destination_port user@ssh_server
```
Note: Make sure that the SSH server allows port forwarding (`AllowTcpForwarding` is set to `yes` in the SSH server configuration), and you have SSH access to the server.
**Establish a dynamic SSH tunnel through ssh_server on port 8080 with the specified user**
```bash
ssh -D 8080 user@ssh_server
```
