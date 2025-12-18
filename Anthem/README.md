# Anthem CTF 
<p align="center">
  <img src="anthem.gif" alt="Anthem Room Completed" width="500"/>
</p>



##  Overview
This repository contains my write-up for the **Anthem** room on **TryHackMe**.  
The focus of this write-up is to document my **learning process**, including enumeration, web analysis, and gaining access to a Windows system.

- **Platform**: TryHackMe  
- **Room Name**: Anthem  
- **Difficulty**: Beginner  
- **Target OS**: Windows  
- **Status**: Completed ✅  

---

## 🧭 Attack Flow Summary
1. Port scanning using **Nmap**
2. Website enumeration and inspection
3. `robots.txt` analysis
4. Directory brute-forcing using **Gobuster**
5. Flag discovery through source inspection
6. Credential discovery from website hints
7. Windows access using **rdesktop**
8. File system enumeration and admin access

---

##  Step 1: Nmap Scan (Port Enumeration)

I started with an Nmap scan to identify open ports and services.

```bash
$ nmap ip -sC -sV -F -Pn
Findings:

Port 80 (HTTP) – Website accessible

Port 3389 (RDP) – Windows Remote Desktop enabled

This indicated both web and Windows-based attack surfaces.
```
##  Step 2: Website Enumeration

I accessed the website using the target IP address.

Key actions:

Browsed all visible pages

Inspected page source

Checked comments and metadata

Result:

Found 2 flags in two different webpages through inspection and source analysis

## Step 3: robots.txt Analysis

I visited:Step 3: robots.txt Analysis

I visited:```http://<TARGET_IP>/robots.txt```
http://<TARGET_IP>/robots.txt

## Step 4: Directory Enumeration using Gobuster

To find hidden directories, I used Gobuster.
```
gobuster dir -u ip -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --no-error -x .txt,.html,.php -t 40

```
Result:

Discovered hidden directories

Identified the CMS admin pane

## Step 5: Admin Panel Discovery & Credential Clues

From the Gobuster results:

Found the CMS admin login page

Credential information was gathered from:

Password → Found from robots.txt

Created a wordlist file "name" which contains words similar to the figured username
```
hydra -L /home/alen/name -p UmbracoIsTheBest! rdp://10.80.148.21
```
Username → Identified from a poem displayed on the website

This highlighted how information disclosure can lead to authentication bypass.

## Step 6: Windows Access via rdesktop 

ince RDP (port 3389) was open, I used rdesktop to connect to the Windows machine.
```
rdesktop IP -u SG -p UmbracoIsTheBest!


```
Outcome:

Successfully logged into the Windows system

Found another flag inside the user environment

## Step 7: File System Enumeration

After accessing the system:

Explored user directories

Located the Administrator folder

Found a backup file containing admin credentials

## Step 8: Admin Access & Final Flag

Using the credentials from the backup file:

Gained access to the Administrator account

Changed the file permissions 

Accessed restricted directories

Retrieved the final flag

## Key Learnings

Nmap is essential for identifying exposed services

Website inspection reveals hidden information

robots.txt should never contain sensitive data

Gobuster helps uncover hidden endpoints

Open RDP combined with weak credentials is dangerous

File system enumeration can reveal critical secrets

## ⚠️ Disclaimer

This write-up is for educational purposes only.
All activities were performed in TryHackMe’s authorized lab environment.
