# osTicket Help Desk Lab 

🚀 End-to-End Deployment & Troubleshooting (IIS, PHP, MySQL)


<p align="center">
  <img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/OS%20Ticket.JPG" width="600">
</p>

## Overview

In this lab, I deployed a fully functional osTicket help desk system from scratch on a Windows virtual machine hosted in Microsoft Azure.

Rather than relying on automated installers, I manually configured each component so I could better understand how web applications work in a real-world environment.

This project demonstrates my ability to:

- Deploy and configure a Windows-based web server (IIS)
- Integrate PHP with IIS using FastCGI
- Install and configure a MySQL database
- Troubleshoot real-world server, configuration, and application errors
- Understand how backend components interact in a web application stack

---

## Objectives

- Deploy a Windows VM in Microsoft Azure  
- Install and configure Internet Information Services (IIS)  
- Set up PHP and MySQL  
- Install and configure osTicket  
- Access and use a working help desk system  

---

## Technologies Used

- **Platform:** Microsoft Azure Virtual Machine  
- **OS:** Windows 10  
- **Web Server:** IIS (Internet Information Services)  
- **Backend Language:** PHP 7.3  
- **Database:** MySQL 8  
- **Application:** osTicket v1.15.8  

---

## What I Built

I successfully deployed a working help desk system accessible at:

http://localhost/osTicket

The system supports:

- Ticket creation and tracking  
- Admin dashboard access  
- User and agent management  
- Database-backed storage using MySQL  

---

## Key Components Explained

- **Azure VM** – Remote environment used to simulate a real server  
- **IIS** – Hosts and serves the web application  
- **PHP** – Executes backend logic for osTicket  
- **MySQL** – Stores tickets, users, and configuration data  
- **FastCGI** – Enables IIS to process PHP files  

---

## Installation Summary

I performed the following high-level steps:

1. Created and connected to a Windows VM in Azure  
2. Installed IIS and enabled CGI support  
3. Installed MySQL Server and configured root access  
4. Installed PHP manually and configured `php.ini`  
5. Connected PHP to IIS using FastCGI  
6. Verified PHP functionality using `info.php`  
7. Deployed osTicket files into IIS web root  
8. Configured IIS default document (`index.php`)  
9. Enabled required PHP extensions  
10. Completed osTicket installation and connected to MySQL  

---

## ✅ Verification

### osTicket Application Running

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/10.Login%20Success.JPG" width="700">

After completing the installation, I accessed the osTicket admin panel to verify the system was fully operational.

From the dashboard, I was able to view and manage tickets, confirming that the application, web server, and database were all working together correctly. I also created a test ticket to confirm that data was being written to the database successfully.

Accessed via: `http://localhost/osTicket`

---

### PHP Integration with IIS

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Verify%20PHP%20Information%20Page.JPG" width="900">

To verify PHP was correctly integrated with IIS, I created and executed a test script (`info.php`).

Accessed via: `http://localhost/info.php`

When accessed through the browser, the page displayed detailed PHP configuration information, confirming that IIS was successfully processing PHP files through FastCGI. 

This also confirmed that the PHP installation and IIS handler mapping were configured correctly.

---

## 🛠️ Troubleshooting & Problem Solving

During this lab, I encountered multiple real-world issues while configuring IIS, PHP, MySQL, and osTicket. I resolved each issue by identifying the root cause and applying the appropriate fix.

---

### PHP Not Working in IIS (HTTP 404.3)

**Problem:**  
IIS could not process `.php` files.

**Cause:**  
PHP was not mapped to IIS, so `.php` files were treated as unsupported content.  
I identified this based on the HTTP 404.3 error shown in the browser, which i looked into some forums online which indicates that the server does not have a handler configured for the requested file type.

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Troubleshooting/Error%201.JPG" width="700">

**Fix:**  
Configured a FastCGI handler mapping in IIS:

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Adding%20Module%20Mapping.JPG" width="700">

```text
Request path: *.php
Module: FastCgiModule
Executable: C:\PHP\php-cgi.exe
Name: PHP via FastCGI
```
**Result:**  
IIS was able to process PHP files correctly, and PHP scripts executed successfully in the browser, confirming that the FastCGI configuration was working as expected.

---

### MySQL Installation Issues

**Problem:**  
The installer would not install files properly, likely due to dependency or configuration issues during the setup process.  
I identified this after noticing the installation would fail partway through and not fully complete.

**Cause:**  
Installer dependency issues and incomplete setup execution.

**Fix:**  
I navigated back through the installer and re-executed the setup steps multiple times, ensuring each stage completed properly.

**Result:**  
MySQL Server installed successfully and was available for database use. This approach helped resolve the issue without needing to restart the entire installation.

