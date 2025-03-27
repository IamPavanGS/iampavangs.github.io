---
title: "AD Password-Reset GUI tool using Powershell"
date: 2023-08-28 00:00:00 +0800
categories: Powershell
tags: powershell active-directory windows-server password-reset powershell-gui
---
### A GUI tool helps in resetting the password for AD accounts.

AD password-Reset is an application that helps in resetting the LDAP password under diverse situations.

* Once the user opens the application, all the fields will be graded out except the user name and the Get Data button. After entering the username and clicking on the Get Data option, all the respective fields will be filled and buttons will be activated based on the data fetched from the Active Directory.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692975473155/39eaf699-7c26-48e6-b9f4-4aa99dcfe9f0.png align="center")

Based on the criteria and the data fetched from the AD, the respective buttons get activated. For example,

* If the account is locked the unlock button is activated
    

If the account is disabled, the enable button is activated.

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692975529368/fab82115-4ca5-457e-93a1-eadf6f2996b2.png" style="display: block; margin: 0 auto;" alt="Image description">


## Let us visualize how each function works.

* **What if the account gets locked?**
    
    If the account gets locked, the Unlock button is activated. Please note that Locked text will be shown in red.
    

Once the user clicks on the Unlock button, the account gets unlocked and the user will get the status of the account by turning the Unlocked indicator to green color.

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692975649389/69509bb9-4403-49dc-81d3-388078e81d4f.png" style="display: block; margin: 0 auto;" alt="Image description">

## Visualizing Account Status

**What if the account expires?**

If the account expires, the color indicator will be in red which describes the account's health that is the account is expired.

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692975736650/45809151-66e7-4b4c-9374-052ed5da4049.png" style="display: block; margin: 0 auto;" alt="Image description">

## How to change the password?

* Generate a random password and enter the same in the New Password Field. Now click on the Reset button.
    
* Enabling Force change at the next logon check box. This feature will let the user force change the password at the next login.
    

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692975862744/b26368f7-3b7e-4089-ba56-536985ba2d50.png" style="display: block; margin: 0 auto;" alt="Image description">

Once the password is successfully changed a dialogue box will appear stating

**The password has been changed successfully**

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692975914233/5f7113ce-baf4-4c36-9f2a-5669b139d0a6.png" style="display: block; margin: 0 auto;" alt="Image description">

## AD Account Password Health Indicators.

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692976158448/cc67ad96-9271-4aa2-8d66-de02083e16c4.png" style="display: block; margin: 0 auto;" alt="Image description">

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692976129304/274d4a32-f2ae-4d3f-8bb5-d2b6dc11047d.png" style="display: block; margin: 0 auto;" alt="Image description">

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1692976410320/9fa35c97-4665-44c9-b4e7-5614425c76c8.png" style="display: block; margin: 0 auto;" alt="Image description">

### If you want to make any modification to the Code based on your preference you can do it at the line

```xml
<Label Name="toolVersionRevisionTxt" Content="Copyright By " HorizontalAlignment="Right" Margin="0,5,10,5" VerticalAlignment="Top" FontSize="10" Foreground="#000080"/>
```

### And

```xml
<Label Name="creatorTxt" Content="Created by IT Team" HorizontalAlignment="Right" Margin="0,15,8,0" VerticalAlignment="Top" FontSize="10" Foreground="#000080"/>
    <Label Name="UserNameLbl" Content="User:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,40,0,0" Foreground="Black" />
```

### To change the background color you can use the below line

```xml
Title="AD Password Reset" Height="360.000" Width="400.000" Foreground="#FF161ED4" Background
```

---

# Complete Powershell Script

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[void][System.Reflection.Assembly]::LoadWithPartialName('presentationframework')
Add-Type -Assembly System.Windows.Forms

