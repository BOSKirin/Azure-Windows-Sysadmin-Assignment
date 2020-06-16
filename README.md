# Azure-Windows-Sysadmin-Assignment
A day in the life of a Windows Sysadmin

**Note:  based on research, there is issue relate to the Hyper-V manager:  if we cannot login the windows 10 vm as domain users even if the domain users granted RDP permission, we need to click ‘view’ option on the top bar of the vm window, then uncheck the ‘Advanced Session’ option. Now, we should be able to login as domain users.**

### Task 1:  Create an Account Lockout GPO

According to the document of Configuring Account Lockout from Microsoft, there are pros and cons of creating an account lockout GPO. One of the most important pros is that to set up account lockout GPO can prevent users’ passwords getting attacked easily, especially for the brute force password-guessing attacks. One of the significant cons is that the account lockout GPO may lock users, expose the organization to accidental lockouts, or permanently accidentally; it can also cause DoS attacks to the companies.
Before setting up the account lockout GPO, we need to consider the baseline and tradeoffs of this GPO. There are three settings can complete the account lockout configuration:

- Account lockout threshold
- Account lockout duration
- Reset account lockout counter after

To set up the account lockout GPO, we need to design a good combination for these three values. However, depends on the structure, situations, and other factors of the organizations, the setting of account lockout may vary.
If the lockout threshold is set high, it may not really help to protect the users’ password from attacking; it is set to low, the chance to lockout accidental will be significantly increased, which will bring more extra work to the service desk as well as it will affect employees work efficiency. If the account lockout duration is set too large, it will definitely affect users’ work if they were lockout accidentally; if the account lockout duration is set too short, it can lead to the continuous lockout when attackers maintain a sustained attack. The reset account lockout counter after is similar to the account lockout duration setting. We can always combine account lockout GPO with other password policies to harden the password and reduce the need for account lockout.
In this task, I would like to choose the Microsoft recommended settings for the account lockout GPO, which is 10/15/15. These three number means the threshold for bad attempts is 10, a 15-minutes lockout duration, the bad-logon counter will be reset after 15 minutes.

Click ‘Tools’ on the top right bar in the server manager window screen, select ‘Group Policy Management’, then create a new GPO for our GC domain → Computer Configuration → Windows Settings → Security Settings → Account Policies → Account Lockout Policy

![](Images/GPM-1.png)

Change settings for all three policies here:

- Account lockout threshold to 10 invalid logon attempts
- Account lockout duration to 15 mins
- Reset account lockout counter after 15 mins

![](Images/GPM-2.png)

Run the ‘Command Prompt’ as Administrator → type command ‘gpupdate /force’ to update GPO

![](Images/UpdateGPO.png)

![](Images/NewGroup.png)