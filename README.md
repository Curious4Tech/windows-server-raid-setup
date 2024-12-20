# windows-server-raid-setup
This repository provides a comprehensive technical guide on setting up RAID (Redundant Array of Independent Disks) on Windows Server. It covers RAID types, prerequisites, and step-by-step instructions using Disk Management and Storage Spaces. The guide is designed for system administrators and IT professionals.


# **Setting Up RAID on Windows Server**  

This guide explains how to set up RAID (Redundant Array of Independent Disks) on Windows Server using **Disk Management** and **Storage Spaces**. The steps cover RAID types including **`RAID 0, 1, 5, and 10`**.

---

## **Table of Contents**  

- [Introduction](#introduction)  
- [Prerequisites](#prerequisites)  
- [RAID Types Overview](#raid-types-overview)  
- [Option 1: Setup RAID Using Disk Management](#option-1-setup-raid-using-disk-management)  
- [Option 2: Setup RAID Using Storage Spaces](#option-2-setup-raid-using-storage-spaces)  
- [Verification and Testing](#verification-and-testing)  
- [Troubleshooting](#troubleshooting)  
- [Conclusion](#conclusion)

---

## **Introduction**  

RAID helps improve **`data redundancy`**, **`performance`**, or both by combining multiple physical disks into a single logical unit. This guide focuses on setting up RAID in Windows Server.

---

## **Prerequisites**  

- **`Windows Server 2016/2019/2022`** or later.  
- **`At least 2 physical disks`** (depending on RAID type).  
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

## **Option 1: Setup RAID Using Disk Management**  

1. **Open Disk Management:**  
   - Press **`Windows + R`**, type `diskmgmt.msc`, and press **`Enter`**.  

2. **Initialize Disks:**  
   - Right-click each new disk and select **`Initialize Disk`**.  

3. **Create RAID Volume:**  
   - Right-click one of the initialized disks.  
   - Select one of the following options based on your RAID type:  
     - **`New Spanned Volume`** (RAID 0 - Striping)  
     - **`New Mirrored Volume`** (RAID 1 - Mirroring)  
     - **`New RAID-5 Volume`** (RAID 5)  

4. **Follow the Wizard:**  
   - Select the disks to include.  
   - Assign a drive letter.  
   - Format with **`NTFS`** or **`ReFS`**.  
   - Click **`Finish`**.

---

## **Option 2: Setup RAID Using Storage Spaces**  

1. **Open Server Manager:**  
   - Go to **`File and Storage Services`** > **`Storage Pools*`*.  

2. **Create Storage Pool:**  
   - Click **`Tasks`** > **`New Storage Pool`**.  
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
- Simulate drive failure (if using RAID 1/5/10) by disconnecting one disk and checking data integrity.

---

## **Troubleshooting**  

- **`Disks Not Detected:`** Ensure the disks are connected and initialized.  
- **`RAID Setup Fails:`** Recheck disk selection and supported RAID configurations.  
- **`Poor Performance:`** Consider using hardware RAID if available.

---

## **Conclusion**  

Setting up **`RAID`** on Windows Server can significantly improve performance and data redundancy. Follow this guide carefully to ensure your storage is configured correctly. For advanced scenarios, consider using third-party RAID controllers.
