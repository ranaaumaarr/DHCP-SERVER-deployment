# DHCP Server Deployment

Ubuntu Server lab configuring an ISC DHCP server to automatically assign IP addresses to clients.

## Setup
| Setting | Value |
|---|---|
| Interface | ens33 |
| Static IP | 192.168.10.1/24 |
| DHCP Range | 192.168.10.100 – 192.168.10.200 |
| Gateway | 192.168.10.1 |
| DNS | 8.8.8.8, 8.8.4.4 |
| Lease Time | 600s (default) / 7200s (max) |

## Installation
```bash
sudo apt update
sudo apt install isc-dhcp-server
```

## Configure Static IP (Netplan)
`/etc/netplan/00-installer-config.yaml`:
```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.10.1/24]
```
```bash
sudo netplan apply
```

## Configure DHCP Pool
`/etc/dhcp/dhcpd.conf`:
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    default-lease-time 600;
    max-lease-time 7200;
}
```
Set interface in `/etc/default/isc-dhcp-server`: `INTERFACESv4="ens33"`

## Start Service
```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

## Verification
```bash
ip addr show ens33
sudo ss -ulnp | grep :67
ping 192.168.10.1
```
Client should auto-receive IP, subnet mask, gateway, and DNS.

## Requirements
- Ubuntu Server 22.04 LTS+, VMware/VirtualBox

## Files
- `project.png`, `commands.png` — screenshots

## Author
Rana Umar
