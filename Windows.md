#windows

## Batch rename multiple files

Program -  Bulk rename utility

Put files in logical order
`Menu - Display Options - Sorting - Logical sorting`
File name style: image-***
`Name(2) - Name: Fixed, 'image-'`
`Numbering(10) - Mode: Suffix; Pad: 3`
___

## Windows 11 without Microsoft Account

```powershell
netsh interface show interface
netsh interface set interface "Wi-Fi" disable
oobe\bypassnro
```
___

## Prevent Windows from automatically adding keyboard layouts 

```powershell
reg delete "HKEY_USERS\.DEFAULT\Keyboard Layout\Preload" /f
```

[How to prevent Windows 10 from automatically adding keyboard layouts (i.e. US keyboard)](https://superuser.com/questions/1092246/how-to-prevent-windows-10-from-automatically-adding-keyboard-layouts-i-e-us-ke)
___

## Загрузка, установка Office 2024 LTSC

![[Как загрузить, установить Office 2024 LTSC с сайта Microsoft и активировать навсегда.Хабр.mhtml]]
___


