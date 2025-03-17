# pfSense/Ubuntu VM Installation and Network Configuration

# Description
This project documents the deployment and configuration of pfSense as a firewall and Ubuntu as an internal network client within a VirtualBox environment. The goal is to create a secure network setup, allowing pfSense to act as the firewall/router and Ubuntu to operate as a client within an internal subnet.

## Tools and Utilities
- **VirtualBox**: Used for virtualization of pfSense and Ubuntu.
- **pfSense**: Open-source firewall and router software.
- **Ubuntu Desktop 24**: Internal network client.
- **Networking Tools**: Command-line utilities such as ip addr, ping, and grep for troubleshooting.
- **DHCP & DNS Configuration**: Used within pfSense to manage IP assignments and name resolution.

# Lab Topology
1. pfSense will have **two Network Interfaces**
    - Bridged Adapted (Acting as WAN)
    - Internal Network (Acting as LAN)
2. Ubuntu Desktop 
    - Will be sitting in the internal network and anything trying to get to the internet from the internal network will have to go through pfSense firewall to do so
      
![image](https://github.com/user-attachments/assets/6ba35a2a-ba99-4d0a-b781-54178a2d9e86)

# Documentation
## pfSense Installation and Configuration

1. In Virtual Box, create a New VM and name it **pfSense.** For the type and Version, it should be **BSD** and **FreeBSD (64-bit)**
2. For RAM, there should be at least **2GB,** CPU **1 core**, and Storage **16GB**
3. In the Settings of the VM, select storage and mount the **pfSense ISO Image** 
4. In the **Network Settings, Adapter 1 - Bridged** and **Adapter 2 - Internal (inet)** 
5. Run the pfSense VM and select Ok to all default configs. Once the installation is finished, exit to the **shell** and run the following command

```jsx
shutdown -h now 
```

6. While the OS is halted, press the **CTRL + ALT** and at the bottom of the VM screen, select the storage icon and **unmount** the ISO Image, otherwise the config process will be ran again

## Ubuntu Server Installation and Configuration

1. Add an **Ubuntu machine** in Virtualbox with at least basic RAM, CPU, and storage
2. Once created, select **Settings** for the Ubuntu machine and under storage, mount the Ubuntu 24 Desktop ISO Image to the machine 
3. Under Network, select **Internal Network** for the interface as this machine will be on the internal network (LAN)
4. Run the Ubuntu machine and finish installing Ubuntu Desktop
5. Once installed if the machine is on the **Internal Network** 

```powershell
ip addr | grep inet 
```

![image 1](https://github.com/user-attachments/assets/fcc1060d-3d36-4a0e-b4c7-49c12c6bbfd0)


## Accessing pfSense via Ubuntu

1. In the browser of the Ubuntu machine, access pfSense by inputting the IP Address of the firewall in the URL. It will say it is an insecure connection because it is a self signed certificate. Go to advanced settings and confirm exception
    - e.g. 192.168.1.1

![Screenshot_(1012)](https://github.com/user-attachments/assets/f66b11df-17a4-47b9-803b-b1d04ae896e5)

# pfSense Initial Setup

Always be sure to start the pfSense firewall and know the LAN Address

![Screenshot_(1013)](https://github.com/user-attachments/assets/7af7fa84-d81d-409f-836b-4c6051291a19)

1. Check if both the firewall and Ubuntu Desktop are both on the right subnet (e.g. 192.168.1.0/24)
2. The login information to access pfSense in the browser is **admin:pfsense**
3. Continue with the setup of pfSense and for DNS, Google’s DNS Servers can be used 

![image 2](https://github.com/user-attachments/assets/15a1c8d8-e1aa-42ea-a326-1d7cf43c4804)

![image 3](https://github.com/user-attachments/assets/af20872f-0608-4fa5-b227-c8bdb04df3c8)

4. The WAN interface can be left as DHCP and the following sections can be left blank
5. The LAN interface can be left as default 192.168.1.1 unless desired otherwise and subnet size can be left as /24 

![Screenshot_(1016)](https://github.com/user-attachments/assets/c65a8a55-61b5-4b73-be58-db7eba2c3ab2)

6. Admin password should be changed and once finished, select **reload** and the firewall be configured and rebooted

![Screenshot_(1017)](https://github.com/user-attachments/assets/5dde9ff4-7dfd-4e4e-8668-6f4227a7c777)

## Adding DNS Servers to DHCP

1. Currently the Ubuntu machine behind the pfSense firewall is unable to resolve queries such as pinging a domain

![image 4](https://github.com/user-attachments/assets/6338f3bc-7176-4a34-be1e-863cd3c82abe)

2. But when pinging by IP address, the requests are received and replied to, so that means the **networking works, but name resolution does not**

![image 5](https://github.com/user-attachments/assets/c62e83d9-b98c-43ff-a174-0c88d0c50b07)

3. In the pfSense web GUI navigate to **Services → DHCP Server.** Under Server Settings input 8.8.8.8 and 8.8.4.4 to resolve domain names, then reset both the firewall and ubuntu machine

![image 6](https://github.com/user-attachments/assets/28be5c0f-12bd-4428-a795-3adfde8b06b7)

![image 7](https://github.com/user-attachments/assets/61c0447a-7fee-4361-90fc-8c88ba07fa26)


# Conclusion

This setup successfully deploys **pfSense as a firewall** (both externally and internally facing) as well as an **Ubuntu client within an internal network**. The firewall provides network security, while proper DHCP and DNS configuration ensures Ubuntu clients can communicate effectively over the network. This environment can be expanded to include additional services such as VPNs, VLANs, or intrusion detection systems.















