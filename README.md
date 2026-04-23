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
