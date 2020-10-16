# Windows Tweaks

I created this repository to hold all sorts of registry (`.reg`) files that can modify the Windows experience.  This is a work in progress.

Many of these registry values can be changed in a more user friendly way using the Group Policy Editor, but users with a Home Edition of Windows who cannot access the Group Policy editor can still change the registry to achieve many of the same configurations.

Because directly editing the registry can be intimidating and often dangerous, I've put together these files to automatically apply changes when you open them.

I strongly encourage you to inspect these files before you merge them into your registry, just as a best practice.

## Note about user-specific changes
Some of these tweaks are machine-wide changes, while others are per-user changes.  I will do my best to indicate which ones are each type in the file names.

Keep in mind that all the per-user registry keys will be added to the hive `HKEY_CURRENT_USER` (abbreviated `HKCU`).  Most of these modifications to the registry require administrator privileges, when you merge them, you will be running the merge as an administrator.  Therefore, `HKEY_CURRENT_USER` will refer to the administrator account you are using to merge the files.

If you use a standard, non-administrative account as your everyday account, and you click `Run as Administrator` when you open `regedit` or `cmd` to complete the merge, the `HKEY_CURRENT_USER` hive will actually be the administrator account, not the everyday account you're actually using.  Bear this in mind, as the changes you expect might not show up on your standard account.  (If you try to run the registry merge as the standard user, you will most likely get `Access Denied`, or the merge simply won't work.)

If you want to apply tweaks to a specific non-administrative user account, you will need to find that account's `SID`, a unique identifier created by Windows that remains stable throughout the life of the account, even if the username changes.  The `SID` has this general format:
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