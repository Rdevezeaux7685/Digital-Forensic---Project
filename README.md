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
   - [3.3 Data Files](#33-datas-files)
   - [3.4 Web History](#34-web-history)
   - [3.5 Texts and SMS](#35-texts-and-sms)
   - [3.6 Installed Applications](#36-installed-application)
   - [3.7 Databases](#37-databases)
- [4. Conclusion](#4-conclusion)

---

## 1. Project Overview

The objective of this project is to conduct an analysis of digital evidence utilizing digital forensic tools. Specifically, Autopsy will be utilized for the analysis of the received evidence.

The evidence in question originates from a mobile device belonging to a suspect.  A search warrant has been obtained for this mobile device in relation to a drug dealing case. The purpose of the analysis is to identify any evidence that may establish a connection between the suspect and the alleged crime. 

## 1.1 Project Description

**Source:**

The evidence files come from a *Forensic Analysis* class from my home institution *Howest University*. This is an image from a mobile device. 
The zip of the evidence has been added as *ForensicLab8.zip* (this had to be removed due to the size of the file)

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
In `dump/system/build.pro` : This file reveals crucial details about a Samsung SM-N970U1 device running Android 11, including build type, security patch, and Knox version. Key specifics include the build fingerprint and build date (May 18, 2021). ![Device Information](/Evidences/device-informations.png)

### 3.2 Credential Encrypted Directory
The folder `/dump/data/system_ce/` is extremely important as it is the so-called "Credential Encrypted" directory, whose encryption key includes the user-specific password. And these files are certainly inaccessible to most apps, except those to which I grant root access
reference: 
   - [direct-boot](https://developer.android.com/privacy-and-security/direct-boot)
   - [Auditing Android Security](https://tech.michaelaltfield.net/2018/11/09/android-security-auditing-investigating-unauthorized-screenshots/)

### 3.3 Datas files

The file accounts_cd.db provides valuable information regarding accounts present on the mobile device, there is also a hashed password for the main Google account.
![Accounts_ce](/Evidences/accounts_ce.png). *The database was added to the evidences [discovery/database](/Evidences/databases/)*

In the **File Views/File Types/** one can see a lot of different informations, such as pictures, videos, etc.
![File Types](/Evidences/FilesType.png)


#### 3.4 Web History

contains the data of the different applications:

1. Chrome : `/data/data/com.android.chrome/app_chrome/Default/History`
This file is a SQL database that contains the History of the Chrome Application. There one can find the history of key searched and URL visited.
Nothing compromising in the suspect chrome History.

![Chrome History](/Evidences/history-google-chrome.png)
![URL Google Chrome](/Evidences/url-google-chrome.png)

*The databases where added to the evidences [discovery/database](/Evidences/databases/)*

These information can also be found on the **Data artifacts** from Autopsy Analysis 
![Data Artifacts](/Evidences/Data-articafts.png)

Looking Through the suspect **Web History** and **Web searches**, there seems to be no compromising searches or information there.

### 3.5 Texts and SMS

![](/Evidences/messages.png)

In the messages, one can see multiple phone number the suspect had communication with.Using filters to only show communication with a certain phone number, it is possible to see more easily the conversation the supect had. 

![](/Evidences//15402993169.png)

By creating a search query with the phone number, one can find all communication made with that person.![](/Evidences/search-phone-number.png)
It seems that the person and the suspect met for a car sale.

I was also able to find the db file; this database contains all text messages. ![](/Evidences/telephony-db.png) 

*The database was added to the evidences [discovery/database](/Evidences/databases/)*

A 2nd database showed us that some images where send through a conversation: 
![](/Evidences/0-message.png)
By looking in the appropriate directory `/user_de/0/com.android.providers.telephony/app_parts/`, the pictures can be found. There are picture of a car.

### 3.6 Installed applications

Looking through the application, a first application looked suspicious. `thoughcrime.securesms`. After looking on the internet this is the application Signal. Nothing suspicious.

![](/Evidences/thoughtcrime.png)

Somme application:

    com.orto.usa >  License Plate Recognition
    com.napko.RealDash > best vehicle companion app for road trips
    com.instantcheckmate.app> Instant Checkmate offers background checks 


#### 3.6.1 **Suspicious app:**

> `com.flatfish.cal.privacy `

Disguised as a secret calculator, HideX gallery vault & video lock is a stunning free video gallery vault, photo gallery lock, audio protector and privacy lock for your personal information and media files!! [Calculator (com.flatfish.cal.privacy) 3.5.17.4 APK Download](https://fr.apkshub.com/app/com.flatfish.cal.privacy)

In the data of this application we can see that 2 other application's data where hidden

- Catch cheating spouse
- whatsapp

![](/Evidences/fake-app.png)


In the /Dump/data/data/com/flatfish.cal.privacy/cache.image_manager_disk_cache one suspicious picture was found. 

![](/Evidences/secret-app/51e0876703e25c6ce5e459dd3cdda42171b3c30653a3f657af9db3bdb5db7c77.png)

and this
 ![](/Evidences/secret-app/message.png)

After making a search with the term `cheating`. One can see that an application called "catching cheating spouse" was indeed installed on the mobile device.
![](/Evidences/search-result.png)


### 3.6.2 **suspicious application**

`app.greyshirts.sslcapture`

This application seems to be capturing data acting like a man-in-the-middle

![](/Evidences/sslcapture.png)


### 3.7 Databases:

A few databases where added to the evidences files.

Some communication databases can be found here. 
![](/Evidences/databses.png) 
All of these databases either comes from Whatsapp or the Telephonie application as already covered in the [3.4 Text and sms](#34-texts-and-sms)

## 4. Conclusion

The forensic analysis of the mobile device provided insights into the suspect's activities. The examination of device information, data files, web history, texts and SMS, and installed applications revealed details about the user's interactions.

Key findings include the identification of the mobile device as a Samsung SM-N970U1 running Android 11, along with evidence of communication related to a car sale. The suspect's web history and searches did not reveal compromising information nor the various pictures founded.

However, the analysis uncovered potentially concerning applications, such as a disguised calculator app concealing other application in it and an application capturing data as a man-in-the-middle.

Text message analysis exposed communications with multiple phone numbers, including discussions related to a car sale. The investigation also revealed suspicious applications, one associated with catching cheating spouses and another capturing SSL data.

In summary, while the mobile forensic analysis using Autopsy provided information about the suspect's activities, it did not yield hard proof of drug dealing. The evidence gathered sheds light on various interactions and potentially suspicious behavior, but definitive evidence linking the suspect to drug-related activities was not found.

---

**Author: Romane Devezeaux De Lavergne**
