# System Auditing & Integrity

## 1. Hashing Tools (Generating Checksums)
*How to generate a unique digital fingerprint for a file using system tools.*

### PowerShell (Windows Standard)
The primary tool is `Get-FileHash`. By default, it uses the SHA256 algorithm.

**Basic Generation:**

```powershell

Get-FileHash "C:\Path\To\File.exe".
```
**Changing the Algorithm:** Use the `-Algorithm` parameter for MD5, SHA1, or SHA512.
```powershell

Get-FileHash "C:\Path\To\File.exe" -Algorithm SHA512
```

**Formatting the Output:** Essential for scripts or reports where you need clean data.
```powershell

# List format (easier to read full paths)
Get-FileHash "C:\Path\To\File.exe" | Format-List

# Extract ONLY the hash string (useful for automation)
(Get-FileHash "C:\Path\To\File.exe").Hash
```

## 2. Integrity Verification Workflow
*Procedure to verify that a downloaded file has not been corrupted or tampered with (Man-in-the-Middle attacks).*

### The Process
1. Locate the official hash provided by the vendor on their website.
2. Run the command below on your downloaded file.
3. Compare the output hash with the vendor's hash. They must match exactly.

### Windows Verification**
**Option A: PowerShell (Recommended)**
```PowerShell

Get-FileHash "C:\Downloads\installer.msi"
```
**Option B: Command Prompt (Legacy)** Useful if PowerShell is restricted.
```DOS

certutil -hashfile "C:\Downloads\installer.msi" SHA256
```

### Linux & macOS Verification
Standard Unix tools. Match the command to the algorithm provided (e.g., md5sum, sha1sum, sha256sum).
```Bash

# For SHA256 (Most common)
sha256sum filename.tar.gz

# For MD5 (Older legacy standard)
md5sum filename.tar.gz
```

## 3. User Account Auditing
*Objective: Identify unauthorized, hidden, or default accounts that could serve as backdoors.*

### Windows
1. GUI Method: Press Win + R, type lusrmgr.msc. Review "Users" and "Groups."
2. CLI Method:
```DOS

net user
```
### Linux
1. View all local accounts:
``` Bash

cat /etc/passwd
```
2. View all accounts (including network/LDAP):
```Bash

getent passwd
```
### macOS
Uses Directory Services.
```Bash

dscl . list /Users
```
