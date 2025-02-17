# Apps-Stop-Start-Control
Framework to start/stop and check applications running on a host

Main scripts are appcontrol.ps1 and appcontrol.sh

Wrapper scripts are provided as azcontrol.ps1 or something like that which basically just call the main scripts with some sort of logging, but basically same.

These scripts provide start/stop or check for the following services:
-	SAPInstance (Windows & Linux)	SAP system instance
-	WINService (Windows)	Windows service
-	WINCluster (Windows)	local cluster
-	HANACluster (Linux)	HANA Cluster

But following the framework within the scripts additional services can be added

## Configuration file
The script reads a configuration file stored in the same directory as the script and called *hostname*.conf. This configuration file describes the applications and services running on this host.<br/>

For example:

```
# File $hostname.conf

WINService -Name "SQL Server (MSSQLSERVER)"
WINService -Name "SQL Server Agent (MSSQLSERVER)"
WINService -Name "Server Intelligence Agent (N24QASDZQA01)"
WINCluster -Roles "SAPSIDROLE", "ANOTHERROLE"
SAPInstance -Dir F:\usr\sap\XYZ\ASCS01 -User xydadm -Pass "blahblah"
SAPInstance -Dir F:\usr\sap\XYZ\DVEBMGS00 -User xydadm -Pass "blahblah" -Grace 60
```
Services are processed from top to bottom (ascending) on Start and bottom to top (descending) on Stop<br/>
Calling ```azcontrol.ps1 Start``` will Start the 3 Windows Services first, then the Local Cluster and then SAP ASCS instance and then app instance<br/>
Calling ```azcontrol.ps1 Stop``` will stop SAP app instance, then ASCS, then local Cluster and then each Windows Service in turn<br/>

Example 2
```
# File $hostname.conf

HANACluster /usr/sap/DEV/HDB00
SAPInstance /usr/sap/XYZ/DVEBMGS69
```
Services are process from top to bottom (ascending) on Start and bottom to top (descending) on Stop<br/>
Calling ```azcontrol.sh stop``` will stop SAP Instance and then shutdown the HANA cluster<br/>
Calling ```azcontrol.sh start``` will start HANA cluster and then start SAP Instance<br/>

## Implementation
Best thing to do is to have a fiddle around with the scripts and see if they are suitable or do what you require. Hopefully the code is pretty easy and self explanatory and you can use or adapt them for you requirements.

The framework basically boils down to:

The main control section calling functions:

start--whatever

stop--whaterver

check--whatever

additional functions can be added if required.


