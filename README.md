# osTicket Help Desk Lab (End-to-End Deployment & Troubleshooting)

## Overview

In this lab, I deployed a fully functional **osTicket help desk system** from scratch on a Windows virtual machine.

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

## Verification

### osTicket Application Running

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/10.Login%20Success.JPG" width="700">

Successfully deployed and accessed the osTicket admin panel. Verified functionality by viewing and managing tickets through the web interface.

Accessed via: `http://localhost/osTicket`

---

### PHP Integration with IIS

<img src="https://github.com/sjmercene/osTicket-Help-Desk-Lab/blob/osTicket-Help-Desk-Lab-Images/Verify%20PHP%20Information%20Page.JPG" width="700">

Verified PHP was correctly integrated with IIS by executing a test script (`info.php`).

Accessed via: `http://localhost/info.php`

This confirmed FastCGI and PHP configuration were working correctly.

---

## Troubleshooting & Problem Solving

During this lab, I encountered multiple real-world issues while configuring IIS, PHP, MySQL, and osTicket. I resolved each issue by identifying the root cause and applying the appropriate fix.

---

### PHP Not Working in IIS (HTTP 404.3)

**Problem:**  
IIS could not process `.php` files.

**Cause:**  
PHP was not mapped to IIS, so `.php` files were treated as unsupported content.

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

---

### MySQL Installation Issues

**Problem:**  
Experienced repeated installation failures during MySQL setup.

**Cause:**  
Installer dependency issues and incomplete setup execution.

**Fix:**  
Re-ran the installer multiple times and ensured all required dependencies completed successfully.

**Result:**  
MySQL Server installed correctly and was available for use.

---

### File Extension Visibility Issue

**Problem:**  
Unable to properly rename configuration files such as `php.ini-development`.

**Cause:**  
Windows was hiding known file extensions.

**Fix:**  
Enabled file extension visibility in File Explorer by disabling the “Hide extensions for known file types” setting.

**Result:**  
Successfully renamed:

```text
php.ini-development -> php.ini
```

---

### FastCGI Crash (HTTP 500)

**Problem:**
PHP process crashed when accessed through the browser.

**Cause:**
Missing Visual C++ Redistributable dependency.

**Fix:**
Installed the required Visual C++ runtime and restarted IIS:

iisreset

**Result:**
PHP executed successfully through IIS.

Default Document Error (HTTP 403.14)

**Problem:**
The osTicket folder loaded but displayed a “Forbidden” error.

**Cause:**
IIS did not know which file to load by default.

**Fix:**
Added index.php to the Default Document list in IIS.

**Result:**
The osTicket installer page loaded correctly.

---

### web.config Conflict Issue

**Problem:**
Encountered issues loading osTicket due to an IIS configuration conflict.

**Cause:**
The existing web.config file interfered with IIS handling of the application.

**Fix:**
Renamed the file:

web.config -> web.config.bak

**Result:**
The osTicket application loaded successfully.

---

### Missing PHP Extensions (MySQLi Error)

**Problem:**
The osTicket installer reported missing required PHP extensions.

**Cause:**
Required extensions were disabled in php.ini.

**Fix:**
Edited C:\PHP\php.ini and enabled the following:

extension=mysqli
extension=gd2
extension=imap
extension=mbstring
extension=openssl
extension=intl

Restarted IIS: `iisreset` Using Adminstrator in CMD

**Result:**
All required extensions were detected and the installer checks passed.

---

###MySQL Authentication Error

**Problem:**
osTicket could not connect to MySQL and displayed:

The server requested authentication method unknown to the client

**Cause:**
MySQL 8 uses caching_sha2_password by default, which is not supported by PHP 7.3.

**Fix:**
Updated the authentication method using MySQL Shell:
```
\sql
\connect root@localhost

ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY 'labuser123!';

FLUSH PRIVILEGES;
```
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
