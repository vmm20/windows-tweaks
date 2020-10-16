# Windows Tweaks

![](https://img.shields.io/badge/license-MIT-blue)

I created this repository to hold all sorts of registry (`.reg`) files that can modify the Windows experience.  This is a work in progress.

Because directly editing the registry can be intimidating and often dangerous, I've put together these files to automatically apply changes when you open them.  I strongly encourage you to inspect these files before you merge them into your registry, just as a best practice.

## Standard Warning

Many of the data in the registry are necessary for Windows to function correctly.  Changing, creating, or deleting registry keys or values improperly may render your system inoperable.

Before making any changes to the registry, please back up all of your personal data and store it on a different device such as a removable drive, or with a cloud storage provider.

## Intended Use

**Warning:** If you're operating in an enterprise environment, do not use any files in this repository.  They might put individual machine policies in conflict with the network.  Use the Group Policy Editor instead, which can sync policy changes across computers through Active Directory.

If you are using a personal computer not connected to an enterprise network, the tweaks in this repository are for you.  Make sure you have administrator rights on your computer before you try any of these.

*If you are using macOS or any Linux distro, the files in this repository are useless for you.  These operating systems have different configuration options you can investigate elsewhere.*

## Windows Registry structure

The Windows registry is a hierarchical database that holds configuration data for the operating system.  At the top level, it is organized into hives, each of which holds a collection of configuration data:
- `HKEY_CLASSES_ROOT`
- `HKEY_CURRENT_USER`
- `HKEY_LOCAL_MACHINE`
- `HKEY_USERS`
- `HKEY_CURRENT_CONFIG`

Each hive contains a collection of keys and subkeys, along with values.  Each value has a name and corresponding data.  Several types of values are available, including strings (`REG_SZ`), integers (`REG_DWORD` and `REG_QWORD`), and others.

It might be helpful to draw some parallels between the registry and the regular Windows filesystem.
- Hives are similar to drives; each of them contains a root directory and a tree of data.
- Keys are comparable to folders; each of them contains subkeys, values, or both, just as a folder can contain subfolders, files, or both.
- Values are analogous to files; each of them has a name, and content.

Although the registry and the filesystem may have similar structures, they serve completely different purposes.  The registry hives ultimately live in the filesystem just like everything else on the computer.  Also, moving files and folders around is as simple as dragging and dropping in the File Explorer.  However, it is not possible to drag and drop registry values and keys into other keys.  To "move" a registry value, you must delete the data from their original location and recreate them in the new location.

## Registry Editor vs. Group Policy Editor

Many of these registry values can also be changed using the Group Policy Editor.  Note that not all configurations in the Group Policy Editor are possible to change using the registry (for example, the Security Settings).  Similarly, not all registry keys have corresponding Group Policy options.

The Group Policy Editor is specifically designed by Microsoft to implement these changes safely and efficiently.  In fact, the Group Policy Editor is simply modifying registry keys behind the scenes.  If you have access to the Group Policy Editor, I recommend it over using these tweaks to directly modify the registry.  However, users who cannot access the Group Policy editor (those who have a Home Edition of Windows) can still change the registry to achieve many of the same configurations.

Be aware that the correspondences between Group Policy objects and registry keys are public knowledge, [published in an Excel spreadsheet on Microsoft's website](https://www.microsoft.com/en-us/download/details.aspx?id=101451).

## Note about user-specific changes
Some of these tweaks are machine-wide changes, while others are per-user changes.  I will do my best to indicate which ones are each type in the file names.

Keep in mind that all the per-user registry keys will be added to the hive `HKEY_CURRENT_USER` (abbreviated `HKCU`).  Most of these modifications to the registry require administrator privileges, when you merge them, you will be running the merge as an administrator.  Therefore, `HKEY_CURRENT_USER` will refer to the administrator account you are using to merge the files.

If you use a standard, non-administrative account as your everyday account, and you click `Run as Administrator` when you open `regedit` or `cmd` to complete the merge, the `HKEY_CURRENT_USER` hive will actually be the administrator account, not the everyday account you're actually using.  Bear this in mind, as the changes you expect might not show up on your standard account.  (If you try to run the registry merge as the standard user, you will most likely get `Access Denied`, or the merge simply won't work.)

If you want to apply tweaks to a specific non-administrative user account, you will need to find that account's `SID`, a unique identifier created by Windows that remains stable throughout the life of the account, even if the username changes.  Note that using registry tweaks in this way is often more flexible than using the Group Policy Editor.

The `SID` has this general format:
```
S-1-5-21-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXXX-YYYY
```
All the digits marked `X` tend to be the same for all user accounts on your computer.  The digits marked `Y` will be different for each user account.  The builtin `Administrator` account has an `SID` ending in `-500`.  Accounts created manually should have `SID`s ending in `-1YYY`.

To find the `SID`s of all user accounts, you can open `cmd.exe` or `powershell.exe`, as any user, and run the following command:
```
wmic useraccount get name, fullname, sid
```
The output will show you a table of user accounts' usernames, full names (if different from usernames), and `SID`s.  Find the SID that you need.

Now, you're ready to apply the changes to a specific user account.  Make a copy of the `.reg` file from this repository, and open the copy in a simple text editor, like `notepad.exe`.  Then, do a global find-and-replace, substituting `HKEY_CURRENT_USER` with
```
HKEY_USERS\S-1-5-21-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXXX-YYYY
```
... of course, replacing the generic `SID` listed above with the one you found for your specific desired account).  Now, you should be ready to merge the registry keys into your computer's registry, and the configuration will be applied to the specific account you want, rather than the administrator account you are using to make the changes.