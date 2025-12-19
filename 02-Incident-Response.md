# Incident Response: Malware Removal

## Standard Operating Procedure (SOP)
*This guide follows the industry-standard lifecycle: Identification, Containment, Eradication, and Recovery.*

---

### Phase 1: Identification & Quarantine
*Objective: Confirm the infection and stop the bleeding.*

1.  **Verify Symptoms:**
    * **Network Check:** Open a terminal/command prompt as Administrator and run:
        ```cmd
        netstat -nab
        ```
        * **Explanation:**
            * `-n`: Displays numerical addresses (no DNS lookup delays).
            * `-a`: Shows all connections and listening ports.
            * `-b`: **Crucial.** Shows the executable (program) involved in creating the connection.
        * **What to look for:** Connections to unknown external IP addresses, or familiar programs (like `svchost.exe`) communicating on non-standard ports.
    * **Process Check:** Use **Process Explorer** or Task Manager. Look for processes with high CPU usage, strange names, or missing digital signatures.
    * **Hash Check:** If you find a suspicious file, upload its hash to [VirusTotal](https://www.virustotal.com) to see if other vendors flag it.

2.  **Isolate the System:**
    * **Action:** Physically unplug the Ethernet cable and toggle Wi-Fi off.
    * **The Logic:** Malware often communicates with a "Command & Control" (C2) server to receive instructions or exfiltrate stolen data. Cutting the network kills this link.
    * **Peripherals:** Unmount and remove USB drives immediately to prevent the malware from jumping to the storage device.

---

### Phase 2: Containment
*Objective: Prevent the malware from entrenching itself further.*

3.  **Disable System Restore (Windows):**
    * **Action:** Navigate to System Properties > System Protection > Configure > **Disable system protection**.
    * **The Logic:** Clever malware creates "shadow copies" or hides inside System Restore points. If you clean the PC but leave Restore on, the malware might resurrect itself the next time Windows tries to "heal" itself. Disabling it flushes these infected backups.

---

### Phase 3: Eradication
*Objective: Remove the threat completely.*

4.  **Remediate the System:**
    * **Scenario A: System is Bootable**
        * Update your anti-malware signatures (download on a clean PC and transfer via USB if necessary).
        * Run a **Full System Scan** (not a Quick Scan).
    * **Scenario B: System Unstable / Malware Fights Back**
        * **Boot into Safe Mode:** (Restart > Hold Shift > Troubleshoot > Startup Settings > Safe Mode).
        * **The Logic:** Safe Mode only loads essential drivers. Most malware sets itself to "Auto-Start" with normal Windows. In Safe Mode, the malware likely won't start, making it easier to delete.
    * **Scenario C: The "Nuke" Option (Ransomware/Rootkits)**
        * If the malware has encrypted files or burrowed into the kernel (Rootkit), you cannot trust the OS.
        * **Action:** Backup critical data (scan it aggressively!) and perform a **Clean Install** (Wipe the drive and reinstall Windows/Linux from fresh media).

---

### Phase 4: Recovery & Lessons Learned
*Objective: Restore normal operations and prevent recurrence.*

5.  **Hardening:**
    * Enable the firewall.
    * Run Windows Update / `sudo apt upgrade` to patch the vulnerability the attacker exploited.
6.  **Re-enable System Restore:**
    * Once the system is 100% clean, turn System Protection back on and manually create a new restore point immediately.
7.  **User Education:**
    * Identify how the infection happened (e.g., "User clicked a link in an invoice email").
    * Conduct a brief training session or put up a notice to warn others of the specific tactic used.
