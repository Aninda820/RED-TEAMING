

| Extension |	         | Purpose |	                                  | Example Use |
.ps1	-->    Regular PowerShell scripts	                Automation scripts, tools
.psm1	-->Modules with reusable functions  	Custom libraries
.psd1 -->   Data/configuration file	                    Store settings, manifests
.ps1xml --> Defines object display in console	 Customizing output views
.psc1	--   Custom console setup               	    Preconfigured PS environment
.pssc	-    Session configuration for JEA	        Role-based restricted remoting

##### PowerShell command for scan open TCP ports
```
1..1024 | % { echo ((new-object Net.Sockets.TcpClient).Connect("192.168.0.101",$_)) "Port $_ is open!"} 2>$null
```

