# pfSense-On-Hyper-V 

# Installing pfSense on Microsoft Hyper-V

A comprehensive guide for installing pfSense® software in a virtual machine under Microsoft Hyper-V. This guide is applicable to any Hyper-V version, including desktop and standalone Hyper-V Server.

 **Note:** While virtualization is convenient, if pfSense will be used as a perimeter firewall where security is critical, consider running it on dedicated hardware to minimize the attack surface.

## Prerequisites

- Windows with Hyper-V role/feature installed
- Basic understanding of networking and Hyper-V virtualization
- pfSense installation ISO image

## Setting up Hyper-V Networking

### 1. Create LAN Virtual Switch
1. Open Hyper-V Manager
2. Click **Virtual Switch Manager** from **Actions** menu
   
   ![image](https://github.com/user-attachments/assets/dec29713-6d0d-42de-8ec1-a26d34a67f68)

### 2. Create LAN Virtual Switch

   ![image](https://github.com/user-attachments/assets/f69be430-a835-40d0-8a66-fc937f15f177)
 
1. Create new switch:
   - **Type**: Private
   - **Name**: LAN
   - **Connection type**: Private network
   - Click **Apply**
   - Set an appropriate description in the **Notes** field 
   

    ![image](https://github.com/user-attachments/assets/c3ec04eb-8221-4bed-9da0-4b573106f86c)

### 3. Create WAN Virtual Switch


    ![image](https://github.com/user-attachments/assets/b4ed8bdd-273f-45f4-9abd-a8db1ba1550d)

1. Create another switch: 
   - **Type**: External
   - **Name**: WAN
   - Select appropriate physical network adapter
   - Optional: Uncheck "**Allow management operating system to share this network adapter**" if using dedicated WAN interface

    ![image](https://github.com/user-attachments/assets/741738ad-603b-4034-9bb5-8aea845f5063)

For the purpose of this guide the management was allowed, however production use requires a separate NIC for WAN.
 
## Creating the Virtual Machine

1. Start New Virtual Machine Wizard
2. Configure basic settings:
   - **Name**: pfSense
   - **Generation**: 2

     ![image](https://github.com/user-attachments/assets/04dce181-44d8-40c2-9b02-f3deec419e3d)

   - **Memory**: 1GB minimum (2GB recommended for packages)
   - **Network**: Select WAN switch

      ![image](https://github.com/user-attachments/assets/4a480f42-6c5a-4c15-9712-00eddfa42a5b)

   - **Hard Disk**: 10-20GB (more if using IDS/IPS packages)
   - **Installation media**: Select pfSense ISO

### Post-Creation Configuration

1. Add LAN Network Adapter:
   - **Settings → Add Hardware**
 
     ![image](https://github.com/user-attachments/assets/f1a43042-a4af-4fa4-9b66-08e5b463d871)

   - Select **Network Adapter**
   - Connect to LAN virtual switch

      ![image](https://github.com/user-attachments/assets/19072ee2-e960-446a-bc66-ca69fd2a9635)


1. Disable Secure Boot:
   - **Settings → Security**
   - Uncheck **Enable Secure Boot**
  

    ![image](https://github.com/user-attachments/assets/09398d2b-4178-4e92-8cae-3b6b3cb35008)


2. Adjust Boot Order:
   - **Settings → Firmware**
   - Move Hard Drive to top of boot order

   
 ![image](https://github.com/user-attachments/assets/51d3f281-ec60-4f3c-8d74-0f2c6db4fe68)


1. Start VM and connect to console
2. Accept EULA
3. Follow standard pfSense installation steps
4. When installation completes, remove ISO and reboot

## First Boot Configuration

1. Interface Assignment:

   ```
   VLAN setup: n
   WAN interface: hn0
   LAN interface: hn1
   Proceed: y
   ```

3. Verify interface IP assignments:
   - WAN should receive IP via DHCP or your configured method
   - LAN default: 192.168.1.1/24

## Next Steps

- Access web configurator via LAN interface (default: https://192.168.1.1)
- Complete initial setup wizard
- Configure firewall rules and additional features as needed

## Tips and Troubleshooting

- Verify MAC addresses in Hyper-V settings if unsure about interface assignments
- Minimum 1GB RAM required, but 2GB+ recommended for package usage
- Increase virtual disk size if planning to use bandwidth-intensive packages
- Make sure both network adapters are connected to correct virtual switches

Congratulations! The virtual machine is now running pfSense software on Microsoft Hyper-V.
