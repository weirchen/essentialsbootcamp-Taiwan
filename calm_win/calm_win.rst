.. _calm_win:

-----------------------
Calm: Windows 工作負載
-----------------------

*完成本實作的估計時間為60分鐘。*

簡介
++++++++

在本練習中，您將通過構建和部署可安裝和配置多層的藍圖，探索在Nutanix Calm中使用Windows工作負載的基礎知識。 本實作假設您熟悉基本的Calm功能或已完成 :ref:`calm_linux` **lab.**

創建藍圖
++++++++++++++++++++++

#. 在Calm中，創建一個新的 **Multi VM/Pod Blueprint**.

#. 填寫以下欄位，然後按一下 **Proceed** 啟動藍圖編輯器：

   - **Name** - *Initials*-CalmWindowsIntro
   - **Description** - [BugNET](\http://@@{MSIIS.address}@@/bugnet)
   - **Project** - *Initials*-Calm

   .. note::

     使用提供的描述值將創建指向BugNET應用程式的超連結，以在部署完成後啟動。

#. 點擊 **Credentials** 並創建以下兩個憑證：

   +---------------------+---------------------+---------------------+
   | **Credential Name** | WIN_VM_CRED         | SQL_CRED            |
   +---------------------+---------------------+---------------------+
   | **Username**        | Administrator       | Administrator       |
   +---------------------+---------------------+---------------------+
   | **Secret Type**     | Password            | Password            |
   +---------------------+---------------------+---------------------+
   | **Password**        | nutanix/4u          | Str0ngSQL/4u$       |
   +---------------------+---------------------+---------------------+

   .. figure:: images/credentials.png

#. 點擊 **Save** 並返回 Blueprint Editor.

#. 點擊 **Configuration** 並創建以下 **Downloadable Image Configuration**:

   - **Package Name** - MSSQL2014_ISO
   - **Description** - Microsoft SQL 2014 Installation ISO
   - **Image Name** - MSSQL2014.iso
   - **Image Type** - ISO Image
   - **Architecture** - X86_64
   - **Source URI** - http://download.microsoft.com/download/7/9/F/79F4584A-A957-436B-8534-3397F33790A6/SQLServer2014SP3-FullSlipstream-x64-ENU.iso
   - **Product Name** - MSSQL
   - **Product Version** - 2014
   - **Checksum Algorithm** - *Leave blank*
   - **Checksum Value** - *Leave blank*

   .. figure:: images/downloadable_image_config.png

#. 點擊 **Save** 然後返回到Blueprint Editor.

#. 使用 **Default** 應用程式設定檔，指定以下內容 **Variables** 到 **Configuration Panel**:

   +---------------------+---------------+----------------+---------------+---------------+
   | **Name**            | **Data Type** | **Value**      | **Secret**    | **Runtime**   |
   +=====================+===============+=================+==============+===============+
   | DbName              | String        | BugNET         | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | DbUsername          | String        | BugNETUser     | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | DbPassword          | String        | Nutanix/4u$    | Yes           | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | User_initials       | String        | *Leave blank*  | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+

   .. figure:: images/variables.png

#. 點擊 **Save**.

添加 Services
+++++++++++++++

#. 在 **Application Overview > Services**下, 點擊 :fa:`plus-circle` 兩次以加入兩個新Service。

   .. figure:: images/create_service.png

#. 使用下表填寫每個服務的**VM**欄位：

   +------------------------------+---------------------------+---------------------------+
   | **Service Name**             | **MSSQL**                 | **MSIIS**                 |
   +------------------------------+---------------------------+---------------------------+
   | **Name**                     | MSSQL2014                 | MSIIS8                    |
   +------------------------------+---------------------------+---------------------------+
   | **Cloud**                    | Nutanix                   | Nutanix                   |
   +------------------------------+---------------------------+---------------------------+
   | **Operating System**         | Windows                   | Windows                   |
   +------------------------------+---------------------------+---------------------------+
   | **VM Name**                  | @@{User_initials}@@-MSSQL | @@{User_initials}@@-MSIIS |
   +------------------------------+---------------------------+---------------------------+
   | **Number of Images**         | 2                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **Image 1**                  | Windows2012R2             | Windows2012R2             |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type 1**            | DISK                      | DISK                      |
   +------------------------------+---------------------------+---------------------------+
   | **Device Bus 1**             | SCSI                      | SCSI                      |
   +------------------------------+---------------------------+---------------------------+
   | **Bootable 1**               | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Image 2**                  | MSSQL2014_ISO             | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type 2**            | CD-ROM                    | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Device Bus 2**             | IDE                       | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Bootable 2**               | No                        | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **vCPUs**                    | 2                         | 2                         |
   +------------------------------+---------------------------+---------------------------+
   | **Cores per vCPU**           | 2                         | 2                         |
   +------------------------------+---------------------------+---------------------------+
   | **Memory (GiB)**             | 6                         | 6                         |
   +------------------------------+---------------------------+---------------------------+
   | **Guest Customization**      | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Type**                     | Sysprep                   | Sysprep                   |
   +------------------------------+---------------------------+---------------------------+
   | **Install Type**             | Prepared                  | Prepared                  |
   +------------------------------+---------------------------+---------------------------+
   | **Script**                   | *Copy script below table* | *Copy script below table* |
   +------------------------------+---------------------------+---------------------------+
   | **Additional vDisks**        | 1                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type**              | DISK                      | DISK                      |
   +------------------------------+---------------------------+---------------------------+
   | **Device Buse**              | SCSI                      | SCSI                      |
   +------------------------------+---------------------------+---------------------------+
   | **Size (GiB)**               | 100                       | 100                       |
   +------------------------------+---------------------------+---------------------------+
   | **VGPUs**                    | None                      | None                      |
   +------------------------------+---------------------------+---------------------------+
   | **Categories**               | None                      | None                      |
   +------------------------------+---------------------------+---------------------------+
   | **Network Adapters**         | 1                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **NIC 1**                    | Primary                   | Primary                   |
   +------------------------------+---------------------------+---------------------------+
   | **Check log-in upon create** | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Credential**               | WIN_VM_CRED               | WIN_VM_CRED               |
   +------------------------------+---------------------------+---------------------------+
   | **Address**                  | NIC 1                     | NIC 1                     |
   +------------------------------+---------------------------+---------------------------+
   | **Connection Type**          | Windows (Powershell)      | Windows (Powershell)      |
   +------------------------------+---------------------------+---------------------------+
   | **Connection Port**          | 5985                      | 5985                      |
   +------------------------------+---------------------------+---------------------------+
   | **Delay (in seconds)**       | Increase to **90**        | Increase to **90**        |
   +------------------------------+---------------------------+---------------------------+

   .. code-block:: XML
     :caption: Sysprep Script

     <?xml version="1.0" encoding="UTF-8"?>
     <unattend xmlns="urn:schemas-microsoft-com:unattend">
       <settings pass="specialize">
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <ComputerName>@@{name}@@</ComputerName>
             <RegisteredOrganization>Nutanix</RegisteredOrganization>
             <RegisteredOwner>Acropolis</RegisteredOwner>
             <TimeZone>UTC</TimeZone>
          </component>
          <component xmlns="" name="Microsoft-Windows-TerminalServices-LocalSessionManager" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">
             <fDenyTSConnections>false</fDenyTSConnections>
          </component>
          <component xmlns="" name="Microsoft-Windows-TerminalServices-RDP-WinStationExtensions" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">
             <UserAuthentication>0</UserAuthentication>
          </component>
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Networking-MPSSVC-Svc" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <FirewallGroups>
                <FirewallGroup wcm:action="add" wcm:keyValue="RemoteDesktop">
                   <Active>true</Active>
                   <Profile>all</Profile>
                   <Group>@FirewallAPI.dll,-28752</Group>
                </FirewallGroup>
             </FirewallGroups>
          </component>
       </settings>
       <settings pass="oobeSystem">
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <UserAccounts>
                <AdministratorPassword>
                   <Value>@@{WIN_VM_CRED.secret}@@</Value>
                   <PlainText>true</PlainText>
                </AdministratorPassword>
             </UserAccounts>
             <AutoLogon>
                <Password>
                   <Value>@@{WIN_VM_CRED.secret}@@</Value>
                   <PlainText>true</PlainText>
                </Password>
                <Enabled>true</Enabled>
                <Username>Administrator</Username>
             </AutoLogon>
             <FirstLogonCommands>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>cmd.exe /c netsh firewall add portopening TCP 5985 "Port 5985"</CommandLine>
                   <Description>Win RM port open</Description>
                   <Order>1</Order>
                   <RequiresUserInput>true</RequiresUserInput>
                </SynchronousCommand>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>powershell -Command "Enable-PSRemoting -SkipNetworkProfileCheck -Force"</CommandLine>
                   <Description>Enable PS-Remoting</Description>
                   <Order>2</Order>
                   <RequiresUserInput>true</RequiresUserInput>
                </SynchronousCommand>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>powershell -Command "Set-ExecutionPolicy -ExecutionPolicy RemoteSigned"</CommandLine>
                   <Description>Enable Remote-Signing</Description>
                   <Order>3</Order>
                   <RequiresUserInput>false</RequiresUserInput>
                </SynchronousCommand>
             </FirstLogonCommands>
             <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <SkipMachineOOBE>true</SkipMachineOOBE>
             </OOBE>
          </component>
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-International-Core" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <InputLocale>en-US</InputLocale>
             <SystemLocale>en-US</SystemLocale>
             <UILanguageFallback>en-us</UILanguageFallback>
             <UILanguage>en-US</UILanguage>
                <UserLocale>en-US</UserLocale>
          </component>
       </settings>
     </unattend>

   花一點時間查看Sysprep腳本。 您可以看到配置為使用WIN_VM_CRED密碼自動登錄到本地Administrator帳戶的VM。 雖然此練習不會將VM加入到Active Directory域中，但是您可以使用Sysprep或Package Install任務腳本來自動加入域。

    此外，防火牆已配置為允許埠5985（Calm用於對主機執行PowerShell腳本）。 對於熟悉Calm早期版本的使用者，不再需要 **Karan** 服務VM才能將PowerShell命令代理到服務VM。 相反，Calm引入了對在遠端主機上運行PowerShell腳本的本機支援。

    與:ref:`calm_linux` 實作中工作管理員中的應用類似, 您想要確保資料庫在IIS Web伺服器設置之前可用。

#. 在Blueprint Editor, 選擇 **MSIIS** 服務並創建對 **MSSQL** service的相依性關係。

   .. figure:: images/services.png

定義 Package Install
++++++++++++++++++++++++

對於以下7個腳本中的 **每個**腳本（對於MSSSQL為3個腳本，對於MSIIS為4個腳本），欄位將相同：

- **Type** - Execute
- **Script Type** - PowerShell
- **Credential** - WIN_VM_CRED

.. note::

  如果您使用的是加入網域的VM，則在將VM加入網域之後，將需要單獨的網域憑據來執行PowerShell腳本。

#. 選擇 **MSSQL** 服務 在 **Configuration Panel**打開**Package**。

#. 為套裝軟體命名，然後按一下**Configure install**以開始添加安裝任務。

   您將添加多個腳本來完成每個安裝。 使用多個腳本可以使用Calm **Task Library**簡化跨多個服務或藍圖的代碼維護和應用。 任務庫允許您創建模組化腳本來實現某些常用功能，例如加入域或配置常用OS設置。

#. 在 **MSSQL > Package Install**下, 點擊 **+ Task** 並填寫以下欄位：
   - **Task Name** - InitializeDisk1
   - **Script** -

   .. code-block:: powershell

     Get-Disk -Number 1 | Initialize-Disk -ErrorAction SilentlyContinue
     New-Partition -DiskNumber 1 -UseMaximumSize -AssignDriveLetter -ErrorAction SilentlyContinue | Format-Volume -Confirm:$false

   上面的腳本僅執行在服務的VM配置期間添加的額外100GB VDisk的初始化和格式。

#. 點擊 **Publish To Library > Publish** 將此任務腳本保存到任務庫中以備將來使用。

#. 重複點擊 **+ Task** 添加其餘兩個腳本：

   - **Task Name** - InstallMSSQL
   - **Script** -

   .. code-block:: powershell

     $DriveLetter = $(Get-Partition -DiskNumber 1 -PartitionNumber 2 | select DriveLetter -ExpandProperty DriveLetter)
     $edition = "Standard"
     $HOSTNAME=$(hostname)
     $PackageName = "MsSqlServer2014Standard"
     $Prerequisites = "Net-Framework-Core"
     $silentArgs = "/IACCEPTSQLSERVERLICENSETERMS /Q /ACTION=install /FEATURES=SQLENGINE,SSMS,ADV_SSMS,CONN,IS,BC,SDK,BOL /SECURITYMODE=sql /SAPWD=`"@@{SQL_CRED.secret}@@`" /ASSYSADMINACCOUNTS=`"@@{SQL_CRED.username}@@`" /SQLSYSADMINACCOUNTS=`"@@{SQL_CRED.username}@@`" /INSTANCEID=MSSQLSERVER /INSTANCENAME=MSSQLSERVER /UPDATEENABLED=False /INDICATEPROGRESS /TCPENABLED=1 /INSTALLSQLDATADIR=`"${DriveLetter}:\Microsoft SQL Server`""
     $setupDriveLetter = "D:"
     $setupPath = "$setupDriveLetter\setup.exe"
     $validExitCodes = @(0)

     if ($Prerequisites){
     Install-WindowsFeature -IncludeAllSubFeature -ErrorAction Stop $Prerequisites
     }

     Write-Output "Installing $PackageName...."

     $install = Start-Process -FilePath $setupPath -ArgumentList $silentArgs -Wait -NoNewWindow -PassThru
     $install.WaitForExit()

     $exitCode = $install.ExitCode
     $install.Dispose()

     Write-Output "Command [`"$setupPath`" $silentArgs] exited with `'$exitCode`'."
     if ($validExitCodes -notcontains $exitCode) {
     Write-Output "Running [`"$setupPath`" $silentArgs] was not successful. Exit code was '$exitCode'. See log for possible error messages."
     exit 1
     }

   查看上面的腳本，您可以看到它正在執行SQL Server的自動安裝，使用SQL_CRED憑據詳細資訊，並使用額外的100GB VDisk存放SQL資料檔案。

   根據Nutanix生產資料庫部署的最佳做法，還需要在VM /安裝中添加哪些內容？

   - **Task Name** - FirewallRules
   - **Script** -

   .. code-block:: powershell

     New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action allow
     New-NetFirewallRule -DisplayName "SQL Admin Connection" -Direction Inbound -Protocol TCP -LocalPort 1434 -Action allow
     New-NetFirewallRule -DisplayName "SQL Database Management" -Direction Inbound -Protocol UDP -LocalPort 1434 -Action allow
     New-NetFirewallRule -DisplayName "SQL Service Broker" -Direction Inbound -Protocol TCP -LocalPort 4022 -Action allow
     New-NetFirewallRule -DisplayName "SQL Debugger/RPC" -Direction Inbound -Protocol TCP -LocalPort 135 -Action allow
     New-NetFirewallRule -DisplayName "SQL Browser" -Direction Inbound -Protocol TCP -LocalPort 2382 -Action allow

   查看上面的腳本，您可以看到它允許通過Windows防火牆進行關鍵SQL服務的入站存取。

    完成後，您的MSSQL服務應如下所示：

   .. figure:: images/mssql_package_install.png

#. 選擇 **MSIIS**服務，然後在 **Configuration Panel**中打開 **Package**選項卡。

#. 為套裝軟體命名，然後按一下 **Configure install**以開始添加安裝任務。

#. 在 **MSSQL > Package Install**下, 按一下 **+ Task**.

#. 與安裝MSSQL服務的第一步類似，您將需要初始化並格式化其他100GB VDisk。 按一下而不是為此任務手動指定相同的腳本，請按一下 **Browse Library**.

#. 選擇 **InitializeDisk1** 您先前發佈的任務，然後按一下 **Select > Copy**.

   .. figure:: images/task_library.png

   .. note::

     如果發佈的任務中存在Calm宏，則任務庫還使您能夠提供變數定義。

#. 指定 **Name** 和 **Credential**, 然後重複點擊 **+ Task** 添加其餘三個腳本：

   - **Task Name** - InstallWebPI
   - **Script** -

   .. code-block:: powershell

     # Install WPI
     New-Item c:/msi -Type Directory
     Invoke-WebRequest 'http://download.microsoft.com/download/C/F/F/CFF3A0B8-99D4-41A2-AE1A-496C08BEB904/WebPlatformInstaller_amd64_en-US.msi' -OutFile c:/msi/WebPlatformInstaller_amd64_en-US.msi
     Start-Process 'c:/msi/WebPlatformInstaller_amd64_en-US.msi' '/qn' -PassThru | Wait-Process
     cd 'C:/Program Files/Microsoft/Web Platform Installer'; .\WebpiCmd.exe /Install /Products:'UrlRewrite2,ARRv3_0' /AcceptEULA /Log:c:/msi/WebpiCmd.log

   上面的腳本將安裝Microsoft Web Platform Installer（WebPI），該WebPI用於下載，安裝和更新Microsoft Web Platform的元件，包括Internet資訊服務（IIS），IIS媒體平臺技術，SQL Server Express，.NET Framework 和Visual Web Developer。

   - **Task Name** - InstallNetFeatures
   - **Script** -

   .. code-block:: powershell

     # Enable Repair via Windows Update
     $servicing = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Servicing"
     New-Item -Path $servicing -Force
     Set-ItemProperty -Path $servicing -Name RepairContentServerSource -Value 2

     # Install Features
     Install-WindowsFeature -Name NET-Framework-Core
     Install-WindowsFeature -Name NET-WCF-Services45 -IncludeAllSubFeature

   上面的腳本在VM上安裝.NET Framework 4.5。

   - **Task Name** - InstallBugNetApp
   - **Script** -

   .. code-block:: powershell

     # Create the installation configuration file
     $configFile = "AppPath[@]Default Web Site/bugnet
     DbServer[@]@@{MSSQL.address}@@
     DbName[@]@@{DbName}@@
     DbUsername[@]@@{DbUsername}@@
     Database Password[@]@@{DbPassword}@@
     DbAdminUsername[@]sa
     DbAdminPassword[@]@@{SQL_CRED.secret}@@"

     echo $configFile >> BugNET0.app

     # Install the application via Web PI
     WebpiCmd-x64.exe /Install /UseRemoteDatabase /Application:BugNET@BugNET0.app /AcceptEula

   上面的腳本使用您在練習開始時定義的Application Profile變數來填充Bug Tracker應用程式的設定檔。 然後，它利用WebPI從以下位置安裝應用程式： `Microsoft Web App Gallery <https://webgallery.microsoft.com/gallery>`_. 只需進行最小的更改，您就可以利用Gallery中許多受歡迎的應用程式，包括CMS，電子商務，Wiki，票務等應用程式。

   完成後，您的MSIIS服務應如下所示：

   .. figure:: images/msiis_package_install.png

#. 按一下 **Save**.

運行藍圖
+++++++++++++++++++++++

#. 在藍圖編輯器的上方工具列中，按一下 **Launch**.

#. 指定唯一 **Application Name** (e.g. *Initials*\ -BugNET) and your **User_initials** Runtime為VM命名的變數值。

#. 按一下 **Create**.

   **Audit** 選項卡可用于監視應用程式的部署。 該應用程式大約需要20分鐘才能部署。

#. 一旦“創建”操作完成，並且應用程式處於 **Running**狀態，請在新選項卡中打開 **BugNET**連結。

   .. figure:: images/bugnet_link.png

#. 系統將顯示 **Installation Status Report**頁面。 等待它報告“安裝完成” **Installation Complete**，然後按一下底部的連結以存取該應用程式。
   .. figure:: images/bugnet_setup.png

  恭喜！ 現在，您有了一個功能齊全的錯誤跟蹤應用程式，可以利用Microsoft SQL Server和IIS自動進行配置。

   .. figure:: images/bugnet_app.png

(Optional) Scale Out IIS Tier
+++++++++++++++++++++++++++++

Leveraging the same approach from the :ref:`calm_linux` lab of having multiple web server replicas, can you add a CentOS based HAProxy service to this blueprint to allow for load balancing across multiple IIS servers?

(Optional) 通過Era管理 MSSQL 
++++++++++++++++++++++++++++++++++

完成 :ref:`Era` 實作室，對Era的功能和操作有基本的瞭解。

使用預設憑據（ ** admin / password **）登錄到BugNET應用程式，然後按照嚮導創建一個新專案。

您剛剛部署了生產BugNET應用程式，現在希望使用最新的可用生產資料快速部署多個開發/測試實例。

您是否可以構建一個利用SQL Server資料庫的Era實體複本版本的藍圖？

 **提示**

- 首先實體複本您現有的藍圖！
- 在Era中註冊SQL Server來源資料庫時，此部署使用預設的MSSQLServer實例名稱。 您可以使用Windows身份驗證通過WIN_VM_CRED憑據存取SQL Server實例。
- 在“靜默”中添加服務時，其中一種“雲”類型使用的是“現有VM”。 現有的VM僅需要VM的IP位址和登錄憑據。
- 實體複本時，Windows Server 2012 R2 VM的Windows許可證金鑰為``W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9''。
- 您可以使用半自動方法，其中對實體複本的資料庫IP使用** Runtime **變數。 在這種情況下，您將創建來源資料庫的實體複本，等待它返回IP地址，並在運行時為藍圖提供指定的IP。
- 您可以使用完全自動化的方法，在其中為“現有VM”創建“套裝軟體安裝任務”。 該任務可以執行對Era的API調用以啟動資料庫實體複本操作並返回IP位址。

-不要忘記相依性項！

概要總結
+++++++++

-Calm為Windows工作負載提供了與Linux工作負載相同的應用程式部署和生命週期管理優勢。

-Calm可以在Windows終結點上本地執行遠端PowerShell腳本，而無需基於Windows的代理程式。

.. |projects| image:: images/projects.png
