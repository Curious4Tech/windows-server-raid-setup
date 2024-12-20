# windows-server-raid-setup
This repository provides a comprehensive technical guide on setting up **`RAID (Redundant Array of Independent Disks)`** on Windows Server. It covers RAID types, prerequisites, and step-by-step instructions using Disk Management and Storage Spaces. The guide is designed for system administrators and IT professionals.


# **Setting Up RAID on Windows Server (Including Adding Disks in VMware)**  

This guide explains how to set up RAID on Windows Server using **Disk Management** and **Storage Spaces**, with an additional section on adding virtual hard disks in **VMware Workstation/ESXi**.

---

## **Table of Contents**  

- [Introduction](#introduction)  
- [Prerequisites](#prerequisites)  
- [RAID Types Overview](#raid-types-overview)  
- [Adding Virtual Hard Disks in VMware](#adding-virtual-hard-disks-in-vmware)  
- [Option 1: Setup RAID Using Disk Management](#option-1-setup-raid-using-disk-management)  
- [Option 2: Setup RAID Using Storage Spaces](#option-2-setup-raid-using-storage-spaces)  
- [Verification and Testing](#verification-and-testing)  
- [Troubleshooting](#troubleshooting)  
- [Conclusion](#conclusion)

---

## **Introduction**  

**`RAID`** helps improve **`data redundancy`**, **`performance`**, or both by combining multiple physical or virtual disks into a single logical unit. This guide covers both setting up RAID in Windows Server and creating virtual disks in VMware.

---

## **Prerequisites**  

- **`VMware Workstation/ESXi`** installed.  
- **`Windows Server 2016/2019/2022`** or later installed as a virtual machine.  
- **`At least 2 virtual or physical hard disks`** (depending on RAID type).  
- **`Administrator permissions`**.

---

## **RAID Types Overview**  

| RAID Type  | Description                    | Minimum Disks |
|------------|--------------------------------|---------------|
| RAID 0     | Striping (Performance only)    | 2             |
| RAID 1     | Mirroring (Data redundancy)    | 2             |
| RAID 5     | Striping with parity (Balanced)| 3             |
| RAID 10    | Combination of RAID 1 & 0      | 4             |

---

## **Adding Virtual Hard Disks in VMware**  

### **1. VMware Workstation**  
1. **`Power off the Virtual Machine.`**  
2. **`Open VMware Workstation.`**  
3. **`Select the VM:`** Right-click the VM and choose **`Settings`**.  
4. **`Add a Hard Disk:`**  
   - Click **`Add...`** in the Hardware tab.

![image](https://github.com/user-attachments/assets/7ead7146-0e2a-4416-9bda-292958f9b36d)

   - Choose **`Hard Disk`** and click **`Next`**.

![image](https://github.com/user-attachments/assets/6992937a-b1aa-4aa1-a526-52f0af4136e7)

   - Choose the disk **`type`** (recommended: NVMe).

   ![image](https://github.com/user-attachments/assets/ce362dab-6aa2-463c-adc1-b7601937da7f)
   
   - Select **`Create a New Virtual Disk`** and click **`Next`**.  
 
 ![image](https://github.com/user-attachments/assets/e575c447-f79f-4845-a89d-645eaded1a03)

   - Select **`Store as a single file`** or **`Split into multiple files`** and specify the **`size`**. Click on **`Next`**. 

   ![image](https://github.com/user-attachments/assets/74a30a86-8af2-499b-baec-f520add39179)

   - Rename it without changing the extension (e,g; **`Data_Store01.vmdk`**) or let the default name and then click on **`Finish`**.

![image](https://github.com/user-attachments/assets/01bb8fe6-2fb0-4653-8eb4-c6c8ab7d115d)


5. **`Repeat the Process:`** Add additional virtual hard disks as needed. I have created **`03 disks`**.

![image](https://github.com/user-attachments/assets/04305a63-8c79-4a3c-b234-044b4047e11c)

---

### **2. VMware ESXi**  
1. **`Log in to VMware ESXi Web Interface.`**  
2. **`Select the VM:`** Click **`Virtual Machines`** and choose the target VM.  
3. **`Edit VM Settings:`**  
   - Click **`Actions`** > **`Edit Settings`**.  
   - Click **`Add New Device`** > **`New Hard Disk`**.  
   - Specify the **`Disk Size`**, **`Type`** (recommended: SCSI), and **`Provisioning`** (Thin/Thick).  
   - Click **`Save`**.  

4. **`Repeat the Process:`** Add more disks as needed.

---

## **Option 1: Setup RAID Using Disk Management**  

1. **Open Disk Management:**  
   - Press **`Windows + R`**, type **`diskmgmt.msc`**, and press **`Enter`**.  

![image](https://github.com/user-attachments/assets/e4a3830e-d6b4-4aae-b78e-e3f9f9fb6a17)

2. **Initialize Disks:**

  - Select all newly added disks and **`GPT`**, then click on **`OK`**

![image](https://github.com/user-attachments/assets/26646c89-18ea-4be0-80e2-238433057cdf)

   -They are all  **`Online`** now.  

![image](https://github.com/user-attachments/assets/addd965c-bf90-4570-b22a-76e66313c3b4)

2. **Create RAID Volume:**  
   - Right-click one of the initialized disks.  
   - Select one of the following options based on your RAID type:  
     - **`New Spanned Volume`** (RAID 0 - Striping)  
     - **`New Mirrored Volume`** (RAID 1 - Mirroring)  
     - **`New RAID-5 Volume`** (RAID 5). As we have 03 disks avaible, we will go with **`RAID 5`**  

![image](https://github.com/user-attachments/assets/98b76d63-580d-4527-94f9-b032fc811e1b)

3. **Follow the Wizard:**  
   - Select the disk to include and click on **`Add`** to all the disks.

![image](https://github.com/user-attachments/assets/e9f1f5b5-5bb4-48fd-baab-6c9c0c680af5)

   - Assign a drive letter and click on **`Next`**.

![image](https://github.com/user-attachments/assets/9b178b53-2f71-4235-9da9-bac0f6d9dd2b)


   - Format with **`NTFS`** or **`ReFS`**.

![image](https://github.com/user-attachments/assets/25087209-c195-4062-9217-d526bff689de)


   - Click **`Finish`**.

![image](https://github.com/user-attachments/assets/eb56c7f8-09a8-43b0-ad82-524df3e5ed95)

  - If a pop-pup open, just click on **`Yes`**.

![image](https://github.com/user-attachments/assets/880df453-d871-4036-a176-8571762de7a2)

 - Your **`RAID-5`** has been created  and running smoothly.

![image](https://github.com/user-attachments/assets/52b2ccae-70ef-4440-a8a3-b4adaba0a161)

 - Now let's change the name of the RAID-5 volume from **`New Volume`** to **`RAID5-DEMO`**. Just a simple renaming.

![image](https://github.com/user-attachments/assets/37510bab-e150-4398-87f7-faf2a7c79f61)

---

## **Option 2: Setup RAID Using Storage Spaces**  

1. **Open Server Manager:**  
   - Go to **`File and Storage Services`** > **`Storage Pools`**.  
   - Click **`Tasks`** > **`New Storage Pool`**.

![image](https://github.com/user-attachments/assets/b8057412-d986-4d11-bc6e-bd9d5cb83f0b)

2. **Create Storage Pool:**  
   - Provide a **`Pool Name`** and select the disks.  
   - Click **`Create`**.

3. **Create a Virtual Disk:**  
   - In **`Storage Pools`**, click **`Tasks`** > **`New Virtual Disk`**.  
   - Select the pool created earlier.  
   - Choose a RAID type:  
     - **`Simple (RAID 0)`**  
     - **`Two-way Mirror (RAID 1)`**  
     - **`Three-way Mirror (Advanced Redundancy)`**  
     - **`Parity (RAID 5)`**  

4. **Configure Disk Settings:**  
   - Set the **`Size`** and **`Drive Letter`**.  
   - Format the disk as **`NTFS`** or **`ReFS`**.  
   - Click **`Create`**.

---

## **Verification and Testing**  

- Open **`File Explorer`** and check the newly created RAID drive.  
- Save test files to verify storage functionality.  
- Simulate drive failure (if using RAID 1/5/10) by disconnecting one virtual disk and checking data integrity.

---

## **Troubleshooting**  

- **`Disks Not Detected:`** Ensure the disks are connected, initialized, and visible in **Disk Management** or **Storage Spaces**.  
- **`RAID Setup Fails:`** Recheck VMware disk configuration and supported RAID levels.  
- **`Performance Issues:`** Consider using hardware RAID if available.

---

## **Conclusion**  

By following this guide, you can set up RAID on a Windows Server installed in VMware or on a physical server. This configuration improves storage redundancy, performance, or both, depending on the RAID type.