[xml]$XAML = @'
<Window x:Name="ADPwordReset"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="AD Password Reset" Height="360.000" Width="400.000" Foreground="#FF161ED4" Background="#E3F2FD"
    ResizeMode="CanMinimize"
    FocusManager.FocusedElement="{Binding ElementName=UserNameTextBox}">
    <Grid Margin="0,0,0,-131">
    
    <Button Name="btnUnlock" Content="Unlock" VerticalAlignment="Top" HorizontalAlignment="right" Margin="0,215,90,0" Width="50" IsEnabled="False" ToolTip="Click to unlock user account." />
    <Button Name="btnEnable" Content="Enable" VerticalAlignment="Top" HorizontalAlignment="right" Margin="0,215,15,0" Width="50" IsEnabled="False" ToolTip="Click to unlock user account." />
    <Button Name="btnGetData" Content="Get Data" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="15,295,0,0" Width="70" />
    <Button Name="btnReset" Content="Reset" VerticalAlignment="Top" HorizontalAlignment="Center" Margin="0,295,0,0" Width="50" IsEnabled="False" />
    <Button Name="btnExit" Content="Exit" VerticalAlignment="Top" HorizontalAlignment="right" Margin="0,295,20,0" Width="50" />
    
    <Label Name="toolVersionRevisionTxt" Content="Copyright By" HorizontalAlignment="Right" Margin="0,5,10,5" VerticalAlignment="Top" FontSize="10" Foreground="#000080"/>
    <Label Name="creatorTxt" Content="Created by IT Team" HorizontalAlignment="Right" Margin="0,15,8,0" VerticalAlignment="Top" FontSize="10" Foreground="#000080"/>
    <Label Name="UserNameLbl" Content="User:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,40,0,0" Foreground="Black" />
    <TextBox Name="UserNameTextBox" HorizontalAlignment="Left" Height="25" Margin="50,40,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="150" BorderBrush="Black" Background="White"/>
    
    <Border Name="UserDataBdr" BorderBrush="Red" BorderThickness="2" HorizontalAlignment="Left" Height="170" Margin="10,70,0,0" VerticalAlignment="Top" Width="375" CornerRadius="5"/>
    <Label Name="FullNameFieldLbl" Content="Name:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="9,70,0,0" Foreground="Black" />
    <Label Name="FullNameLbl" Content="" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="50,70,0,0" Foreground="Blue" />
    
    <Label Name="UserIDFieldLbl" Content="UserID:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,90,0,0" Foreground="Black" />
    <Label Name="UserIDLbl" Content="" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="55,90,0,0" Foreground="Blue" />
    <Label Name="EmailAddressLbl" Content="Email Address:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,110,0,0" Foreground="Black" />
    <Label Name="EmailLbl" Content="" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="95,110,0,0" Foreground="Blue" />
    
    
    <Ellipse Name="ExpiredStpLt" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="15,155,0,0" Fill="LightGray" Height="15" Width="15" StrokeThickness="2" Stroke="Black" ToolTip="User's password has expired." />
    <Label Name="ExpiredLbl" Content="Expired" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="30,150,0,0" Foreground="LightGray" />
    <Label Name="ResetCountFieldLbl" Content="Next Reset:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="135,150,0,0" Foreground="Black" />
    <Label Name="ResetCountLbl" Content="0" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="205,150,0,0" Foreground="Blue" />
    <Label Name="ResetCountUnitsLbl" Content="Days" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="225,150,0,0" Foreground="Black" />
    <Label Name="LastFailedLogonFieldLbl" Content="Last Failed Logon:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,170,0,0" Foreground="Black" />
    <Label Name="LastFailedLogonLbl" Content="" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="110,170,0,0" Foreground="Blue" />
    <Label Name="FailedAttemptsFieldLbl" Content="Failed Attempts:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,190,0,0" Foreground="Black" />
    <Label Name="FailedAttemptsLbl" Content="0" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="100,190,0,0" Foreground="Blue" />
    <Label Name="LockedLbl" Content=" Locked" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="140,190,0,0" Foreground="LightGray" />
    <Ellipse Name="EnabledStpLt" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="205,195,0,0" Fill="LightGray" Height="15" Width="15" StrokeThickness="2" Stroke="Black" ToolTip="User account has been disabled." />
    <Label Name="EnabledLbl" Content="Disabled" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="220,190,0,0" Foreground="LightGray" />
    <Label Name="LastLogonFieldLbl" Content="Last Logon:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,210,0,0" Foreground="Black" />
    <Label Name="LastLogonLbl" Content="" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="75,210,0,0" Foreground="Blue" />
    
    <Label Name="NewPasswordLbl" Content="New Password:" VerticalAlignment="Top" HorizontalAlignment="Left" Margin="10,245,0,0" Foreground="Black" />
    <TextBox Name="NewPasswordTextBox" HorizontalAlignment="Left" Height="25" Margin="105,245,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="150" BorderBrush="Black" Background="DarkGray" IsEnabled="False" />
    
    <CheckBox Name="ForceChangeCheckbox" Content="Force change at next logon" IsEnabled="False" Margin="115,275,0,0" HorizontalAlignment="Left" VerticalAlignment="Top" Foreground="LightGray" ToolTip="Check this box to force user to change their password at next logon." />

    </Grid>
</Window>
'@

# Read XAML
$reader=(New-Object System.Xml.XmlNodeReader $xaml)
try{$Form=[Windows.Markup.XamlReader]::Load($reader)}
catch{Write-Host "Unable to load Windows.Markup.XamlReader.  Some possible causes for this problem include:  .NET Framework is missing.  PowerShell must be launched with PowerShell -sta. Invalid XAML code was encournted.":exit}

#============================================
# Store From Objects in PowerShell
#============================================
$xaml.SelectNodes("//*[@Name]") | %{Set-Variable -Name ($_.Name) -Value $Form.FindName($_.Name)}

