<h1>Hardening Security with User Account Management and Security Controls</h1>


<h2>Description</h2>
Computer security is a series of disparate systems that will each play a key role in the protection of resources and information. However, just about every security policy begins with and is centrally focused on the company’s password policy. Every auditor walking in to gauge the worth of the company’s accounting systems will ask to see the password policy and the logs for change control. In most cases, the passwords to the system are the keys to the vault.

 

Any effective password policy will have several elements. The passwords will expire regularly; they will have a certain complexity requirement for length and special characters, and will also have a minimum age requirement. In addition, every organization has “guest users”, be they auditors, contractors, temps, or other types of transient users on the system. Having a password system that can deal with these varieties of conditions requires intelligent design and well-trained and capable system administrators.

 

In this lab, I hardened user accounts on a Linux system with a secure password policy definition. This definition will include the use of user groups to better manage large numbers of users. I also created temporary user accounts and applied automatic account and password deletion after 90 days.
<br />


<h2>Tools and Software</h2>

- Terminal
- vi Editor

<h2>Lab walk-through:</h2>

I started off by applying hardened security measures on this server by configuring stringent passwords according to a policy definition. I edited the login definitions (login.defs) file in the vi Editor.

Used the su -c 'vi /etc/login.defs' command to launch the vi Editor and load the grub configuration file. The login.defs file appears in the vi Editor window. The login.defs file is used to define the configuration associated with logins into the local Linux system.

![image](https://github.com/user-attachments/assets/24147013-fb9a-47ff-9e36-879269e21241)

Changes to the Password aging controls section of the login.defs file affect the requirements for user account passwords, including the length of the password and the number of days before a password expires. Requiring users to change passwords on a scheduled basis will, at least, keep hackers guessing and force them to work harder to crack passwords all over again each time they are changed. This is time-consuming and off-putting. If the passwords are difficult to guess and change regularly, an organization is already well ahead of the skills of many rudimentary hackers.

PASS_MAX_DAYS is the maximum number of days a password can be used before a change of password is forced.

PASS_MIN_DAYS is the minimum number of days a password must be set on an account before it can be changed again. This is to prevent users from trying to revert back to the original password soon after being forced to change.

PASS_MIN_LEN is the minimum number of characters the password must contain.

PASS_WARN_AGE is the number of days a warning is sent to an account before the password expires.

Made the following changes to the password aging controls:

![image](https://github.com/user-attachments/assets/3c025bab-0c5f-48e2-9216-3ea031a92234)

Futhermore, I hardened security measures on this server by creating several new users accounts using the useradd command and set an initial password for one of the accounts, then require that password to be changed at the first login attempt.

I used the su -c /usr/sbin/useradd dbadmin1 command to create a new database administrator user account (dbadmin1). Additionally created the following accounts too: dbadmin2,
webadmin1, webadmin2.

Used su -c chage -d 0 dbadmin1 to force the password to be changed at the next login attempt (by changing the number of days between password change (-d) to zero).

![image](https://github.com/user-attachments/assets/d28182bc-4071-47e5-9717-fa7902e7e24b)

Next, I hardened security measures on this server by creating two new security groups. I added the new users to those groups and verifed user group membership.

The intelligent design and deployment of security groups greatly eases security management, change control, and access tightening. Putting users into groups and then applying the group to a resource is the best way to manage that resource. Changes to the control only need to be made to the group property, instead of possibly dozens if not hundreds of individual users.

Used the su -c /usr/sbin/groupadd dba command to add a new group called dba (database administrators) and su -c /usr/sbin/usermod -g dba dbadmin1 to add the dbadmin1 user account to the new dba group. Repeated the steps for all the users I created.

![image](https://github.com/user-attachments/assets/e6c4b36b-fdcf-45da-8d50-c80e5ad0338a)

Following that, I created temporary user accounts that will automatically expire in 90 days. This type of action is common in a business that frequently hires contractors or others that will need temporary access to their network.

Used su -c /usr/sbin/useradd jtemp to create a new database administrator user account (jtemp). su -c chage -d 0 jtemp to set the number of days between password changes for the jtemp account to zero (-d 0) and force a password change at the next login. And finally, su -c chage -E date jtemp to force the jtemp user to change the password I just set on the expiration date (-E), where date is the date 90 days from today. Password settings for the jtemp user account below:

![image](https://github.com/user-attachments/assets/ee4fef2e-c994-4595-8869-41531c2a7bd7)

Finally, I restricted the use of the su command which grants the user root level access. I edited the pam.d file to restrict the wheel group from using the su command.

Used su -c vi /etc/pam.d/su to edit the access for the su command in the pluggable authentication module file. Deleted the hash tag (#) at the beginning of the line, then typed x, so that the entire line now reads xauth required pam_wheel.so use_uid. This action cancels the statements and removes the wheel group from those authorized to use the su command.

![image](https://github.com/user-attachments/assets/982f9a89-8745-4d97-9ef5-b5b68d8a40ae)

