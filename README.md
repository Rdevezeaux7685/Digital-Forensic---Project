# Mobile Forensic Analysis Project

## Project Overview


## Evidence Description


**Source:**

The evidence files come from a Forensic Analysis class from my home institution Howest University. This is a Mobile image. 
The zip of the evidence has been added as *ForensicLab8.zip*

## Tools Used

1. **Autopsy**
   *Purpose:* Autopsy is an open-source digital forensics platform that facilitates the analysis of hard drives, smartphones, and other storage devices.
   
   *Reason for Selection:* Autopsy provides a user-friendly interface and supports various file formats, making it suitable for mobile forensic analysis.

### Procedure Autopsy

1. **Create a New Case:**
   Open Autopsy and create a new case for your mobile forensic analysis. Enter relevant case details, such as case name, examiner name, and case number.

2. **Add Data Source:**
   Add the ForensicsLab8.E01 file as the data source for analysis, use the **Disk or image file**
   
   Calculate the Hash value of the image file : MD5             DD6D5114893C51A6B8707748E16D5226

   Using this command on powershell:
   > Get-FileHash .\ForensicsLab8.E01 -Algorithm Md5

4. **Start Analysis:**
   - Initiate the analysis process and let Autopsy examine the contents of the ForensicLab8.E01 file.

### Analysis

In dump/system/build.pro : This file reveals crucial details about a Samsung SM-N970U1 device running Android 11, including build type, security patch, and Knox version. Key specifics include the build fingerprint and build date (May 18, 2021). ![](/Discoveries/device-informations.png)

The folder **/system_ce/** is extemely important as it is the so-called "Credential Encrypted" directory, whoose encryption key includes the user-specific password. And these files are certainly inaccessible to most apps, except those to which I grant root access
reference: 
   - [direct-boot](https://developer.android.com/privacy-and-security/direct-boot)
   - [Auditing Android Security](https://tech.michaelaltfield.net/2018/11/09/android-security-auditing-investigating-unauthorized-screenshots/)
By looking in the data/data files, one can find multiple applications; such as Instagram, Netflix, Snapchat, Venmo, Twitter, Whatsapp, Waze, etc. 



## Results


## Conclusion



---

**Author: Romane Devezeaux De Lavergne**

