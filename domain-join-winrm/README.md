# Deploys a Windows VM, joins an existing Domain and configures a WinRM Https listener. It creates a self signed certificate, so no extra certificate is required.

Description of Template
=======================

This template creates a new VM and joins the VM to an existing domain. 

REQUIREMENTS
- Existing domain controller
- Deploy to the resource group, VNET and Subnet of the domain controller
 
This will then configure a WinRM https listener by creating a new test certificate.

The template uses a custom script extension which executes the script 'ConfigureWinRM.ps1' on the target machine.
This script creates a self signed certificate and configures the WinRM Https listener using the certificate's thumbprint.

This template has been tested with Windows Server 2008-R2-SP1, 2012-Datacenter, and 2012-R2-Datacenter.

How to connect to a Target Azure VM post WinRM configuration
============================================================
Use the below script to connect to an azure vm post winrm configuration. Assign the exact fqdn of your azure vm to $hostname.
The script pops up a credential window, provide the credentials of azure vm.

	$hostName=<fqdn-of-vm> # example: "myvm.westus.cloudapp.azure.com"
	$winrmPort = '5986'

	# Get the credentials of the machine
	$cred = Get-Credential

	# Connect to the machine
	$soptions = New-PSSessionOption -SkipCACheck
	Enter-PSSession -ComputerName $hostName -Port $winrmPort -Credential $cred -SessionOption $soptions -UseSSL
