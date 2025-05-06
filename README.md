# Banana Pi R2 PRO – Setup & Flashing Guide

This repository provides a full guide on how to prepare and flash the **Banana Pi R2 PRO (Rockchip RK3568B2)** single-board computer. It includes instructions for OS installation, module setup, bug fixes, and ready-to-use scripts and tools.

## ✨ Purpose

The goal is to document a working and repeatable method to get the BPI R2 PRO up and running with a Linux-based OS, including firmware flashing and module installation.

---

## 🚀 Flashing OS to BPI R2 PRO

### 📥 Downloading Required Resources

> ✅ **All required files can also be found in the `resources` folder in this repository.**

1. **Download and install Rockchip USB Driver**  
   [Link](https://download.banana-pi.dev/d/ca025d76afd448aabc63/files/?p=%2FTools%2Fimage_download_tools%2FDriverAssitant_v5.11.zip)  
   ![Install USB Driver](img/usb_driver_install.png)

2. **Download RKDevTool v2.84**  
   [Link](https://download.banana-pi.dev/d/ca025d76afd448aabc63/files/?p=%2FTools%2Fimage_download_tools%2FUpdate-EMMC-Tools.zip)

3. **Download OS image**  
   [Link](https://github.com/radxa-build/rock-3a/releases/)  
   For example: `rock-3a_debian_bullseye_xfce_b25.img`

4. **Download loader (bootloader)**  
   - Default:  
     [rk356x_spl_loader_ddr1056_v1.10.111.bin](https://dl.radxa.com/rock3/images/loader/rk356x_spl_loader_ddr1056_v1.06.110.bin)

   - For RK3568 SB:  
     [rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin](https://dl.radxa.com/rock3/images/loader/rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin)

---

### 🔌 Preparing BPI R2 PRO for Flashing

1. Open **RKDevTool_Release_v2.84**  
   ![Open RKDevTool](img/open_rkdevtool.png)

2. Disconnect the power adapter from the board.

3. Connect a **dual male USB cable** from the **top USB host port** of BPI R2 PRO to your **PC**.

4. **Enter USB download mode**:
   - Press and hold the **Maskrom** button (next to 3-pin UART header)
   - Connect the power adapter (or press the **RST** button if already connected)
   - Hold the **Maskrom** button for ~2 seconds, then release it

5. Your PC should recognize the device if the Rockchip USB driver was installed correctly.  
   ![Detected module](img/detect_mudule_maskrom.png)

---

### 💾 Flashing the OS to BPI R2 PRO

All steps are done inside **RKDevTool_Release_v2.84**.

1. Go to the **"Download Image"** tab.

2. In the list, find the row with the name **loader**.  
   - Use the last column (path selector) to choose the file  
     `rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin`  
   ![Select Loader](img/select_loader.png)

3. In the next row, rename it to **image** and select the downloaded OS image file  
   `rock-3a_debian_bullseye_xfce_b25.img`  
   ![Select Image](img/select_image.png)

4. All other rows should be **unchecked or deleted** using the right-click menu.

5. Click the **"Run"** button to start flashing. The process will be displayed in the log window.  
   ![Run Flashing](img/run_flashing.png)

6. Within a few minutes, the module will be flashed. After reboot, a login screen should appear.  
   - Default login for this image:  
     **Username**: `rock`  
     **Password**: `rock`

---

## ⚠️ Problems Encountered

1. **SD card damage with SD_FIRMWARE_TOOL**  
   - Using the official SD booting tool led to **permanent damage of several SD cards**.  
   - One SD card was flashed successfully only once.  
   - Another card worked for a few flashes, then failed irreparably.

2. **Official images failed to flash**  
   - Flashing official images resulted in errors (varied per attempt).  
   - Flashing **custom images from Google Drive** worked ([link](https://drive.google.com/drive/folders/1t4KdXUPkGZ-mtYV1hGTare0nghXwOpwc)), but caused partitioning issues:
     - Only 6GB out of 14GB was available.
     - All attempts to resize or repartition failed.

---

### 🧪 Methods Attempted

#### Method 1: Flashing with SDBoot image (via "Upgrade Firmware")

- Download and extract an image built for **SDBoot**.
- Open the **"Upgrade Firmware"** tab in **RKDevTool**.
- Click **Firmware**, and select the extracted `.img` file (validation takes time).
- Connect the module in **Maskrom mode** (see instructions above).
- Press **Upgrade** to start flashing.
- After completion, the screen will display a login prompt:  
  **Login**: `bpi`  
  **Password**: `bananapi`

#### Method 2: Flashing with EMMCBoot (manual image layout)

- Download and extract an image built for **EMMCBoot**.
- Open the **"Download Image"** tab in **RKDevTool**.
- For each image component (`rootfs.img`, `recovery.img`, `misc.img`, `uboot.img`, `boot.img`, `parameter.txt`, `MiniLoaderAll.bin`), set the correct file path.
- Uncheck or delete unused rows.
- Connect the module in **Maskrom mode**.
- Press **Upgrade** to flash.
- After completion, the screen will display a login prompt:  
  **Login**: `bpi`  
  **Password**: `bananapi`
