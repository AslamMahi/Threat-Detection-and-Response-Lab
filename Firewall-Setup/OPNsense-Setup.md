# OPNsense Firewall Setup for a Network with Windows and Ubuntu Machines

## Overview
In this setup, we have:
- **Windows Machine** (Victim) on the LAN.
- **Ubuntu Machine** (Logging/Monitoring Tools) on the LAN.
- Communication between the Windows and Ubuntu machines is restricted to the LAN.

This document covers the steps to:
1. Download and install OPNsense.
2. Configure LAN and WAN interfaces.
3. Set the default login credentials.
4. Add firewall rules to restrict traffic between the Windows and Ubuntu machines to the LAN only.
5. Enable firewall logging and view logs for communication between machines.

---

## Step 1: Download and Install OPNsense

### 1.1 Download OPNsense

1. **Download OPNsense ISO**:
   - Navigate to the [OPNsense Download Page](https://opnsense.org/download/).
   - Select the appropriate image for your hardware (e.g., ISO or VGA) and download it.

2. **Create Bootable Media**:
   - If installing on physical hardware, use software like [Rufus](https://rufus.ie/) or [balenaEtcher](https://www.balena.io/etcher/) to create a bootable USB drive.
   - If installing on a virtual machine, mount the ISO file directly.

### 1.2 Install OPNsense

1. **Boot from the OPNsense Installation Media**:
   - Insert your bootable USB drive or start the virtual machine with the ISO mounted.
   - Boot into the OPNsense installer.

2. **Follow the Installation Wizard**:
   - Select the default options unless you have specific requirements.
   - When prompted, install OPNsense on your selected disk.

3. **Default Login Credentials**:
   - After installation is complete, the system will reboot.
   - The default login credentials for OPNsense are:

     - **Username**: root
     - **Password**: opnsense

   Make sure to change the default password after logging in.

---

## Step 2: Initial Setup – Interfaces and Network Configuration

### 2.1 Access the Console Menu

1. Once OPNsense finishes booting, you will be presented with the **OPNsense Console Menu**.
2. Select **Option 1**: Assign Interfaces to configure LAN and WAN.

### 2.2 Configure WAN Interface

1. **Assign WAN Interface**:
   - When prompted, type the network interface for your WAN connection. Example: `igb0`, `em0`, or whatever corresponds to your external network interface.
   - Set **IPv4 Configuration Type** to `DHCP` (recommended unless using a static IP from your ISP).

2. **Assign LAN Interface**:
   - Type the network interface for your LAN. Example: `igb1`, `em1`, etc.
   - Set **IPv4 Configuration Type** to `Static` and assign an IP address, e.g., `192.168.1.1/24`.

3. **Complete Interface Assignment**:
   - Confirm the interface assignments and finish the setup.

### 2.3 Log into the Web Interface

1. Open a web browser and navigate to the LAN IP address, for example, `https://192.168.1.1`.
2. **Login** with the default credentials:
   - **Username**: root
   - **Password**: opnsense

3. Follow the setup wizard in the web UI to configure basic settings like time zone, DNS, etc.

---

## Step 3: Set Static IP Addresses for Windows and Ubuntu Machines

1. **Windows Machine**:
   - Assign the static IP `192.168.1.100` on your Windows machine by navigating to **Network and Sharing Center** > **Change Adapter Settings** > **Properties** > **IPv4 Properties**.

2. **Ubuntu Machine**:
   - On the Ubuntu machine, set a static IP address `192.168.1.101` by editing the `/etc/netplan` configuration or using the network settings GUI.

---

## Step 4: Firewall Rules for Windows and Ubuntu Communication

1. **Go to Firewall Rules**:
   - Navigate to **Firewall** > **Rules** > **LAN**.

2. **Add Rule to Allow Communication Between Windows and Ubuntu**:
   - Click the **+ Add** button to create a new rule.
   - Fill in the rule as follows:
   
   **Action**: Pass  
   **Interface**: LAN  
   **Protocol**: Any (for allowing all communication types)  
   **Source**: Single host or network: `192.168.1.100` (Windows IP)  
   **Destination**: Single host or network: `192.168.1.101` (Ubuntu IP)  
   **Description**: Allow Windows-to-Ubuntu communication

   - Click **Save** and **Apply Changes**.

3. **Add Rule to Allow Communication from Ubuntu to Windows**:
   - Create a second rule, similar to the first one, but swap the source and destination IP addresses:

   **Action**: Pass  
   **Interface**: LAN  
   **Protocol**: Any  
   **Source**: Single host or network: `192.168.1.101` (Ubuntu IP)  
   **Destination**: Single host or network: `192.168.1.100` (Windows IP)  
   **Description**: Allow Ubuntu-to-Windows communication

   - Click **Save** and **Apply Changes**.

4. **Restrict Traffic to LAN Only**:
   - To prevent communication between these machines and any other networks (such as the internet), ensure that these rules are higher in priority than any general allow rules for outbound traffic.

---

## Step 5: Enable Logging for Traffic Between the Machines

1. **Log Firewall Events**:
   - Edit the rules created above and enable the **Log** option to ensure all traffic between the Windows and Ubuntu machines is logged.

   - Save and apply the changes.

2. **View Logs**:
   - Navigate to **Firewall** > **Logs** > **Live View** to view the logged traffic between the two machines.

---

## Step 6: Testing the Setup

1. **Ping Test**:
   - From the **Windows** machine, open the command prompt and ping the **Ubuntu** machine's IP: `ping 192.168.1.101`.
   - From the **Ubuntu** machine, use the terminal to ping the **Windows** machine's IP: `ping 192.168.1.100`.

2. **Check Communication**:
   - Test any other communication between the two machines, such as HTTP, file sharing, or SSH, depending on the services running on the Ubuntu machine.

3. **Verify Logs**:
   - Return to the OPNsense web UI to verify that the traffic is being logged as expected.

---

## Step 7: Additional Configuration (Optional)

1. **Block All Other Traffic**:
   - After the above rules are set, you can optionally block all other traffic between other devices on the LAN by adding a **Block** rule for the entire subnet.
   - Navigate to **Firewall** > **Rules** > **LAN** and add a rule that blocks all traffic except the ones specified above.

---

This `.md` file provides a comprehensive guide for setting up the OPNsense firewall to manage and monitor traffic between a Windows victim machine and an Ubuntu machine, while restricting communication to the LAN.
