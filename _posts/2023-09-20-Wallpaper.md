---  
title: "Making Wallpaper as default without GPO and giving user freedom to change the wallpaper"  
date: 2023-09-21 00:00:00 +0800  
categories: [Windows-10-11]  
tags: [server, windows, powershell, group-policy, wallpapers]  
---
# Customizing Windows 11 Default Wallpaper

Windows 11 allows wallpaper customization through two traditional methods:
1. Manual changes (affects only user profile)
2. Group Policy (GPO - system-wide, but restricts user changes)

Here's a third method that sets a custom default wallpaper while maintaining user freedom to change it later.

## The Default Wallpaper Location
Windows 11's default wallpaper is located at:
```
C:\Windows\Web\Wallpaper\Windows\img0.jpg
```

![Default wallpaper location](https://cdn.hashnode.com/res/hashnode/image/upload/v1693301054371/1d9ffe33-6c93-4130-9554-faec2ed1e9a5.png)

## Steps to Replace Default Wallpaper

### Method 1: Using PowerShell
1. Open PowerShell as Administrator
2. Run the following commands:
```powershell
takeown /f C:\Windows\Web\wallpaper\Windows\img0.jpg
takeown /f C:\Windows\Web\4K\Wallpaper\Windows\*.*
icacls C:\windows\Web\Wallpaper\Windows\img0.jpg /Grant 'System:(F)'
icacls C:\Windows\Web\4K\Wallpaper\Windows\*.* /Grant 'System:(F)'
Remove-Item C:\windows\Web\Wallpaper\Windows\img0.jpg
Remove-Item C:\Windows\Web\4K\Wallpaper\Windows\*.*
```

### Method 2: Manual Permissions
If Method 1 fails:
1. Right-click `img0.jpg` → Properties
2. Select Security tab → Administrator → Edit
3. Grant Full Control to Administrator
4. Apply changes

## Final Steps
1. Delete the original `img0.jpg`
2. Copy your custom wallpaper to `C:\Windows\Web\Wallpaper\Windows\`
3. Rename it to `img0.jpg`

This method will set your custom image as the default Windows 11 wallpaper while allowing users to change it if desired.