#============================================
# Add events to Form Objects
#============================================
$btnUnlock.Add_Click({UnlockADAccount})
$btnEnable.Add_Click({EnableADAccount})
$btnGetData.Add_Click({GetADUserData})
$btnReset.Add_Click({SetADPassword})
$btnExit.Add_Click({$Form.Close()})
$UserNameTextBox.Add_KeyDown({if ($Args[1].key -eq 'Return') {GetADUserData}})

#============================================
# Global Parameters
#============================================
[string] $global:targetUser = ""
[array] $global:UserData = @()
[boolean] $global:forceReset = $false
[boolean] $global:disabledAD = $false
[array] $global:UserFieldLbls = @($FullNameLbl,$ExtensionLbl,$UserIDLbl,$EnabledLbl,$EmailLbl,
                                $ExpiredLbl,$ResetCountLbl,$LastFailedLogonLbl,$FailedAttemptsLbl,
                                $LockedLbl,$LastLogonLbl)
[array] $global:StopLgts = @($EnabledStpLt,$ExpiredStpLt)
[int] $global:PasswordLife = 60

#============================================
# Reset Focus Function - reset's focus to User Name text box 
#============================================
function Reset-Focus {
    $UserNameTextBox.Focus()
    $UserNameTextBox.SelectAll()
    
    Return
}

#============================================
# Reset form Function - reset's the form to starting defaults 
#============================================
function Reset-Form {
    # blank all the user data labels
    ForEach ($label in $UserFieldLbls) {
        $label.Content = ""
    }

    # reset all the stoplights to default
    ForEach ($light in $StopLgts) {
        $light.Fill = "LightGray"
    }

    # disable reset functions
    $btnReset.IsEnabled = $false
    $ForceChangeCheckbox.IsEnabled = $false
    $ForceChangeCheckbox.Foreground = "LightGray"
    $NewPasswordTextBox.IsEnabled = $false
    $NewPasswordTextBox.Background = "DarkGray"
    
    Return
}

#============================================
# Unlock AD account function
#============================================

Function UnlockADAccount {
    $user = $targetUser
    Unlock-ADAccount -Identity $user

    GetADUserData
}

#============================================
# Enable AD account function
#============================================

Function EnableADAccount {
    $user = $targetUser
    Enable-ADAccount -Identity $user

    GetADUserData
}

#============================================
# reset form after password reset and unable
# force change at next logon
#============================================
Function UnableToForce {
    # send the pop-up
    $wshell = New-Object -ComObject Wscript.Shell
    $wshell.Popup("This password does not expire.  Unable to force change at next logon.",0,"Done!",0x0)
    
    $NewPasswordTextBox.Text = ""
    $ForceChangeCheckbox.IsChecked = $false
    $ForceChangeCheckbox.IsEnabled = $false

    Return
}

#============================================
# reset form after password reset
#============================================
Function PasswordSet {
    # send the pop-up
    $wshell = New-Object -ComObject Wscript.Shell
    $wshell.Popup("Password has been changed successfully.",0,"Done!",0x0)
    
    $NewPasswordTextBox.Text = ""
    $ForceChangeCheckbox.IsChecked = $false

    Return
}

#============================================
# reset AD user account password
#============================================

Function SetADPassword {
    $password = $NewPasswordTextBox.Text.ToString()
    
    Set-ADAccountPassword -Identity $targetUser -Reset -NewPassword (ConvertTo-SecureString -AsPlainText $password -Force)
    
    If ($($ForceChangeCheckbox.IsChecked))
    {
        Set-ADUser -Identity $targetUser -ChangePasswordAtLogon $true
    } Else
    {
        Set-ADUser -Identity $targetUser -ChangePasswordAtLogon $false
    }

    If ($($userData.PasswordNeverExpires))
    {
        UnableToForce
    } Else
    {
        PasswordSet
    }

    Reset-Focus

    Return
}

#============================================
# reset form if bad or no username
#============================================
Function NoUser {
    
    Reset-Form

    # send the pop-up
    $wshell = New-Object -ComObject Wscript.Shell
    $wshell.Popup("Could not find the specified user: $targetUser.",0,"Oops!",0x0)
    
    Return
}

