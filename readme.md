# Subaru BRZ ECU Tuning & Flashing Guide via TOPDON RLink, FastECU & RomRaider

This guide outlines pulling your stock image, comparing it against an OpenFlash Tablet (OFT) tune, transferring the changes, and reflashing your 2013-2020 Subaru BRZ ECU.

## 1. Prerequisites & Setup
* **Hardware:** [TOPDON RLink J2534 Reprogramming Tool](https://www.amazon.ca/TOPDON-Reprogramming-High-Speed-Programmer-Diagnostic/dp/B0FS11XMH1) connected via USB-C to a Windows 10/11 laptop.
* **Software Installed:** 
  * [TOPDON RLink Platform](https://service.topdon.com/portal/en/kb/articles/subaru-oem-software-with-rlink-j2534-connection-instructions) (with J2534 DLL drivers installed and updated).
  * [FastECU](https://github.com/miikasyvanen/FastECU/releases).
  * [RomRaider](https://www.romraider.com/).
* **Definitions:** Download and place the latest Subaru BRZ/FT86 ECU definitions (e.g., `defs.xml`) into your [RomRaider](https://www.romraider.com/) and FastECU metadata directories. Ensure your vehicle's specific Calibration ID (CalID, e.g., `ZA1JA01G`) is supported.
* **Vehicle Preparation:** Ensure your BRZ's battery voltage is stable (use a tender). Plug the TOPDON RLink into the OBD-II port and the USB into your PC.

---

## 2. Pulling (Reading) the ECU Image
1. Open **FastECU**.
2. Select your vehicle profile: **Subaru -> BRZ / Scion FR-S / GT86**.
3. Choose your interface: Select the **J2534 PassThru** driver corresponding to your TOPDON RLink.
4. Turn the ignition to the **ON** position (engine off).
5. Click the **"Read"** (or Download) button. 
6. Once the ROM image is successfully dumped, save the file immediately as a `.bin` file (e.g., `BRZ_Stock_Backup.bin`). *Keep this safe; it is your baseline restore point.*

---

## 3. Comparing Tunes in RomRaider
1. Open **RomRaider**.
2. Go to **ECU Definitions -> Definition Manager** and ensure your definition file path is pointed to the folder containing your BRZ XML definitions.
3. Open your pulled stock ROM file (`BRZ_Stock_Backup.bin`) via `File -> Open`.
4. Ensure that you are using the correct definition for your base map [SubaruDefs](https://github.com/TD-D/SubaruDefs/tree/Stable).
5. Open your target **OFT Tune file** (e.g., the corresponding Stage 1 or Stage 2 `.bin` map from the [Off-The-Shelf (OTS) Tune File Downloads](https://support.openflashtablet.com/support/solutions/articles/42000015839-off-the-shelf-ots-tune-file-downloads-v4-03)). Note that the definition file for the OTS tune is different than for your base map.
6. With both ROMs open in the sidebar, compare the images by going to `Edit -> Compare Images` (e.g., *Fuel*, *Timing Advance*, *MAF Scaling*).
7. Right-click on differing tables to view them side-by-side or compare them to identify changes made by the OFT tune.

---

## 4. Copying Changes
1. For parameters you wish to transfer from the OFT file to your stock ROM:
   * Highlight/Select the cells containing the modified values in the OFT ROM table.
   * Copy the values (`Ctrl + C`).
   * Navigate to the matching table in your stock ROM, highlight the equivalent cells, and paste the values (`Ctrl + V`).
2. Alternatively, you can copy and paste the entire table.
3. Repeat this for all desired maps (e.g., *Ignition Timing*, *Target Lambda*, etc.).
4. Go to `File -> Save As` and save this newly modified stock ROM under a distinct name (e.g., `BRZ_Custom_Hybrid.bin`).

---

## 5. Reflashing the ECU
1. Open **FastECU** (or EcuFlash).
2. Open your newly modified calibration file (`BRZ_Custom_Hybrid.bin`).
3. Ensure the TOPDON RLink is still connected and the ignition is **ON**.
4. Perform a **"Test Write"** first to ensure checksums and metadata are correctly verified by the software.
5. Once the test write succeeds, click **"Write"** (or Flash) to upload the modified calibration to your BRZ ECU.
6. **Important:** Do not unplug the cable, turn off the ignition, or disrupt power while writing.
7. Once the flash completes successfully, turn the ignition **OFF** for 10 seconds, then start the car to verify smooth idle and clear any temporary trouble codes (CELs) using the software.
