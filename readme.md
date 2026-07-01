# DHCP Server Deployment

A beginner-friendly networking lab that demonstrates how to install, configure, and test a DHCP (Dynamic Host Configuration Protocol) server on Ubuntu Server. This project shows how a Linux server can automatically assign IP addresses and other network settings to client devices.

---

## Project Overview

The purpose of this project is to understand how DHCP works in real-world networks by configuring an Ubuntu Server as a DHCP server. The server is configured with a static IP address and provides automatic IP address allocation to clients within a specified address range.

---

## Features

- Install and configure ISC DHCP Server
- Configure a static IP address using Netplan
- Configure a DHCP address pool
- Assign IP addresses automatically to clients
- Configure default gateway
- Configure subnet mask
- Configure DNS server
- Verify DHCP service status
- Test client connectivity
- Beginner-friendly documentation

---

## Technologies Used

- Ubuntu Server
- ISC DHCP Server
- Netplan
- Linux Terminal
- VMware Workstation / VirtualBox

---

## Project Structure

```text
dhcp-server-deployment/
│
├── README.md
├── requirements.txt
├── lab-notes.md
├── screenshots/
│   ├── network-topology.png
│   ├── netplan-config.png
│   ├── dhcp-config.png
│   ├── service-status.png
│   ├── client-ip.png
│   └── ping-test.png
│
├── configurations/
│   ├── dhcpd.conf
│   └── 00-installer-config.yaml
│
└── network-diagram.png
```

---

## Network Configuration

### DHCP Server

| Setting | Value |
|---------|-------|
| Interface | ens33 |
| Static IP | 192.168.10.1 |
| Subnet Mask | 255.255.255.0 |

### DHCP Pool

| Setting | Value |
|---------|-------|
| Network | 192.168.10.0/24 |
| IP Range | 192.168.10.100 - 192.168.10.200 |
| Gateway | 192.168.10.1 |
| DNS | 8.8.8.8, 8.8.4.4 |
| Default Lease Time | 600 seconds |
| Maximum Lease Time | 7200 seconds |

---

## Installation

### Update Ubuntu

```bash
sudo apt update
```

### Install ISC DHCP Server

```bash
sudo apt install isc-dhcp-server
```

---

## Configure Network Interface

Edit the Netplan configuration file:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Example configuration:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - 192.168.10.1/24
```

Apply changes:

```bash
sudo netplan apply
```

---

## Configure DHCP Server

Edit:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Example configuration:

```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option subnet-mask 255.255.255.0;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    default-lease-time 600;
    max-lease-time 7200;
}
```

---

## Configure DHCP Interface

Edit:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Set:

```text
INTERFACESv4="ens33"
```

---

## Start DHCP Service

```bash
sudo systemctl restart isc-dhcp-server
```

Enable the service:

```bash
sudo systemctl enable isc-dhcp-server
```

Check status:

```bash
sudo systemctl status isc-dhcp-server
```

---

## Verification Commands

Display network interface:

```bash
ip addr show ens33
```

Display routing table:

```bash
ip route
```

Check DHCP status:

```bash
sudo systemctl status isc-dhcp-server
```

View listening ports:

```bash
sudo ss -ulnp | grep :67
```

---

## Testing

The client machine should automatically receive:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

Connectivity can be verified using:

```bash
ping 192.168.10.1
```

---

## Networking Concepts Covered

- DHCP (Dynamic Host Configuration Protocol)
- DORA Process
- IPv4 Addressing
- Static IP Configuration
- Dynamic IP Allocation
- Netplan
- Linux Networking
- Gateway Configuration
- DNS Configuration
- Service Management

---

## Learning Outcomes

After completing this project, I learned how to:

- Install and configure Ubuntu Server
- Configure a static IP using Netplan
- Install ISC DHCP Server
- Configure DHCP scopes
- Manage Linux services using systemctl
- Verify network configuration
- Troubleshoot DHCP configuration
- Test automatic IP assignment

---

## Future Improvements

- Configure multiple DHCP scopes
- Add DHCP reservations using MAC addresses
- Configure DHCP failover
- Capture DHCP packets using Wireshark
- Integrate DNS server
- Deploy in a multi-subnet environment

---

## Author

**Rana Umar**

---

## License

This project is open-source and intended for educational purposes.