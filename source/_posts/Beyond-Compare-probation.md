---
title: Beyond Compare 试用期 
date: 2024-05-20 09:23:46
tags: 工具
---
# 目录
[前言](#1)
[MacOS 下的解决方案](#2)
[Windows 下的解决方案](#3)

<h1 id="1">前言</h1>

本教程旨在记录笔者是如何解决使用 **Beyond Compare** 时遇到试用期到期问题。<br>
Beyond Compare 软件可以直接在[官网](https://www.scootersoftware.com/download)上下载，笔者自己使用的 Beyond Compare 4。<br>

<h1 id="2">MacOS 下的解决方案</h1>

 第一步，进入 Beyond Compare 软件应用目录，路径如下：<br>

    /Applications/Beyond Compare.app/Contents/MacOS

第二步，修改启动程序文件 BCompare 为 BCompare.real<br>

    mv Bcompare Bcompare.real

第三步，创建一个新的启动程序文件 BCompare<br>

    vi BCompare

文件内容如下：<br>

```SHELL
#!/bin/bash
# 删除注册信息
rm "/Users/$(whoami)/Library/Application Support/Beyond Compare/registry.dat"
# 启动真实的Bcompare文件
"`dirname "$0"`"/BCompare.real $@
```

该脚本的作用时，删除 Beyond Compare 下的注册信息文件，然后再启动真实的 BCompare 启动脚本。<br>
第四步，给新创建的启动脚本权限：<br>

    chmod a+x /Applications/Beyond\ Compare.app/Contents/MacOS/BCompare

完成以上步骤后，打开 Beyond Compare 就可以正常使用了。你可以注意到，每天打开后显示的剩余试用期时间都是满的。<br>

<h1 id="3">Windows 下的解决方案</h1>

第一步，打开注册表<br>
使用 `Win+R` 快捷键打开运行对话框，输入 `regedit` 打开注册表
第二步，找到 Beyond Compare 在注册表中的路径<br>

    计算机\HKEY_CURRENT_USER\SOFTWARE\Scooter Software\Beyond Compare 4

第三步，删除其中的 CacheID 后，重新打开软件，可以看到又有30天的试用期了。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202404090954164.png)
### 添加定时任务
每次等软件到期后再去删除注册表信息很麻烦，可以在计算机管理中添加一个计划程序。<br>
第一步，打开计算机管理<br>
使用 `Win+R` 快捷键打开运行对话框，输入 `compmgmt.msc` 即可打开计算机管理。<br>
第二步，选择任务计划程序库，导入任务<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202404090959396.png)
第三步，将下面 xml 代码保存为 xml 文件，用作第二步中导入任务<br>

```xml
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Date>2023-12-30T10:40:05.8128741</Date>
    <Author>JiacaiGuo</Author>
    <Description>清除Beyond Compare 4 有30天试用期</Description>
    <URI>\Beyond Compare Clear Trial</URI>
  </RegistrationInfo>
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2023-12-30T10:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByMonth>
        <DaysOfMonth>
          <Day>1</Day>
        </DaysOfMonth>
        <Months>
          <January />
          <February />
          <March />
          <April />
          <May />
          <June />
          <July />
          <August />
          <September />
          <October />
          <November />
          <December />
        </Months>
      </ScheduleByMonth>
    </CalendarTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <UserId>S-1-5-21-1882866539-4086113214-3650236343-1001</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>true</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT72H</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Windows\System32\reg.exe</Command>
      <Arguments>delete "hkey_current_user\software\scooter software\beyond compare 4" /v cacheid /f</Arguments>
    </Exec>
  </Actions>
</Task>
```

其中， `<Author>` 、 `<UserId>` 请改为本机信息。可以通过导出其他的本机计划来查看这两个信息。<br>