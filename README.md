# DISA STIG Finding & Remediation

![Security-Technical-Implementation-Guide](https://github.com/user-attachments/assets/bac81805-c132-4f6b-8955-d098fb9764ca)


## Summary 
This project documents the remediation of the **Windows 11 STIG finding WN11 AU 000500**. The project demonstrates two methods that can be used for the remedation:

1. **Group Policy configuration**  
2. **Registry based remediation with PowerShell**.

The **Windows Application event log maximum size** must be set to **32768 KB or greater** to support auditing and investigation needs.

### Why this matters
If the Application event log is too small, it can overwrite older events quickly. That reduces visibility during investigations and makes audit retention weaker.

---

## 1st Remediation Method - Group Policy Configuration



## STEP 1


**The initial Tenable scan flagged STIG finding WN11 AU 000500 as Failed**

---

   <img width="1539" height="418" alt="Pic1" src="https://github.com/user-attachments/assets/76f7009d-e462-46ed-ae5e-d88d3a36d895" />

---

## STEP 2 



**Additional remediation guidance was found in STIG-A-VIEW, including the required check and fix steps for this finding.**

---

   <img width="1859" height="621" alt="Pic2" src="https://github.com/user-attachments/assets/3439b8c5-2dd8-445f-89d9-f043c2f37ae9" />

---

## STEP 3



**The remediation process started by applying the guidance from STIG-A-VIEW**

---

  **1. Computer Configuration >>**
  
  **2. Administrative Templates >>**
  
  **3. Windows Components >>**
  
  **4. Event Log Service >>**

  **5. Application >>**
  
---

   <img width="1561" height="875" alt="Pic3" src="https://github.com/user-attachments/assets/a560b445-5e99-421d-adc0-c17bc2ac44dc" />

---

## STEP 4



**Maximum log file size was set "Enabled" with a "Maximum Log Size (KB)" of "32768" or greater**

---

<img width="836" height="571" alt="Screenshot 2026-01-22 145856" src="https://github.com/user-attachments/assets/f7e6880c-6768-4454-8590-ae9095736847" />

---

## STEP 5



**Visual confirmation of the update**

---

   <img width="821" height="193" alt="Pic5" src="https://github.com/user-attachments/assets/89f3725a-9343-4056-9e68-d885000bcd9e" />

---

## STEP 6



**After remediation, Tenable marked STIG WN11 AU 000500 as Passed**

---

   <img width="1572" height="238" alt="Pic6" src="https://github.com/user-attachments/assets/385dd04f-4d51-4ded-b2b4-2296bfd91ec9" />
<br><br>
<br><br>

---

## 2nd Remediation Method - Registry based remediation with PowerShell

---

**After completing the first remediation using Group Policy, the settings were reverted, reran the Tenable scan to confirm the finding returned as Failed, and then remediated it again using PowerShell.**

---

## STEP 1

**The remediation script was executed using PowerShell ISE**

---

<img width="1017" height="655" alt="Screenshot 2026-01-22 151659" src="https://github.com/user-attachments/assets/ae2fa3ef-e164-45bb-af6c-88c33b428497" />

---

## STEP 2

**Visual confirmation of the update was performed in Registry Editor:**

 **1. Software >>**
 
 **2. Policies >>**
 
 **3. Microsoft >>**
 
 **4. Windows >>**
 
 **5. EventLog >>**
 
 **6. Application >>**

 ---

 <img width="1121" height="724" alt="Screenshot 2026-01-22 152744" src="https://github.com/user-attachments/assets/b23f0b58-2317-4eb0-b362-4f62b8cd8ae4" />

---

## STEP 3



**After a rescan in Tenable, STIG WN11 AU 000500 was marked Passed**

---

   <img width="1572" height="238" alt="Pic6" src="https://github.com/user-attachments/assets/385dd04f-4d51-4ded-b2b4-2296bfd91ec9" />

---

### PowerShell Remediation Script

```powershell

# STIG ID: WN11-AU-000500
# Set Application event log maximum size to 32768 KB (32 MB)

$registryPath = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\Application"
$valueName = "MaxSize"
$valueData = 32768

# Create the registry path if it doesn't exist
if (!(Test-Path $registryPath)) {
    New-Item -Path $registryPath -Force | Out-Null
}

# Set the registry value
Set-ItemProperty -Path $registryPath -Name $valueName -Value $valueData -Type DWord

# Verify the change
$result = Get-ItemProperty -Path $registryPath -Name $valueName
Write-Host "MaxSize set to: $($result.MaxSize) KB"

```
---

## Summary

This lab focused on remediating the Windows 11 STIG finding WN11 AU 000500, which requires the Application event log maximum size to be set to 32768 KB or greater. The finding was first identified as Failed in Tenable, then remediation guidance was referenced in STIG A View to confirm the required registry path and expected value. Remediation was completed using two methods: Group Policy configuration and registry based remediation using PowerShell. Registry Editor was used to validate the applied setting, and a final Tenable rescan confirmed the control was marked Passed.

---

```text
Author        : Manny Aguirre
LinkedIn      : linkedin.com/in/mannyaguirre/
GitHub        : github.com/mannyaguirre
Date Created  : Jan 22, 2026
Last Modified : Jan 22, 2026
Version       : 1.0
STIG ID       : WN11-AU-000500
