When hitting a URL, the **`hosts` file** comes into play before the **`resolv.conf`** file. Here's the sequence:

1. **`hosts` file**:  
   The system first checks the `hosts` file (located at `/etc/hosts` on Linux or `C:\Windows\System32\drivers\etc\hosts` on Windows). If there is a mapping for the URL or hostname to an IP address, the system uses that and skips further DNS resolution.

2. **`resolv.conf` file**:  
   If the `hosts` file does not contain an entry for the URL, the system refers to the DNS servers specified in the `/etc/resolv.conf` file to resolve the hostname to an IP address.

### Why this happens:
This behavior is defined by the **name resolution order**, which is typically configured in the **`nsswitch.conf`** file on Linux systems. It specifies the order of hostname resolution methods. For example:
```
hosts: files dns
```
This means:
1. First, check the `hosts` file.
2. If not found, query the DNS servers (using `/etc/resolv.conf`).

### Example:
#### **`/etc/hosts` file:**
```plaintext
127.0.0.1 localhost
192.168.1.10 example.com
```
If you try to access `example.com`, it will resolve to `192.168.1.10` without querying the DNS servers.

#### **`/etc/resolv.conf` file:**
```plaintext
nameserver 8.8.8.8
nameserver 1.1.1.1
```
If `example.com` is not found in the `hosts` file, the system queries `8.8.8.8` or `1.1.1.1` for DNS resolution.

### Summary:
- **`hosts` file**: Checked first for manual hostname-to-IP mappings.
- **`resolv.conf`**: Used for DNS lookups if the `hosts` file doesn't contain the entry.
