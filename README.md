# Active Directory & Access Control home Lab

## Overview

I built this lab to get hands-on experience with Windows Server and Active Directory, particularly how companies manage employee accounts, organize departments, and enforce security policies.

Using Oracle VirtualBox, I set up a Windows Server domain controller and created a small company environment with four departments: IT, HR, Finance, and Sales.

I also configured an account lockout policy and tested a failed login attempt to see how Windows records authentication events.

## Lab Setup

- **Virtualization:** Oracle VirtualBox
- **Operating System:** Windows Server
- **Domain:** `homelab.local`
- **Domain Controller:** `DC01`
- **Tools:** Active Directory Users and Computers, Group Policy Management, Windows Event Viewer, PowerShell

## 1. Setting Up Active Directory

After configuring the domain controller, I used Active Directory Users and Computers to create an `Employees` organizational unit (OU).

Inside it, I created four separate OUs to represent different departments in a company.

| Department | Employee |
|---|---|
| IT | June Hu |
| HR | John Hu |
| Finance | Jason Hu |
| Sales | Jacob Hu |

Each employee assigned to their respective department.

### IT Department

![IT Department](Screenshots/IT_setup.png)

### HR Department

![HR Department](Screenshots/HR_setup.png)

### Finance Department

![Finance Department](Screenshots/finanace_setup.png)

### Sales Department

![Sales Department](Screenshots/sales_setup.png)

## 2. Creating Security Groups

Next, I created security groups for each department:

- `IT-Staff`
- `HR-Staff`
- `Finance-Staff`
- `Sales-Staff`

I added the corresponding employees to their department's security group.

![Security Groups](Screenshots/Securitygroups_setup.png)

![Group Membership](Screenshots/securitygroup_setup2.png)

## 3. Configuring Account Lockout Policies

I wanted to explore how Active Directory can help protect accounts against repeated password guessing.

Using Group Policy Management, I configured the following settings:

| Setting | Value |
|---|---|
| Account lockout threshold | 5 failed attempts |
| Account lockout duration | 10 minutes |
| Reset lockout counter | 10 minutes |
| Administrator account lockout | Enabled |

After configuring the policy, I ran the following commands in PowerShell:

```powershell
gpupdate /force
net accounts /domain
```

The first command refreshed Group Policy, while the second allowed me to verify the domain account policy settings.

![Account Policy Verification](Screenshots/policy_verfication.png)

## 4. Investigating a Failed Login

I also wanted to see what happens behind the scenes when someone enters an incorrect password.

I simulated a failed authentication attempt using a domain account and then opened Windows Event Viewer to check the Security logs.

I found **Event ID 4625**, which Windows generates when a login attempt fails.

The event contained several useful details:

- **Account:** `june.hu`
- **Domain:** `HOMELAB`
- **Event ID:** `4625`
- **Failure reason:** Unknown user name or bad password
- **Status:** `0xC000006D`
- **Substatus:** `0xC000006A`

The substatus indicated an incorrect password.

This was useful because it showed me how a security analyst can investigate authentication failures using Windows event logs rather than relying only on what appears on the login screen.

![Failed Login Event](Screenshots/failed_login_event.png)