#============================================
# Output AD user data to Form
#============================================
Function FormatUserData {
    # local function parameter reset
    $lastLogon = ""
    $lastFail = ""
    $expiresOn = ""
    $resetDays = 0

    # Password day and date calculations
    $lastLogon = '{0:dd-MMM-yy /  HH:mm}' -f $($UserData.LastLogonDate)
    $lastFail = '{0:dd-MMM-yy  /  HH:mm}' -f $($UserData.LastBadPasswordAttempt)
    $today = Get-Date # today's date
    $expiresOn = $($UserData.PasswordLastSet).AddDays($PasswordLife)
    $resetDays = $($expiresOn - $today).Days

    # Fill-in-the-Blanks
    $FullNameLbl.Content = $UserData.Name
    #$ExtensionLbl.Content = $UserData.telephoneNumber
    $UserIDLbl.Content = $UserData.SamAccountName
    $EmailLbl.Content = $UserData.EmailAddress
    #$DeptLbl.Content = $UserData.Department
    #$TitleLbl.Content = $UserData.Title
    #Sowmya 
    $LastLogonLbl.Content = $lastLogon
    $LastFailedLogonLbl.Content = $lastFail
    $FailedAttemptsLbl = $UserData.badPwdCount
    
    # Color-coding the Output and allow for password reset
    if ($($UserData.Enabled))
    {
        $EnabledStpLt.Fill = "Green"
        $EnabledLbl.Content = "Enabled"
        $EnabledLbl.Foreground = "Green"
        If ($disabledAD)
        {
            $btnEnable.IsEnabled = $false
        } Else
        {
            Out-Null
        }
        
        # enable reset functions
        $btnReset.IsEnabled = $true
        $ForceChangeCheckbox.IsEnabled = $true
        $ForceChangeCheckbox.Foreground = "Black"
        $NewPasswordTextBox.IsEnabled = $true
        $NewPasswordTextBox.Background = "White"

    } Else
    {
        $EnabledStpLt.Fill = "Red"
        $EnabledLbl.Content = "Disabled"
        $EnabledLbl.Foreground = "Red"
        If ($disabledAD)
        {
            $btnEnable.IsEnabled = $true
        } Else
        {
            Out-Null
        }
        
        # disable reset functions
        $btnReset.IsEnabled = $false
        $ForceChangeCheckbox.IsEnabled = $false
        $ForceChangeCheckbox.Foreground = "LightGray"
        $NewPasswordTextBox.IsEnabled = $false
        $NewPasswordTextBox.Background = "DarkGray"
    }

    If ($($UserData.PasswordNeverExpires))
    {
        $resetDaysColor = "Green"
        $ExpiredStpLt.Fill = "Orange"
        $resetDays = 0
        $ExpiredLbl.Content = "Never Expires"
        $ExpiredLbl.Foreground = "Orange"
        $ExpiredStpLt.ToolTip = "This password never expires.  Not best practice..."
    } Else
    {   
        $resetDaysColor = Switch ($resetDays) {
            {$_ -lt 0} {"Red"}
            {$_ -ge 0 -and $_ -le 4} {"Red"}
            {$_ -ge 5 -and $_ -le 14} {"Orange"}
            {$_ -ge 15} {"Green"}
        }
        $ExpiredLbl.Foreground = $resetDaysColor
        $ExpiredStpLt.Fill = $resetDaysColor
        
        If ($resetDays -le 0)
        {
            $ExpiredLbl.Content = "Expired"
            $resetDays = 0
            $ExpiredStpLt.ToolTip = "This password has expired."
        } Else
        {
            $ExpiredLbl.Content = "Not Expired"
            $ExpiredStpLt.ToolTip = "This password is still good."
        }
    }

    If ($($userData.LockedOut))
    {
        $LockedLbl.Content = "Locked"
        $LockedLbl.Foreground = "Red"
        $btnUnlock.IsEnabled = $true
    } Else
    {
        $LockedLbl.Content = "Unlocked"
        $LockedLbl.Foreground = "Green"
        $btnUnlock.IsEnabled = $false
    }

    $ResetCountLbl.Content = $resetDays
    $ResetCountLbl.Foreground = $resetDaysColor

}

#============================================
# get AD user data
#============================================

Function GetADUserData {
    # Parameters
    $global:targetUser = $UserNameTextBox.Text.ToString()
    $global:UserData = @()
    $userProperties = @("Name","SamAccountName","Enabled","EmailAddress","badPwdCount",
                         "LastBadPasswordAttempt","LastLogonDate","PasswordExpired",
                        "PasswordLastSet","PasswordNeverExpires","LockedOut")
    
    # Pull Active Directory user data
    $global:UserData = Get-ADUser -Identity $targetUser -Properties * | Select $userProperties
    
    If ($($UserData.Length) -gt 0)
    {
        FormatUserData
    } Else
    {
        NoUser
    }
    
    Reset-Focus

    Return
}

#============================================
# Shows the form
#============================================
$Form.ShowDialog() | out-null

<#
_________                                ________    _________
\______   \_____ ___  _______    ____    /  _____/   /   _____/
 |     ___/\__  \\  \/ /\__  \  /    \  /   \  ___   \_____  \ 
 |    |     / __ \\   /  / __ \|   |  \ \    \_\  \  /        \
 |____|    (____  /\_/  (____  /___|  /  \______  / /_______  /
                \/           \/     \/          \/          \/ 
#>
```

