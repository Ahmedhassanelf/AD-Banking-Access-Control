# 🏦 AD-Banking-Access-Control
#  enterprise IAM & RBAC using Active Directory, Kerberos auth, and LDAP — Banking environment lab

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-Windows%20Server-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Kerberos](https://img.shields.io/badge/Auth-Kerberos%20GSSAPI-red?style=for-the-badge)
![LDAP](https://img.shields.io/badge/Protocol-LDAP3-green?style=for-the-badge)
![Security](https://img.shields.io/badge/Field-Cybersecurity-black?style=for-the-badge&logo=hackthebox&logoColor=white)

**A role-based access control (RBAC) system integrated with Windows Active Directory, demonstrating enterprise-grade identity and access management (IAM) concepts through a simulated banking environment.**

</div>

---

## 📌 Project Overview

This project simulates a **real-world enterprise IAM scenario** inside a banking organization. It leverages **Microsoft Active Directory** as the identity provider, **Kerberos (GSSAPI)** for authentication, and **LDAP** for group membership queries — enforcing strict folder-level access control based on user roles.

> **Cybersecurity Context:** This project directly applies core IAM principles tested in certifications like CompTIA Security+, CEH, and OSCP — including Kerberos ticket-based auth, LDAP enumeration, and least-privilege access enforcement.

---

## 🔐 Security Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AD Domain: AhmedHassan.com               │
│                    DC: DC1.AhmedHassan.com                  │
├─────────────────────────────────────────────────────────────┤
│  Authentication Layer:  Kerberos v5 (GSSAPI/SASL)          │
│  Directory Protocol:    LDAP3                               │
│  Authorization Model:   RBAC via AD Group Membership        │
└─────────────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                  GROUP → FOLDER MAPPING                     │
├──────────────────┬──────────────────────────────────────────┤
│  Admins          │  C:\Banking\Administrator                │
│  Employees       │  C:\Banking\Employees                   │
│  Customers       │  C:\Banking\customers                   │
└──────────────────┴──────────────────────────────────────────┘
```

---

## 🗂️ AD Structure (LDAP Distinguished Names)

| Group      | DN                                                         |
|------------|------------------------------------------------------------|
| Admins     | `CN=Admins,OU=managers,DC=AhmedHassan,DC=com`             |
| Employees  | `CN=Employees,OU=Employees,DC=AhmedHassan,DC=com`         |
| Customers  | `CN=customers,OU=customers,DC=AhmedHassan,DC=com`         |

---

## 🖥️ Application Screenshots

### 1️⃣ Login Portal — System Overview & Authentication Panel

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 48 01 AM" src="https://github.com/user-attachments/assets/2547697c-9f8c-46ba-a11f-b7b6c9eb216c" />


> **What's happening here:** The main login window shows two panels — a **System Overview** (left) listing domain info, defined AD groups, folder paths, and test accounts — and an **Authentication Portal** (right) for credential entry.
>
> **Cybersecurity relevance:** This mirrors what a penetration tester would see when assessing an enterprise portal — the domain name, DC hostname, and group structure are all visible. In a real engagement, this info would be enumerated via **LDAP null binds** or **BloodHound**. Here it's intentionally exposed for lab transparency.

---

### 2️⃣ Domain Controller Connectivity Test

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 49 19 AM" src="https://github.com/user-attachments/assets/af052333-ebe1-4064-a657-b442ab2c8660" />


> **What's happening here:** Clicking "TEST CONNECTION" sends ICMP (ping) probes to `DC1.AhmedHassan.com`. A successful 2-packet response confirms the DC is reachable.
>
> **Cybersecurity relevance:** Network reachability to a DC is a prerequisite for both **legitimate Kerberos authentication** and **adversarial techniques** like Pass-the-Hash, Kerberoasting, or AS-REP Roasting. This step validates the lab environment before auth attempts.

---

### 3️⃣ Help & Documentation Window

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 49 27 AM" src="https://github.com/user-attachments/assets/4166802a-5c30-4e52-99c8-28528bbbd638" />


> **What's happening here:** A documentation panel explaining the authentication flow, group-based access model, folder structure, and troubleshooting steps.
>
> **Cybersecurity relevance:** Proper documentation is a key part of **security posture**. Knowing the exact group-to-folder mapping helps admins enforce the **principle of least privilege** — each group can only access what it needs.

---

### 4️⃣ User Authentication — Customer Account

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 49 35 AM" src="https://github.com/user-attachments/assets/1afa0873-8b77-4570-bf65-0b65fcd4038c" />


> **What's happening here:** A domain user (`mohamed.yousef`) enters credentials. The system will bind to the LDAP server using **Kerberos GSSAPI** and query the user's `memberOf` attribute to determine group membership.
>
> **Cybersecurity relevance:** This simulates how **LDAP queries** work post-authentication in enterprise environments. Attackers can abuse this with tools like `ldapsearch` or **BloodHound** to enumerate group memberships and plan privilege escalation paths.

---

### 5️⃣ Successful Kerberos Authentication

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 49 48 AM" src="https://github.com/user-attachments/assets/99cf8057-d160-4e1e-9208-d3d89cfeaa8d" />


> **What's happening here:** Kerberos (GSSAPI) authentication succeeded for `mohamed.yousef`. The dialog confirms the authentication method, domain, and server.
>
> **Cybersecurity relevance:** **Kerberos** is the default authentication protocol in Active Directory environments. Understanding how it works — TGTs, service tickets, and the KDC role — is essential for both defenders and red teamers. Attacks like **Kerberoasting** and **Golden Ticket** target this exact mechanism.

---

### 6️⃣ Customer Access — Folder Access Control in Action

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 50 07 AM" src="https://github.com/user-attachments/assets/e50dce19-3100-4b08-b793-faee5200120a" />


> **What's happening here:** After authentication, `mohamed.yousef` (a Customer) is shown **only** the `C:\Banking\customers` folder. No admin or employee paths are visible.
>
> **Cybersecurity relevance:** This demonstrates **least-privilege access control** — a fundamental security principle. In a real attack scenario, a compromised customer account should have limited blast radius. This is the RBAC model working correctly.

---

### 7️⃣ Customer Folder Contents

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 50 23 AM" src="https://github.com/user-attachments/assets/8f668701-4794-46b9-9edb-d16af02a9820" />


> **What's happening here:** The customers folder contains `account_details.txt`, `feedback.txt`, and `transaction_history.txt` — data appropriate for customer-level access.
>
> **Cybersecurity relevance:** **Data classification** and **access segmentation** are critical in banking environments, regulated by standards like **PCI-DSS** and **ISO 27001**. Customers should only see their own data — no admin logs, no employee records.

---

### 8️⃣ NTFS Permissions — No Unauthorized Write Access

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 50 51 AM" src="https://github.com/user-attachments/assets/921f0f8d-997c-41a7-b413-cec9a6a80fd4" />


> **What's happening here:** Right-clicking in the customers folder shows standard Windows context menu options. NTFS permissions restrict what users can actually do inside the folder.
>
> **Cybersecurity relevance:** Even if a user can **view** a folder, NTFS ACLs (Access Control Lists) control read/write/execute permissions independently. This is a second layer of defense — **defense in depth**. Testing these ACLs is a standard step in **Active Directory security assessments**.

---

### 9️⃣ Employee Login — Switching User Context

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 50 56 AM" src="https://github.com/user-attachments/assets/bed02779-0219-4488-a33b-71577c610ac1" />


> **What's happening here:** A different user (`Employee1`) logs in. The system re-authenticates via Kerberos and re-queries AD group membership.
>
> **Cybersecurity relevance:** Each authentication session is **independent** — there's no session sharing or token reuse between users. This matters in the context of **token impersonation attacks** (e.g., `Incognito`, `RunAs`) used in post-exploitation.

---

### 🔟 Employee Access — Role Isolation Confirmed

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 51 06 AM" src="https://github.com/user-attachments/assets/2894294a-1b82-44b9-9ca0-8bf454888833" />


> **What's happening here:** `Employee1` can only see `C:\Banking\Employees` — completely isolated from customer and admin folders.
>
> **Cybersecurity relevance:** This validates **horizontal privilege separation** between roles. In a real environment, misconfigured AD permissions (e.g., over-permissive `GenericAll` or `WriteDACL`) could allow lateral movement between these groups — a classic finding in AD penetration tests.

---

### 1️⃣1️⃣ Admin Authentication Success

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 51 15 AM" src="https://github.com/user-attachments/assets/5b084cd2-8259-4977-8568-0bc2d8a4c262" />


> **What's happening here:** Admin user `Admin1` authenticates successfully via Kerberos. The system confirms authentication method and domain.
>
> **Cybersecurity relevance:** **Admin accounts in AD** are high-value targets. They often have `AdminSDHolder` protection and reside in privileged groups. This simulates why **tiered admin models** (Tier 0/1/2) exist — to protect domain admin credentials from being used on workstations.

---

### 1️⃣2️⃣ Admin Access — Privileged Folder View

<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 51 35 AM" src="https://github.com/user-attachments/assets/52b199e2-96f6-4d2c-85b5-6ae15d831902" />


> **What's happening here:** `Admin1` can access `C:\Banking\Administrator` — the highest-privilege folder containing sensitive system files.
>
> **Cybersecurity relevance:** Admin-tier access isolation is the foundation of **Active Directory tiering**. If an admin account is compromised, the attacker gains access to critical data. This is why **Privileged Access Workstations (PAWs)** and **Just-In-Time (JIT) access** are recommended in hardened AD environments.

---

### 1️⃣3️⃣ Admin Folder Contents — Sensitive System Files


<img width="1440" height="900" alt="Screenshot 1447-11-01 at 6 51 57 AM" src="https://github.com/user-attachments/assets/b8d6ba9e-1908-45bd-8cfb-24d7361c2a07" />

> **What's happening here:** The administrator folder contains `daily_logs.txt`, `platform_settings.txt`, `security_policies.txt`, and `user_permissions.txt`.
>
> **Cybersecurity relevance:** These file types represent **critical assets** in any organization. In a red team engagement, accessing `security_policies` or `user_permissions` files would provide an attacker with a full map of the environment's defenses — making **data-at-rest protection** and **folder-level ACLs** essential countermeasures.

---

## ⚙️ Technical Stack

| Component       | Technology                            |
|-----------------|---------------------------------------|
| Language        | Python 3.8+                           |
| GUI Framework   | Tkinter                               |
| AD Protocol     | LDAP3                                 |
| Authentication  | Kerberos v5 via GSSAPI/SASL           |
| Directory       | Windows Active Directory (2019/2022)  |
| OS              | Windows 10/11 Enterprise (Domain-joined) |
| Network Test    | ICMP / subprocess ping                |

---

## 🚀 Setup & Requirements

### Prerequisites

```bash
pip install ldap3
pip install gssapi
```

### AD Requirements

1. Windows Server with Active Directory Domain Services (AD DS)
2. Domain: `AhmedHassan.com` / DC: `DC1.AhmedHassan.com`
3. Create the following OUs and Groups in AD:
   - `OU=managers` → `CN=Admins`
   - `OU=Employees` → `CN=Employees`
   - `OU=customers` → `CN=customers`
4. Create folders on the domain machine:
   ```
   C:\Banking\Administrator
   C:\Banking\Employees
   C:\Banking\customers
   ```
5. Ensure the client machine is **domain-joined**

### Run the Application

```bash
python AD_Banking_Login.py
```

---

## 👥 Test Accounts

| Role       | Users                                      |
|------------|--------------------------------------------|
| Admins     | `Administrator`, `AdminUser1`, `AdminUser2`, `Admin1` |
| Employees  | `Employee1`, `Employee2`, `Employee3`      |
| Customers  | `user1`, `user2`, `user3`, `user4`, `mohamed.yousef` |

---

## 🛡️ Cybersecurity Concepts Demonstrated

| Concept | Description |
|---------|-------------|
| **IAM (Identity & Access Management)** | Centralized identity via AD, access decisions based on group membership |
| **Kerberos Authentication** | Ticket-based SSO protocol — the backbone of Windows domain auth |
| **LDAP Enumeration** | Querying `memberOf` attribute to determine user privileges |
| **RBAC (Role-Based Access Control)** | Permissions tied to roles, not individuals |
| **Least Privilege Principle** | Each user sees only what their role permits — nothing more |
| **Defense in Depth** | LDAP group check + NTFS ACLs = two independent access control layers |
| **Privilege Separation** | Admins, Employees, and Customers are fully isolated |
| **AD Tiering Model** | Simulates Tier 0 (Admins) vs Tier 1 (Employees) vs Tier 2 (Customers) |

---

## 📁 Project Structure

```
AD-Banking-Access-Control/
├── README.md
├── AD_Banking_Login.py          # Main application
└── screenshots/
    ├── 01_login_portal.png
    ├── 02_connection_test.png
    ├── 03_help_window.png
    ├── 04_customer_login.png
    ├── 05_auth_success.png
    ├── 06_customer_access.png
    ├── 07_customer_folder_files.png
    ├── 08_customer_context_menu.png
    ├── 09_employee_login.png
    ├── 10_employee_access.png
    ├── 11_admin_auth.png
    ├── 12_admin_access.png
    └── 13_admin_folder_files.png
```

---

## ⚠️ Disclaimer

> This project is developed **for educational and portfolio purposes only**, as part of a cybersecurity specialization curriculum. All testing was performed in an **isolated, controlled lab environment**. The techniques demonstrated here (LDAP enumeration, Kerberos auth flow) are standard knowledge for **defensive security practitioners**. Do not use any concepts from this project on systems you do not own or have explicit written permission to test.

---

## 👨‍💻 Author

**Cybersecurity Specialist | Penetration Tester**

> *"Understanding how access control works from the inside is the first step to breaking it — and the only way to defend it properly."*

---

<div align="center">

⭐ If you found this project useful for learning AD security, give it a star!

</div>
