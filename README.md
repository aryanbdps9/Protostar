# Solution scripts of [Protostar](https://exploit.education/protostar/) challenges

# Acknowledgement
I was watching a [LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w) [playlist](https://www.youtube.com/playlist?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN) and was trying to solve these problems on my own (before starting the video, if that makes sense). So, almost everything is taken from there (including setup).

# Setup
1. Install [VMware Workstation Player](https://www.vmware.com/in/products/workstation-player/workstation-player-evaluation.html).
2. Download [Protostar ISO](https://github.com/ExploitEducation/Protostar/releases/download/v2.0.0/exploit-exercises-protostar-2.iso)
3. Create a VM from VMware with a **32-bit OS** (I used **Debian 8.x** as OS) using the protostar ISO that you just downloaded.
4. Start the VM. 
5. Login using username `user`:
   1. Here are the login credentials for ordinary user:
      - username: `user`
      - password: `user`
   2. Here are the login credentials for root user:
      - username: `root`
      - password: `godmode`
6. Change your default shell to bash:
   1. Run `chsh`.
   2. Type your password(`user`) and press `enter`.
   3. In the `Login Shell` prompt, type `/bin/bash` and press `enter`
7. Note down IP address of the machine. 
   1. Type `id addr`. Sample output:
   ```bash
   user@protostar:~$ ip addr
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
       inet6 ::1/128 scope host
          valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
       link/ether 00:0c:29:38:63:e6 brd ff:ff:ff:ff:ff:ff
       inet 192.168.183.128/24 brd 192.168.183.255 scope global eth0
       inet6 fe80::20c:29ff:fe38:63e6/64 scope link
          valid_lft forever preferred_lft forever
    ```
    2. The number next to `inet` in `eth0` section (`192.168.183.128` in the sample output) is the IP address of the machine.
    Note: This IP doesn't change very frequently.
8. Connect to the machine using ssh. Type: `ssh user@<ip noted in previous step>` from a terminal. (Now Windows also has ssh and there is no need to install putty).
