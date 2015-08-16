Run a PowerShell script

There are several ways to run a PowerShell script.

Before running any scripts on a new PowerShell installation, you must first set an appropriate Execution Policy, e.g. Set-ExecutionPolicy RemoteSigned

A PowerShell script is the equivalent of a Windows CMD or MS-DOS batch file, the file should be saved with a .ps1 extension, e.g. MyScript.ps1

The most common (default) way to run a script is by calling it:

PS C:\> & "C:\Belfry\My first Script.ps1"

If the path does not contain any spaces, then you can omit the quotes and the '&' operator

PS C:\> C:\Belfry\Myscript.ps1

If the script is in the current directory, you must indicate this using .\ (or ./ will also work)

PS C:\> .\Myscript.ps1

Run with elevated permissions

Some PowerShell cmdlets and Windows commands such as REG ADD and SUBINACL have to be run from an elevated prompt, there are several ways of doing this.

It is possible to right click Powershell.exe (or it's Start menu shortcut) and run it As Admin.
Shortcuts can be edited to always run as Admin - Properties | Shortcut | Advanced then tick "Run as administrator".

To elevate a script from a (non-elevated) PowerShell command line :

PS C:\> Start-Process powershell -ArgumentList '-noprofile -file MyScript.ps1' -verb RunAs

A set of commands can also be saved in a scriptblock variable, and then passed to a new (elevated) PowerShell session:

Start-Process -FilePath powershell.exe -ArgumentList $code -verb RunAs -WorkingDirectory C:

To run an entire PowerShell session 'As Admin' from an existing PowerShell (non-elevated) session:

PS> Start-Process powershell.exe -Verb runAs
Testing for Elevation

To ensure a script is only run As Admin, add this to the beginning

If (-NOT ([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator"))
 {    
  Echo "This script needs to be run As Admin"
  Break
 }

In PowerShell v4.0 the above can be simplified by using a #Requires statement:
#Requires -RunAsAdministrator
