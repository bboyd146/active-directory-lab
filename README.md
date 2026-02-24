# 🏢 Active Directory Home Lab (ARM Virtualization on Apple Silicon)

## 📌 Project Overview

This project documents the deployment of a fully functional **Active Directory domain environment** built on an Apple Silicon (M1) Mac using ARM virtualization.

The lab simulates a real-world enterprise network environment including:

- Active Directory Domain Controller
- Internal DNS & Kerberos authentication
- Windows 11 Pro domain-joined client
- User & group management
- Help desk scenario simulations
- DNS troubleshooting & authentication validation (coming soon)

This environment was built to gain hands-on experience with enterprise identity management and help desk operations.

---

## 🖥️ Environment Architecture

### Host Machine
- Mac (Apple Silicon - M1)
- UTM Virtualization (ARM-based)

### Domain Controller (DC01)
- Ubuntu Server 24.04 ARM64
- Samba Active Directory Domain Controller
- Internal DNS (SAMBA_INTERNAL)
- Kerberos Authentication

### Client Machine (PC01)
- Windows 11 Pro ARM
- Domain-joined to `pixelpioneer.local`

---

## 🌐 Domain Information

- Domain Name: `pixelpioneer.local`
- NetBIOS Name: `PIXELPIONEER`
- Domain Controller Hostname: `dc01`
- DC IP Address: `192.168.64.3`

---

## 🔐 Technologies Implemented

- Samba Active Directory Domain Services
- DNS (AD-integrated)
- Kerberos Authentication
- Windows Domain Join
- PowerShell
- User & Group Management
- Organizational Units (OU)
- Local Administrator Management
- Network Troubleshooting (IPv4/IPv6 DNS resolution)

---

## 🧱 Implementation Steps

### 1️⃣ Domain Controller Deployment
- Installed Ubuntu Server 24.04 ARM64
- Configured static IP
- Installed Samba AD DC packages
- Provisioned domain using:

```bash
sudo samba-tool domain provision --use-rfc2307 --interactive
```
- Configured domain parameters:
  - Realm: PIXELPIONEER.LOCAL
  - NetBIOS Domain: PIXELPIONEER
  - Server Role: Domain Controller (dc)
  - DNS Backend: SAMBA_INTERNAL
  - DNS Forwarder: 8.8.8.8

- Replaced Kerberos configuration:
```bash
sudo cp /var/lib/samba/private/krb5.conf /etc/krb5.conf
```

- Enabled and started the Active Directory service:
```bash
sudo systemctl disable smbd nmbd winbind
sudo systemctl enable samba-ad-dc
sudo systemctl start samba-ad-dc
```

- Verified service status:
```bash
sudo systemctl status samba-ad-dc
```

- Confirmed Kerberos authentication:
```bash
kinit administrator
klist
```

-Successfully obtained a Kerberos ticket for:
```bash
administrator@PIXELPIONEER.LOCAL
```

### 2️⃣ User & Group Configuration

- Created domain users:
 ```bash
  sudo samba-tool user create brad.it
```
- Created security group:
  ```bash
  sudo samba-tool group add IT_admins
  ```
- Added user to group:
  ```bash
  sudo samba-tool group addmembers IT_Admins brad.it
```

- Verified group membership:
```bash
  sudo samba-tool group listmembers IT_Admins
```

### 3️⃣ Windows 11 Pro Domain Join

- Installed Windows 11 Pro ARM in UTM
- Configured DNS to point to Domain Controller:
    - 192.168.64.3

- Validated DNS resolution:
```bash
nslookup pixelpioneer.local
```

- Confirmed domain resolution to:
```bash
192.168.64.3
```

- Joined Windows machine to domain:
```bash
pixelpioneer.local
```

- Authenticated using:
  ```bash
  PIXELPIONEER\Administrator
```

- Rebooted system and logged in as:
```bash
PIXELPIONEER\brad.it
```

### 4️⃣ Domain Authentication Validation

- Confirmed successful domain authentication:
```PowerShell
whoami
```
Output:
```PowerShell
pixelpioneer\brad.it
```

- Verified logon server:
  ```PowerShell
  $env:LOGONSERVER
  ```
  Output:
  ```PowerShell
  \\DS01
  ```

  ### 5️⃣ Help Desk Senerio Simulations

  🔐 Password Reset
  ```bash
  sudo samba-tool user setpassword brad.it
  ```
  Successfully forced password updated and validated login.

  🚫 Account Disable / Enable
  ```bash
  sudo samba-tool user disable brad.it
  sudo samba-tool user enable brad.it
```
  Verified authentication failure and restoration.

  👥 Group Membership Troubleshooting

  Validated membership and access control:
```bash
sudo samba-tool group listmembers IT_Admins
```

## 🌐 DNS Troubleshooting (Real-World Scenario)

### Issue encountered:

- Windows client prioritized IPv6 DNS

- Domain resolution failed

### Resolution steps:

- Disabled IPv6 adapter preference

- Manually configured IPv4 DNS

- Flushed DNS cache

- Revalidated resolution

This simulated real-world help desk DNS troubleshooting.

## 🧠 Skills Demonstrated

- Active Directory Deployment (Samba AD DC)

- Kerberos Authentication Configuration

- Internal DNS Setup

- Windows Domain Join

- Identity & Access Management

- User Lifecycle Management

- Group-Based Access Control (RBAC)

- Network Troubleshooting

- IPv4 / IPv6 DNS Diagnostics

- PowerShell Administration

- ARM Virtualization Infrastructure

## 📸 Screenshots

(Add screenshots below)

Recommended captures:

- systemctl status samba-ad-dc

- klist Kerberos ticket output

- Windows domain join success message

- whoami showing domain user

- $env:LOGONSERVER

- nslookup pixelpioneer.local

- User disable / enable test

## 🎯 Professional Relevance

This lab environment simulates real enterprise IT operations including:

- Domain-based authentication

- Account lifecycle management

- Group-based access control

- DNS troubleshooting

- Kerberos validation

- Multi-VM network configuration

This hands-on deployment demonstrates applied Active Directory knowledge beyond theoretical coursework.

## 🚀 Future Enhancements

Planned expansions:

Implement Group Policy Objects (GPO)

Create Organizational Unit (OU) hierarchy

Deploy file server with NTFS permissions

Add second Windows client

Configure domain password policies

Implement auditing & logging

Simulate help desk ticket documentation workflow

## Author

Bradley Boyd
Computer Science Student