---

### File Extension Visibility Issue

**Problem:**  
Unable to properly rename configuration files such as `php.ini-development`.

**Cause:**  
Windows hides file extensions by default, which made it appear as if the file did not have an extension.  
I identified this after noticing the file name did not change correctly when attempting to rename it.

**Fix:**  
Enabled file extension visibility in File Explorer by disabling the `Hide extensions for known file types` setting.

**Result:**  
I was able to correctly rename file extension.

```text
php.ini-development -> php.ini
```
This ensured PHP loaded the correct configuration file required for proper operation.

---

### FastCGI Crash (HTTP 500)

**Problem:**
PHP process crashed when accessed through the browser throwing a HTTP 500 error 

**Cause:**
Missing Visual C++ Redistributable dependency.  
I conducted further research online, which pointed to a missing Visual C++ Redistributable dependency.

**Fix:**
Installed the required Visual C++ runtime and restarted IIS using CMD in Adminstrator with the command:
`iisreset`

**Result:**
PHP executed successfully through IIS.

---

### Default Document Error (HTTP 403.14)

**Problem:**
When accessing the osTicket directory in the browser, IIS returned a 403.14 Forbidden error and did not display the application.

**Cause:**
I identified the cause by researching the 403.14 error code, which indicates that no default document is configured in IIS. This means IIS does not know which file (e.g., index.php) to load when accessing a directory.

**Fix:**
Added `index.php` to the Default Document list in IIS.

**Result:**
The osTicket installer page loaded correctly in the browser, confirming that IIS was able to serve the application as expected.

---

### web.config Conflict Issue

**Problem:**
The osTicket site would not load correctly in IIS.

**Cause:**
The `web.config` file was likely causing a configuration conflict.  
I suspected this after other settings were corrected but the site still would not load properly.

**Fix:**
Renamed the file:

`web.config -> web.config.bak`

**Result:**
The osTicket application loaded successfully.

---

### Missing PHP Extensions (MySQLi Error)

**Problem:**
The osTicket installer reported missing required PHP extensions.

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Troubleshooting/Error%204%20missing%20MySQL.JPG" width="600">

**Cause:**
Required extensions were disabled in the `php.ini` configuration file.  
I identified this after reviewing the installer requirements page, which showed missing extensions needed for osTicket to function properly.

**Fix:**
Edited `C:\PHP\php.ini` using notepad and enabled the following extensions by removing the `;` in front of each line:
```
extension=mysqli
extension=gd2
extension=imap
extension=mbstring
extension=openssl
extension=intl
```
Then restarted IIS:
`iisreset` Using Adminstrator in CMD

**Result:**
All required extensions were detected by the installer, and the setup was able to proceed successfully.

This ensured PHP had the required functionality to support osTicket and communicate with the database.

---

### MySQL Authentication Error

**Problem:**
MySQL 8 uses `caching_sha2_password` as the default authentication method, which is not supported by the PHP version being used.  
I identified this after encountering the error during installation and researching the message, which pointed to an authentication compatibility issue.

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Troubleshooting/MYSQL%20error%202.JPG" width="500">

**Cause:**
MySQL 8 uses caching_sha2_password by default, which is not supported by PHP 7.3.

**Fix:**
Connected to MySQL using MySQL Shell and updated the authentication method:
```
\sql
\connect root@localhost

ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY 'labuser123!';

FLUSH PRIVILEGES;
```
I initially encountered errors when running commands in the wrong mode and before connecting to the database, but resolved this by switching to SQL mode and establishing a connection first.

**Result:**
osTicket successfully connected to the MySQL database.

---

### Configuration File Not Writable

**Problem:**  
osTicket could not write to `ost-config.php` during installation.

**Cause:**  
The IIS service account did not have the required permissions to write to the configuration file.

**Initial Attempt:**  
I initially assigned permissions to `IIS_IUSRS`, but the issue persisted.

**Fix:**  
After further troubleshooting, I identified that IIS was running under the `IUSR` account. I added `IUSR` to the file permissions for `ost-config.php` and granted:

- Read  
- Execute  
- Write  

Then I restarted IIS:

```bat
iisreset
```
**Result:** 
The installer was able to write to the configuration file successfully, allowing the osTicket installation to complete without errors.

---

### Final Result

The osTicket system was successfully installed and is fully functional.

I can now:

- Access the admin panel
- Create and manage tickets
- Simulate a real help desk environment

## What I Learned

This project helped me understand:

- How web servers handle dynamic content
- How IIS integrates with PHP using FastCGI
- How backend services (PHP + MySQL) interact
- How to troubleshoot real deployment issues step-by-step
- The importance of permissions and authentication in web applications
