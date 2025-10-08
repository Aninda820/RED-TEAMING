

### **Change network configuration and interfaces in metasploitable2 machine** 
`  sudo nano /etc/network/interfaces  `

```
# External network
auto eth0
iface eth1 inet static
address 192.168.133.50
netmask 255.255.255.0
gateway 192.168.1.1
    
    
# Internal network
auto eth1
iface eth1 inet static
address 10.10.10.5
netmask 255.255.255.0
gateway 10.10.10.1
```

After complete the network configuration once restart the network
`  sudo /etc/init.d/networking restart  `





### **Kali-Linux network configuration**
`  sudo nano /etc/network/interfaces  `

```
auto eth0
iface eth0 inet static 
address 192.168.133.2
netmask 255.255.255.0
gateway 192.168.150.1
```

After complete the network configuration once restart the network
`  sudo /etc/init.d/networking restart  `

  or use this command 
  
`  sudo systemctl restart networking  `



					Active directory
kali --> metasploit -->      App
					 User







