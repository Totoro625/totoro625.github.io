---
title: 未闻花名KMS服务器
date: 2017-10-15 14:38:10
update: 2018-02-27 14:54:00
tags: network
category: bash
---

本服务器采用[vlmcsd](https://github.com/Wind4/vlmcsd)搭建，由[dakkidaze](https://github.com/dakkidaze)提供的[脚本](https://github.com/dakkidaze/one-key-kms)进行管理。
服务器可以激活几乎全部的Win操作系统以及Office软件。

------

各种不同版本的KMS密钥请前往[这里](https://technet.microsoft.com/en-us/library/jj612867.aspx)查看。
Win10家庭版用户可以使用VK7JG-NPHTM-C97JM-9MPGT-3V66T升级至专业版本。
<!--more-->
------

### [未闻花名](https://snrat.com/8.html)的KMS

Win激活方法：
1、以管理员身份打开命令提示符【如`win+R` 输入`cmd`】
2、在命令提示符下输入 `slmgr /ipk [对应版本的KMS密钥]`
3、在命令提示符下输入 `slmgr /skms kms.snrat.com`
4、在命令提示符下输入 `slmgr /ato`

------

Office激活方法：
1、以管理员模式运行命令提示符【如`win+R` 输入`cmd`】
2、在命令提示符下 进入Office 文件 `ospp.vbs` 所在的目录【如Office 2013 默认文件位置`C:\Program Files\Microsoft Office\Office15`则输入`cd "C:\Program Files\Microsoft Office\Office15"`】
3、在命令提示符下输入` cscript ospp.vbs /inpkey:[对应Office产品GVLK序列号]`
4、在命令提示符下输入` cscript ospp.vbs /sethst:kms.snrat.com`
5、在命令提示符下输入 `cscript ospp.vbs /act`

## 我的KMS

同时我根据其方法搭建了自己的一份KMS服务器

将其激活服务器`kms.snrat.com`改为我的`kms.totoro.ink`即可使用

同时写了一份[bat脚本](to/totoro_kms.bat)，可以激活Windows与office（注：仅可激活64位office）

```
@echo off

:: BatchGotAdmin
:-------------------------------------
REM  --> Check for permissions
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

REM --> If error flag set, we do not have admin.
if '%errorlevel%' NEQ '0' (
    echo Requesting administrative privileges...
    goto UACPrompt
) else ( goto gotAdmin )

:UACPrompt
    echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
    set params = %*:"=""
    echo UAC.ShellExecute "cmd.exe", "/c %~s0 %params%", "", "runas", 1 >> "%temp%\getadmin.vbs"

    "%temp%\getadmin.vbs"
    del "%temp%\getadmin.vbs"
    exit /B

:gotAdmin
    pushd "%CD%"
    CD /D "%~dp0"
:--------------------------------------

color 2F
echo.
echo.
echo.1.Office 2010 激活
echo.
echo.2.Office 2013 激活
echo.
echo.3.Office 2016 激活
echo.
echo.4.Windows 激活
echo.
echo.
set KMS_Server=kms.snrat.com
set /p c=请输入数字并回车:
if %c%==1 goto 1
if %c%==2 goto 2
if %c%==3 goto 3
if %c%==4 goto 4
:office
setlocal EnableDelayedExpansion
reg query %strRegKey% >nul 2>nul
if %errorlevel%==0 (set strCurrentKey=%strRegKey%) else (set strCurrentKey=%strRegKey6432%)
for /f "delims=" %%i in ('reg query %strCurrentKey%') do (
set strInstPath=%%i
set strInstPath=!strInstPath:*REG_SZ=!
)
:LTrim
if "%strInstPath:~0,1%"==" " set "strInstPath=%strInstPath:~1%" && goto LTrim
:RTrim
if "%strInstPath:~-1%"==" " set "strInstPath=%strInstPath:~0,-1%" && goto RTrim
if "%strInstPath:~-1%" neq "\" set strInstPath=%strInstPath%\
echo office安装目录为%strInstPath% 
cd /d %strInstPath%
cscript ospp.vbs /sethst:%KMS_Server%
cscript ospp.vbs /act
pause
exit

:1
set "strRegKey=HKEY_LOCAL_MACHINE\Software\Microsoft\Office\14.0\Common\InstallRoot /v Path"
set "strRegKey6432=HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Office\14.0\Common\InstallRoot /v Path"
goto office

:2
set "strRegKey=HKEY_LOCAL_MACHINE\Software\Microsoft\Office\15.0\Common\InstallRoot /v Path"
set "strRegKey6432=HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Office\15.0\Common\InstallRoot /v Path"
goto office

:3
set "strRegKey=HKEY_LOCAL_MACHINE\Software\Microsoft\Office\16.0\Common\InstallRoot /v Path"
set "strRegKey6432=HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Office\16.0\Common\InstallRoot /v Path"
goto office


:4
cscript "%SystemRoot%\system32\slmgr.vbs" /skms %KMS_Server%
cscript "%SystemRoot%\system32\slmgr.vbs" -ato
pause
exit
```