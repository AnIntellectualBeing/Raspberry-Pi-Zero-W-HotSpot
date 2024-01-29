# Raspberry-Pi-Zero-W-HotSpot
Raspberry Pi Zero W HotSpot


```markdown
# Raspberry Pi Wireless Access Point Setup

This guide will walk you through setting up your Raspberry Pi as a wireless access point. You can connect to this access point and, if configured, share your internet connection.

## Prerequisites

- Raspberry Pi with Raspbian OS installed
- A compatible Wi-Fi adapter
- Administrative access to the Raspberry Pi

## Steps

### 1. Update and upgrade the system packages:

```bash
sudo apt-get update
sudo apt-get upgrade
```

### 2. Install required packages for the access point setup:

```bash
sudo apt-get install hostapd dnsmasq dhcpcd5 iptables
```

### 3. Configure the hostapd configuration file:

```bash
sudo nano /etc/hostapd/hostapd.conf
```

### 4. Edit the `dhcpcd.conf` file:

```bash
sudo nano /etc/dhcpcd.conf
```

### 5. Restart the dhcpcd service:

```bash
sudo service dhcpcd restart
```

### 6. Configure the dnsmasq configuration file:

```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
sudo nano /etc/dnsmasq.conf
```

### 7. Set up IP forwarding:

```bash
sudo nano /etc/sysctl.conf
```

Add the following line to the sysctl configuration:

```
net.ipv4.ip_forward=1
```

### 8. Enable NAT for internet sharing:

```bash
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
```

### 9. Configure IP forwarding rules:

```bash
sudo iptables -A FORWARD -i wlan0 -o wlan1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan1 -o wlan0 -j ACCEPT
```

### 10. Save the iptables configuration:

```bash
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

### 11. Create an executable script for iptables:

```bash
sudo nano /etc/network/if-pre-up.d/iptables.sh
sudo chmod +x /etc/network/if-pre-up.d/iptables.sh
```

### 12. Reboot the Raspberry Pi:

```bash
sudo reboot
```

### 13. After the reboot, navigate to the `nodogsplash` directory:

```bash
cd nodogsplash/
```

### 14. Enable nodogsplash:

```bash
sudo systemctl enable nodogsplash
```

### 15. Configure nodogsplash:

```bash
sudo nano /etc/nodogsplash/nodogsplash.conf
```

### 16. Check IP forwarding status:

```bash
cat /proc/sys/net/ipv4/ip_forward
```

### 17. Check NAT and iptables rules:

```bash
sudo iptables -t nat -L -n
sudo iptables -L -n
```

### 18. Check dnsmasq and hostapd configuration files:

```bash
cat /etc/dnsmasq.conf
cat /etc/hostapd/hostapd.conf
```

### 19. Start the hostapd service:

```bash
sudo systemctl start hostapd
```

### 20. Unmask and enable hostapd service:

```bash
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
sudo systemctl start hostapd
```

### 21. Verify the `hostapd` configuration:

```bash
cat /etc/hostapd/hostapd.conf
```

You have successfully set up your Raspberry Pi as a wireless access point. You can now connect to it using the configured SSID and password. If you have internet access through the access point, the basic setup is complete. 
