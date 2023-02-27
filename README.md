# Information for install BigFix and ILMT
Author: WB  
Date: April 26, 2021
___
This document guide you to install BigFix and IBM License Metric Tool (ILMT) in windows server. For more information about ILMT please click [here](https://www.ibm.com/software/passportadvantage/ibmlicensemetrictool.html)
## Prerequirsite
- Windows Server 2019
  - Enable .Net framework 3.5 
- BigFix Installation file
- ILMT license file (Free generate)
- MSSQL Express Edition (Free download) **Basic is enough

# Step to Install
1. Allow flash player can run in the server.  
Open command terminal as administrator then copy the code and paste into terminal. Press Enter.
```
## Windows Server 2019
dism /online /add-package /packagepath:"C:\Windows\servicing\Packages\Adobe-Flash-For-Windows-Package~31bf3856ad364e35~amd64~~10.0.17763.1.mum"
```
```
## Windows Server 2016
dism /online /add-package /packagepath:"C:\Windows\servicing\Packages\Adobe-Flash-For-Windows-Package~31bf3856ad364e35~amd64~~10.0.14393.0.mum"
```

1. Install MSSQL database  
  Install MSSQL database into your machine. You need to install in mixed mode authentication (Mixed mode is allow you can login to your database with Windows user or Database user.) and allow firewall for port 1433 both TCP/UDP protocal, and enable TCP connection and set all network port to 1433 (You can set it with Server Configuration Manager.)
2. Install SQL Server Management Studio (SSMS) 
3. Login to MSSQL with windows authentication and then change password of user sa. (You can choose your custom password as you need.)
4. Install BigFix.
5. Activate all modules in bigfix cconsole.
6. Choose All computer feature in bigfix console.
7. Download ILMT installation file from bigfix console.
8. Go to bigfix download file path, Extract ILMT installation file.
9. Install ILMT from installation bat file, And follow the wizard step.
10. Deploy client agent all of you need.

# Resource
## BigFix download page
https://support.bigfix.com/bes/release/10.0/patch2/

## ILMT generate license page
https://www.ibm.com/support/pages/ilmt-authorization-file-request

> **_NOTE:_** You need to login with IBM ID before generate the license
```

```

## Download MSSQL Express Edition
You can download MSSQL Express Edition for free from below this url. In this document use MSSQL Express 2019 Basic.
> **_NOTE:_** You can download other version [Here!](https://www.hanselman.com/blog/download-sql-server-express) or [Here!](https://stackoverflow.com/questions/39835986/sql-server-2016-2017-and-2019-express-full-download)

### **_SQL Server 2019 Express Edition (English):_**
> - Basic (239 MB): https://download.microsoft.com/download/7/c/1/7c14e92e-bdcb-4f89-b7cf-93543e7112d1/SQLEXPR_x64_ENU.exe
> - Advanced (700 MB): https://download.microsoft.com/download/7/c/1/7c14e92e-bdcb-4f89-b7cf-93543e7112d1/SQLEXPRADV_x64_ENU.exe
> - LocalDB (53 MB): https://download.microsoft.com/download/7/c/1/7c14e92e-bdcb-4f89-b7cf-93543e7112d1/SqlLocalDB.msi

### **_SQL Server 2017 Express Edition (English):_**
> - Core (275 MB): https://download.microsoft.com/download/E/F/2/EF23C21D-7860-4F05-88CE-39AA114B014B/SQLEXPR_x64_ENU.exe
> - Advanced (710 MB): https://download.microsoft.com/download/E/F/2/EF23C21D-7860-4F05-88CE-39AA114B014B/SQLEXPRADV_x64_ENU.exe
> - LocalDB (45 MB): https://download.microsoft.com/download/E/F/2/EF23C21D-7860-4F05-88CE-39AA114B014B/SqlLocalDB.msi

### **_SQL Server 2016 with SP2 Express Edition (English):_**
> - Core (437 MB): https://download.microsoft.com/download/4/1/A/41AD6EDE-9794-44E3-B3D5-A1AF62CD7A6F/sql16_sp2_dlc/en-us/SQLEXPR_x64_ENU.exe
> - Advanced (1445 MB): https://download.microsoft.com/download/4/1/A/41AD6EDE-9794-44E3-B3D5-A1AF62CD7A6F/sql16_sp2_dlc/en-us/SQLEXPRADV_x64_ENU.exe
> - LocalDB (45 MB): https://download.microsoft.com/download/4/1/A/41AD6EDE-9794-44E3-B3D5-A1AF62CD7A6F/sql16_sp2_dlc/en-us/SqlLocalDB.msi

### **_SQL Server 2016 with SP1 Express Edition (English):_**
> - Core (411 MB): https://download.microsoft.com/download/9/0/7/907AD35F-9F9C-43A5-9789-52470555DB90/ENU/SQLEXPR_x64_ENU.exe
> - Advanced (1255 MB): https://download.microsoft.com/download/9/0/7/907AD35F-9F9C-43A5-9789-52470555DB90/ENU/SQLEXPRADV_x64_ENU.exe
> - LocalDB (45 MB): https://download.microsoft.com/download/9/0/7/907AD35F-9F9C-43A5-9789-52470555DB90/ENU/SqlLocalDB.msi

## Windows registry for mod.
Copy the code and paste into text file, Then save the file as .reg file type. For example file name is "winmod.reg"

```
Windows Registry Editor Version 5.00

; Open command window here as Administrator

[-HKEY_CLASSES_ROOT\Directory\shell\runas]

[HKEY_CLASSES_ROOT\Directory\shell\runas]
@="Command as Administrator"
"HasLUAShield"=""

[HKEY_CLASSES_ROOT\Directory\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""

[-HKEY_CLASSES_ROOT\Directory\Background\shell\runas]

[HKEY_CLASSES_ROOT\Directory\Background\shell\runas]
@="Command as Administrator"
"HasLUAShield"=""

[HKEY_CLASSES_ROOT\Directory\Background\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""

[-HKEY_CLASSES_ROOT\Drive\shell\runas]

[HKEY_CLASSES_ROOT\Drive\shell\runas]
@="Command as Administrator"
"HasLUAShield"=""

[HKEY_CLASSES_ROOT\Drive\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""


; take owner
[HKEY_CLASSES_ROOT\*\shell\runas]
@="Take ownership"
"HasLUAShield"=""
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\*\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"

[HKEY_CLASSES_ROOT\Directory\shell\runas]
@="Take ownership"
"HasLUAShield"=""
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\Directory\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"

; Edit with notepad
[HKEY_CLASSES_ROOT\*\shell\Open with Notepad]

[HKEY_CLASSES_ROOT\*\shell\Open with Notepad\command]
@="notepad.exe %1"
```
