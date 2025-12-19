# Lab Environment Setup & Tools

## 1. Hypervisor Maintenance (VirtualBox)
*Keep the virtualization software updated to ensure stability and USB compatibility.*

**Checking for Updates:**
1.  Open **Oracle VM VirtualBox Manager**.
2.  Click on **File** (top left menu).
3.  Select **Check for Updates**.
4.  If a new version is found, follow the prompts to download and install.
    * *Note: You may need to update the "VirtualBox Extension Pack" separately if prompted.*

---

## 2. Kali Linux Maintenance
*Regular maintenance ensures you have the latest security patches and tool repositories.*

### Updating the System
Run this regularly to keep the distribution and tools current.
```bash
sudo apt update && sudo apt upgrade -y
```
**Troubleshooting Repositories**
If updates fail or the connection times out (common in some regions):

1. Run the configuration tool:

```Bash

kali-tweaks
```

2. Navigate to Network Repositories.

3. Ensure mirrors are set to use HTTPS (or change mirrors if the download speed is too slow).

### Desktop Environment
To install the GNOME desktop environment for a more modern UI experience:

```Bash

sudo apt install kali-desktop-gnome -y
```

## 3. Tool Installation: RsaCtfTool
Used for RSA attacks in CTF challenges.

### Prerequisites
Ensure Git and Python environment tools are installed.

```Bash

sudo apt update
sudo apt install git python3 python3-pip python3-venv -y
```

### Installation
```Bash

# Clone the repository
git clone [https://github.com/RsaCtfTool/RsaCtfTool.git](https://github.com/RsaCtfTool/RsaCtfTool.git)
cd RsaCtfTool

# Create a Python Virtual Environment (Best Practice)
python3 -m venv venv
source venv/bin/activate

# Install Dependencies
pip install -r requirements.txt
```
### Troubleshooting `gmpy2`
If the installation fails on `gmpy2` (a common math library error), install the required system libraries:

```Bash

sudo apt install build-essential libgmp-dev libmpfr-dev libmpc-dev -y
pip install gmpy2
```

### Verification
Run the help command to confirm successful installation.

```Bash

python3 RsaCtfTool.py --help
```
