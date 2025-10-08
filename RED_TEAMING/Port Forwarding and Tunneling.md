
SSH tunneling command
` ssh -R <ATTACKER_PORT>:<ATTACKER_IP>:<TARGET_PORT> terget_user@<TARGET_IP> `


## Local Port Forwarding (Most common)

text

`ssh -L [local_port]:[remote_host]:[remote_port] [user]@[ssh_server]`

- Forwards connections on your local machine's `[local_port]` to the `[remote_host]:[remote_port]` through the `[ssh_server]`.
    
- Example: Forward local port 8080 to a remote web server's port 80 via SSH server
    

text

`ssh -L 8080:internal-webserver:80 user@ssh-server.com`

Then open `http://localhost:8080` to access the remote web server.

## Remote Port Forwarding

text

`ssh -R [remote_port]:[local_host]:[local_port] [user]@[ssh_server]`

- Forwards connections on the remote server's `[remote_port]` to your local machine's `[local_host]:[local_port]`.
    
- Example: Make local web server on port 3000 accessible on remote server port 8080
    

text

`ssh -R 8080:localhost:3000 user@ssh-server.com`

## Dynamic Port Forwarding (SOCKS proxy)

text

`ssh -D [local_socks_port] [user]@[ssh_server]`

- Creates a SOCKS proxy on your local machine on specified port.
    
- Example:
    

text

`ssh -D 1080 user@ssh-server.com`

Then configure applications to use SOCKS proxy `localhost:1080`.

## Additional useful options:

- `-N` : Do not execute remote commands; useful for just forwarding ports.
    
- `-f` : Requests ssh to go to background just before command execution.
    

---

## Example: Local Port Forwarding running in background without remote command:

text

`ssh -f -N -L 9090:remote-server:80 user@ssh-server.com`

These commands allow secure tunneling of network traffic over SSH for accessing internal or restricted services remotely.[](https://www.revsys.com/writings/quicktips/ssh-tunnel.html)