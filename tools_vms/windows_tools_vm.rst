.. _windows_tools_vm:

----------------
Windows 工具 VM
----------------

簡介
+++++++++

此Windows Server 2012 R2映像檔預裝有許多工具，包括：

- Microsoft Remote Server Administration Tools (RSAT)
- PuTTY, CyberDuck, WinSCP
- Sublime Text 3, Visual Studio Code
- OpenOffice
- Python
- pgAdmin
- Chocolatey Package Manager

如果指示這樣做，請在 ** Lab Setup **中將此虛擬機器部署到您分配的群集上。

.. raw:: html

  <strong><font color="red">Only deploy the VM once, it does not need to be cleaned up as part of any lab completion.</font></strong>

部署工具VM
++++++++++++++++++

在 **Prism Central** > 選擇:fa:`bars` **> Virtual Infrastructure > VMs**, 並按一下 **Create VM**.

填寫以下欄位：

- **Name** - *Initials*-Windows-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 4 GiB

- 選擇 **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - ToolsVM.qcow2
    - 選擇 **Add**

- 選擇 **Add New NIC**
    - **VLAN Name** - Secondary
    - 選擇 **Add**

按一下 **Save** 創建VM.

啟動VM.

使用以下憑據通過RDP或控制台會話登錄到VM：

- **Username** - NTNXLAB\\Administrator
- **password** - nutanix/4u
