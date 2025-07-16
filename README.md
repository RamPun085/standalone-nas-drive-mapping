# standalone-nas-drive-mapping
# NAS Network Drive Mapping Setup

This guide explains how to map network drives from a NAS device on your local network using a friendly hostname instead of IP address, set custom drive labels, and automate the process with batch scripts.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1: Configure Hosts File for Name Resolution](#step-1-configure-hosts-file-for-name-resolution)
- [Step 2: Verify SMB Shares on NAS](#step-2-verify-smb-shares-on-nas)
- [Step 3: Map Network Drives Manually](#step-3-map-network-drives-manually)
- [Step 4: Create Batch File to Automate Drive Mapping](#step-4-create-batch-file-to-automate-drive-mapping)
- [Step 5: Customize Drive Labels](#step-5-customize-drive-labels)
- [Step 6: Delete Mapped Drives](#step-6-delete-mapped-drives)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

- NAS device with SMB shares created (e.g., `Ram` and `Public`)
- NAS IP address (e.g., `10.126.3.5`)
- Windows PC with admin rights to edit `hosts` file and run batch scripts

---

## Step 1: Configure Hosts File for Name Resolution

To avoid using IP addresses in UNC paths, map a friendly hostname (`nas-server`) to the NAS IP.

1. Open Notepad as Administrator  
   - Search for **Notepad** → Right-click → **Run as administrator**

2. Open the hosts file  
   - File > Open  
   - Navigate to: `C:\Windows\System32\drivers\etc\`  
   - Change file type filter to **All Files**  
   - Select `hosts` and open it

3. Add the following line at the bottom:
10.126.3.5 nas-server
   
4. Save and close the file

5. Test with Command Prompt:
You should see replies from `10.126.3.5`.

---

## Step 2: Verify SMB Shares on NAS

- Ensure SMB is enabled on the NAS.
- Confirm shared folders exist (e.g., `Ram`, `Public`).
- Confirm you can access via UNC path, e.g., `\\10.126.3.5\Ram`.

---

## Step 3: Map Network Drives Manually

- Open File Explorer
- Right-click **This PC** → **Map network drive**
- Assign drive letters (e.g., `Q:` for `Ram`, `P:` for `Public`)
- Use folder paths:

# NAS Drive Mapping Guide

---

## Step 4: Create Batch File to Automate Drive Mapping

1. Open Notepad  
2. Paste the following script:
# NAS Drive Mapping Guide

---

## Step 4: Create Batch File to Automate Drive Mapping

1. Open Notepad  
2. Paste the following script:

```batch
@echo off
net use Q: \\nas-server\Ram /persistent:yes
net use P: \\nas-server\Public /persistent:yes
```
Save the file as map-nas-drives.bat

File type: All Files

Location: Desktop or your preferred folder

Run the batch file by double-clicking it.

Step 5: Customize Drive Labels
To make drive labels user-friendly, add these lines to the batch file after the mapping commands:
``` batch 
powershell -Command "& {Set-Volume -DriveLetter Q -NewFileSystemLabel 'Ram SSO'}"
powershell -Command"& {Set-Volume -DriveLetter P -NewFileSystemLabel 'Public'}"
```
Note: PowerShell is required for this. You can customize the labels as needed.
Step 6: Delete Mapped Drives
To disconnect mapped drives, use:
```batch
net use Q: /delete
net use P: /delete
```

Or to delete all mapped drives:
Troubleshooting
If \\nas-server does not connect, verify the hosts file entry and try pinging the server.

Ensure SMB service is running on the NAS device.

Verify that shared folder names and permissions are correct.

For multiple PCs, consider adding DNS records on your router or local DNS server instead of editing the hosts file on each machine.

Run batch files as Administrator if needed.

Additional Notes
When mapping drives manually, check Reconnect at sign-in if you want the drives to reconnect automatically after reboot.

Finish by verifying that mapped drives appear correctly in File Explorer''''

