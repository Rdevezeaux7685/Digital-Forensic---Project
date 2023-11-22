# Mobile Forensic Analysis Project

## Table of Contents

- [1. Project Overview](#1-project-overview)
   - [1.1 Project Description](#11-project-description)
   - [1.2 Evidence Source](#12-evidence-source)
- [2. Tools Used](#2-tools-used)
   - [2.1 Autopsy](#21-autopsy)
      - [2.1.1 Purpose](#211-purpose)
      - [2.1.2 Reason for Selection](#212-reason-for-selection)
      - [2.1.3 Procedure Autopsy](#213-procedure-autopsy)
- [3. Analysis](#3-analysis)
   - [3.1 Device Information](#31-device-information)
   - [3.2 Credential Encrypted Directory](#32-credential-encrypted-directory)
   - [3.3 File Views and Types](#33-file-views-and-types)
   - [3.4 Web History](#34-web-history)
   - [3.5 Texts and SMS](#35-texts-and-sms)
- [4. Conclusion](#4-conclusion)

---

## 1. Project Overview

## 1.1 Project Description

**Source:**

The evidence files come from a Forensic Analysis class from my home institution Howest University. This is a Mobile image. 
The zip of the evidence has been added as *ForensicLab8.zip*

## 2. Tools Used

### 2.1 Autopsy
   *Purpose:* Autopsy is an open-source digital forensics platform that facilitates the analysis of hard drives, smartphones, and other storage devices.
   
#### 2.1.2 Reason for Selection
   Autopsy provides a user-friendly interface and supports various file formats, making it suitable for mobile forensic analysis.

#### 2.1.3 Procedure Autopsy

1. **Create a New Case:**
   Open Autopsy and create a new case for your mobile forensic analysis. Enter relevant case details, such as case name, examiner name, and case number.

2. **Add Data Source:**
   Add the ForensicsLab8.E01 file as the data source for analysis, use the **Disk or image file**
   
   Calculate the Hash value of the image file: MD5             DD6D5114893C51A6B8707748E16D5226

   Using this command on PowerShell:
   > Get-FileHash .\ForensicsLab8.E01 -Algorithm Md5

4. **Start Analysis:**
   - Initiate the analysis process and let Autopsy examine the contents of the ForensicLab8.E01 file.

## 3. Analysis

### 3.1 Device Information
In `dump/system/build.pro` : This file reveals crucial details about a Samsung SM-N970U1 device running Android 11, including build type, security patch, and Knox version. Key specifics include the build fingerprint and build date (May 18, 2021). ![Device Information](/Discoveries/device-informations.png)

### 3.2 Credential Encrypted Directory
The folder `/dump/data/system_ce/` is extremely important as it is the so-called "Credential Encrypted" directory, whose encryption key includes the user-specific password. And these files are certainly inaccessible to most apps, except those to which I grant root access
reference: 
   - [direct-boot](https://developer.android.com/privacy-and-security/direct-boot)
   - [Auditing Android Security](https://tech.michaelaltfield.net/2018/11/09/android-security-auditing-investigating-unauthorized-screenshots/)

The file accounts_cd.db provides valuable information regarding accounts present on the mobile device, there is also a hashed password for the main Google account.
![Accounts_ce](/Discoveries/accounts_ce.png)

In the **File Views/File Types/** one can see a lot of different informations, such as pictures, videos, etc.
![File Types](/Discoveries/FilesType.png)

### 3.3 File Views and Types

#### Web History
contains the data of the different applications:

1. Chrome : `/data/data/com.android.chrome/app_chrome/Default/History`
This file is a SQL database that contains the History of the Chrome Application. There one can find the history of key searched and URL visited.
Nothing compromising in the suspect chrome History.
![Chrome History](/Discoveries/history-google-chrome.png)
![URL Google Chrome](/Discoveries/url-google-chrome.png)

These information can also be found on the **Data artifacts** from Autopsy Analysis 
![Data Artifacts](/Discoveries/Data-articafts.png)

Looking Through the suspect **Web History** and **Web searches**, there seems to be no compromising searches or information there.

### 3.4 Texts and SMS

## 4. Conclusion



---

**Author: Romane Devezeaux De Lavergne**
