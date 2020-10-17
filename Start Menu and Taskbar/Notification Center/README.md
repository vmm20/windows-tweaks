# Notification Center

This folder contains registry tweaks for modifying the Notification Center, also known as the Action Center, in Windows 10 and Windows Server.

## Toggle Notification Center
![](https://img.shields.io/badge/Windows%2010-blue?style=for-the-badge&logo=windows)
![](https://img.shields.io/badge/Windows%20Server-blue?style=for-the-badge&logo=windows)

These files can completely disable/hide and enable/unhide the notification center for one user or all users.

Note that disabling the notification center will not stop notifications from appearing.  It will only remove the button from the end of the taskbar and prevent notifications from being collected after they are shown.

This is a negative policy, so in the absence of any registry value, the notification center will be enabled.  You may need to use the "unset" files if you want to apply specific changes to individual users, but you need to eliminate the machine-wide policy first.

> **Restart explorer.exe for these changes to take effect.**

| Action | ![](https://img.shields.io/badge/Scope-Machine-orange) | ![](https://img.shields.io/badge/Scope-User-blue) |
| :--: | :--: | :--: |
| Disable | [View](NotificationCenter-machine-disable.reg) | [View](NotificationCenter-user-disable.reg)
| Enable | [View](NotificationCenter-machine-enable.reg) | [View](NotificationCenter-user-enable.reg)
| Unset | [View](NotificationCenter-machine-unset.reg) | [View](NotificationCenter-user-unset.reg)