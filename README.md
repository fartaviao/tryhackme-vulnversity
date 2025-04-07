<img src="https://raw.githubusercontent.com/fartaviao/fartaviao/refs/heads/main/Banner%20Fausto.jpg" alt="Banner Fausto Artavia Ocampo">

# Vulnversity - TryHackMe Room

## 📘 Overview

This repository contains a detailed walkthrough for the **[Vulnversity](https://tryhackme.com/room/vulnversity)** room on TryHackMe. It covers the complete penetration testing process, including:

- VPN configuration
- Nmap reconnaissance
- Web directory enumeration with Gobuster
- Web exploitation and reverse shell upload
- Privilege escalation using SUID misconfiguration

---

## 🧰 Tools Used

- `nmap` – Network reconnaissance and service enumeration
- `gobuster` – Directory brute-forcing
- `burpsuite` – Intercepting proxy and fuzzing
- `netcat` – Listening for reverse shell
- `python` – Spawn interactive TTY shell
- `find` – Enumerate SUID binaries

---

## 📁 Repository Structure

```
tryhackme-vulnversity/
├── README.md                      # Introduction and overview
├── vulnversity.md                 # Main documentation (full guide)
├── Scripts/
│   └── reverse-shell.phtml        # Reverse shell script
└── Screenshots/                   # Visual references
    ├── Screenshot-01.png
    ├── Screenshot-02.png
    ├── ...
    ├── Screenshot-37.png
    └── Screenshots.md
```

---

## 🚀 Getting Started

To follow this guide, ensure you have:
- A **TryHackMe** account ([Sign up here](https://tryhackme.com/)).
- A machine or VM with **Kali Linux** or **Parrot Security**.
- An active internet connection.

### Connect to TryHackMe VPN
- Follow this [VPN guide](https://github.com/fartaviao/tryhackme-tutorial) to connect using OpenVPN.
- Join and start the machine in the [Vulnversity Room](https://tryhackme.com/room/vulnversity)

## Documentation and Screenshots
For detailed documentation and step-by-step guide, refer to the [main documentation](https://github.com/fartaviao/tryhackme-vulnversity/blob/main/Vulnversity.md)
as well you can check the [Screenshots](https://github.com/fartaviao/tryhackme-vulnversity/blob/main/Screenshots/Screenshots.md) for better clarity of the process.

## How to Use This Repository
1. Clone this repository:
   ```bash
   git clone https://github.com/fartaviao/tryhackme-vulnversity.git
   ```
2. Navigate to the repository folder:
   ```bash
   cd tryhackme-vulnversity
   ```
3. Open `Vulnversity.md` to follow the steps.

## License
This documentation is provided for **educational purposes**. Feel free to modify and use it as needed.

### Recommended Resources:
- TryHackMe Official Documentation → [https://tryhackme.com/](https://tryhackme.com/)
- OpenVPN Documentation → [https://openvpn.net/](https://openvpn.net/)
- TryHackMe safe VPN access → [https://github.com/fartaviao/tryhackme-tutorial/blob/main/Tutorial.md](https://github.com/fartaviao/tryhackme-tutorial/blob/main/Tutorial.md)
- GTFOBins – Unix Privilege Escalation Techniques [GTFOBins](https://gtfobins.github.io/)
- pentestmonkey [GitHub](https://github.com/pentestmonkey)

### Security Considerations
- Always **disconnect the VPN** after finishing a session.
- Use **firewall rules** to prevent unauthorized access.

---

## Author
Created by **Fausto Artavia Ocampo** for educational use and cybersecurity training.

Happy hacking! 🚀
