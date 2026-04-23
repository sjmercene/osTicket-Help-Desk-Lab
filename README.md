# osTicket Help Desk Lab (End-to-End Deployment & Troubleshooting)

## 📌 Overview
In this lab, I deployed a fully functional osTicket help desk system from scratch on a Windows virtual machine.

Rather than using automated installers, I manually configured each component to better understand how web applications function in a real-world environment.

This project demonstrates my ability to:

Deploy and configure a Windows-based web server (IIS)
Integrate PHP with IIS using FastCGI
Install and configure a MySQL database
Troubleshoot real-world server, configuration, and application errors
Understand how backend components interact in a web application stack

---

## 🎯 Objectives
- Deploy a Windows VM in **Microsoft Azure** 
- Install and configure **Internet Information Services**
- Set up PHP and MySQL
- Install and configure osTicket
- Access and use a working help desk system

---

**🧰 Technologies Used**

- Platform: Microsoft Azure Virtual Machine
- OS: Windows 10
- Web Server: IIS (Internet Information Services)
- Backend Language: PHP 7.3
- Database: MySQL 8
- Application: osTicket v1.15.8

---

What I Built

I successfully deployed a working help desk system accessible at:

`http://localhost/osTicket`

The system supports:

- Ticket creation and tracking
- Admin dashboard access
- User and agent management
- Database-backed storage using MySQL

---

Key Components Explained
- Azure VM – Remote environment used to simulate a real server
- IIS – Hosts and serves the web application
- PHP – Executes backend logic for osTicket
- MySQL – Stores tickets, users, and configuration data
- FastCGI – Enables IIS to process PHP files

---
Installation Summary

I performed the following high-level steps:

- Created and connected to a Windows VM in Azure
  
- Installed IIS and enabled CGI support
- Installed MySQL Server and configured root access
- Installed PHP manually and configured php.ini
- Connected PHP to IIS using FastCGI
- Verified PHP functionality using info.php
- Deployed osTicket files into IIS web root
- Configured IIS default document (index.php)
- Enabled required PHP extensions
- Completed osTicket installation and connected to MySQL

---

### Troubleshooting & Problem Solving

During this lab, I encountered multiple real-world issues and resolved them step-by-step.

## - MySQL Installation Issues
Experienced repeated installation failures during MySQL setup
Resolved by re-running the installer and ensuring dependencies installed correctly****

## - File Extension Visibility Issue

Windows initially hid file extensions, which prevented proper renaming of:

php.ini-development → php.ini
Fixed by enabling:
“Show file extensions” in File Explorer

<br> 

## - PHP Not Working in IIS (HTTP 404.3)
IIS could not process .php files
Root cause: PHP not mapped to IIS

Fix:

Configured FastCGI handler:
*.php → C:\PHP\php-cgi.exe

<br>

## - FastCGI Crash (500 Error)
PHP process crashed when accessed via browser

Root cause:

Missing Visual C++ Redistributable

Fix:

Installed required runtime 
Restarted IIS using `issrestart` in Adminstrator CMD

<br> 

## - Default Document Error (HTTP 403.14)
osTicket directory loaded but showed “Forbidden”

Root cause:

IIS did not know which file to load

Fix:

Added index.php to Default Document in IIS

## - web.config Conflict Issue
Encountered issues loading osTicket due to IIS configuration conflict

Fix:

Renamed:

web.config → web.config.bak

<br> 

## - Missing PHP Extensions (MySQLi Error)
osTicket installer showed missing required extensions

Fix:
Edited php.ini file using notepad and removed `;` from in front of these file names to enable them. 

extension=mysqli 
extension=gd2
extension=imap
extension=mbstring
extension=openssl
extension=intl

After saving file, i restarted IIS using `issrestart` in Adminstrator CMD 

<br> 

## - MySQL Authentication Error

Error:

The server requested authentication method unknown to the client

Root cause:

MySQL 8 uses caching_sha2_password, which PHP 7.3 does not support

Fix:
Switched authentication method using MySQL Shell and typing the following commands:

`\sql`
`\connect root@localhost`

ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY 'labuser123!';

FLUSH PRIVILEGES;

<br> 

##Configuration File Not Writable

Error:

Configuration file is not writable

Root cause:

- IIS user did not have permission to write to ost-config.php
- Initial Attempt
- Assigned permissions to IIS_IUSRS
- Issue persisted
- Final Resolution
- Identified correct IIS user account: IUSR

Fix:

Added IUSR to file permissions
Granted:
- Read
- Execute
- Write
Restarted IIS using `issrestart` in Adminstrator CMD

This resolved the issue and allowed installation to complete.

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
