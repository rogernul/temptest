# h

***
## w11
### old pc
code: 
~~unzip and rename Appraiserres.dll~~

~~VirtualBox Guest Additions bypass reg~~

REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassTPMCheck /t REG_DWORD /d 1
REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassSecureBootCheck /t REG_DWORD /d 1
REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassRAMCheck /t REG_DWORD /d 1
REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassStorageCheck /t REG_DWORD /d 1
REG ADD HKLM\SYSTEM\Setup\LabConfig /v BypassCPUCheck /t REG_DWORD /d 1


### oobe
code: 
Shift+F10
oobe\BypassNRO.cmd

@echo BypassNRO.cmd
@echo off
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\OOBE /v BypassNRO /t REG_DWORD /d 1 /f
shutdown /r /t 0

ctrl+shift+F3 bypass
alt+F4

Ctrl + Shift + Esc task manager

### bypass create account
start ms-cxh:localonly

### kms
show
slmgr /dlv

act
185.213.174.199	labcfg
slmgr /skms labcfg
slmgr /ato

remove
slmgr /upk

***
## Chrome
https://www.google.cn/intl/zh-CN/chrome/

***
## o 365
https://officecdn.microsoft.com/db/492350F6-3A01-4F97-B9C0-C7C6DDF67D60/media/zh-CN/O365ProPlusRetail.img
https://officecdn.microsoft.com/db/492350F6-3A01-4F97-B9C0-C7C6DDF67D60/media/en-US/O365ProPlusRetail.img

@echo off
cd /d "%ProgramFiles%\Microsoft Office\Office16"
cscript ospp.vbs /inslic:..\root\Licenses16\MondoVL_KMS_Client-ppd.xrm-ms
cscript ospp.vbs /inslic:..\root\Licenses16\MondoVL_KMS_Client-ul-oob.xrm-ms
cscript ospp.vbs /inslic:..\root\Licenses16\MondoVL_KMS_Client-ul.xrm-ms
cscript ospp.vbs /inpkey:HFTND-W9MK4-8B7MJ-B6C4G-XQBR2
cscript ospp.vbs /sethst:labcfg
cscript ospp.vbs /act
exit


# l
