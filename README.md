# 🌐 Web Security Self-Study — Web Red Module

My personal notes while studying Web Security, covering URL structure, DNS, cURL, HTTPS/TLS, HTTP methods, JSON, and Nmap.

---

## 🔗 1. URL Structure (Uniform Resource Locator)

**Definition:** A URL is an address used to locate a resource on the internet.

**Example:**

```text
https://example.com:443/Products?id=10#details
```

| Component | Description |
|---|---|
| `https` | Scheme — defines the protocol used |
| `example.com` | Domain / Host — identifies the server |
| `443` | Port — the default port for HTTPS |
| `/Products` | Path — identifies the requested resource |
| `?id=10` | Query String — passes parameters to the server |
| `#details` | Fragment — identifies a specific section of the page |

**Important Note:** The fragment (`#details`) is handled by the browser and is not sent to the server in the HTTP request.

---

## 🔍 2. DNS (Domain Name System)

**Definition:** DNS translates domain names into IP addresses so devices can locate and communicate with servers.

### How DNS Works

1. The user enters `google.com` in the browser.
2. The device looks for the corresponding IP address, using its DNS resolver.
3. The DNS resolver returns the IP address.
4. The browser connects to the destination server and sends an HTTP or HTTPS request.

**Example:**

```text
Domain Name → DNS Resolution → IP Address → HTTP/HTTPS Request
```

---

## 🛠️ 3. cURL (Client URL)

**Definition:** cURL is a command-line tool used to transfer data to and from servers using various protocols, including HTTP and HTTPS.

### Common cURL Commands

| Command | Description |
|---|---|
| `curl https://example.com` | Sends a request and displays the response body |
| `curl -o index.html https://example.com` | Saves the response to a file named `index.html` |
| `curl -O https://example.com/file.zip` | Saves the file using its remote filename |
| `curl --help` | Displays available options |
| `curl -k https://example.com` | Skips TLS certificate verification (insecure) |
| `curl -v https://example.com` | Displays detailed connection and request information |
| `curl -I https://example.com` | Sends a HEAD request to retrieve response headers only |
| `curl -i https://example.com` | Displays response headers along with the response body |
| `curl -s https://example.com` | Runs in silent mode, suppressing progress output |
| `curl -H "Header: Value" https://example.com` | Adds a custom HTTP header |
| `curl -u username:password https://example.com` | Supplies HTTP authentication credentials |
| `curl -X POST https://example.com` | Sends a POST request |
| `curl -d "username=omar&password=123" https://example.com` | Sends data in the request body |

### Example: Sending JSON Data

```bash
curl -X POST https://example.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"omar","password":"example"}'
```

**Security Notes:**
- Avoid using `-k` in production because it disables certificate verification.
- Do not put real passwords or secrets directly in commands that may be saved in shell history.
- `-I` sends a HEAD request, while `-i` includes the response headers in the displayed output.

---

## 🔒 4. HTTPS & Security Mechanics

### What Is HTTPS?

HTTPS is HTTP secured using TLS (Transport Layer Security).

- **HTTP:** Commonly uses port `80`.
- **HTTPS:** Commonly uses port `443`.
- **TLS:** Protects communication through encryption, integrity checks, and server authentication.

### SHA-256

SHA-256 is a cryptographic hash function that produces a **256-bit (32-byte) hash**.

**Important:** Hashing is not encryption. A hash function is designed to be one-way, meaning the original input cannot feasibly be recovered directly from the hash.

### Why SHA-256 Is Used in Digital Certificates

- **Security:** Provides strong protection against practical collision attacks.
- **Compatibility:** Widely supported by modern certificate and security systems.
- **Efficiency:** Offers practical performance for certificate and signature verification.

**Correction:** SHA-256 does not make certificate signatures smaller simply because it is used. The signature size depends primarily on the signature algorithm and key type. SHA-256 produces a 256-bit digest.

### TLS Handshake — Simplified Flow

1. **Client Hello:** The client proposes supported TLS versions, cipher suites, and other parameters.
2. **Server Hello:** The server selects the connection parameters.
3. **Server Authentication:** The server provides its certificate when required, and the client validates the certificate and server identity.
4. **Key Exchange:** The client and server establish shared session keys using the negotiated key exchange mechanism.
5. **Finished Messages:** Both sides verify the handshake.
6. **Encrypted Communication:** Application data, such as HTTP requests and responses, is protected using the negotiated session keys.

**Note:** The exact handshake sequence depends on the TLS version. In TLS 1.3, the handshake is streamlined compared with TLS 1.2.

---

## 📋 5. CRUD Operations & HTTP Methods

CRUD stands for **Create, Read, Update, and Delete** — the four basic operations used to manage data.

| CRUD Operation | HTTP Method | Description |
|---|---|---|
| Create | `POST` | Creates a new resource |
| Read | `GET` | Retrieves a resource |
| Update | `PUT` | Replaces a resource or updates it according to the API's design |
| Delete | `DELETE` | Deletes a resource |

### Example

```http
GET /products/10
POST /products
PUT /products/10
DELETE /products/10
```

**Additional Note:** `PATCH` is commonly used for partial updates, while `PUT` generally represents replacement of the target resource.

---

## 📦 6. JSON (JavaScript Object Notation)

**Definition:** JSON is a lightweight data-interchange format used to exchange structured data between clients and servers.

JSON represents data using key-value pairs, arrays, and other supported data types.

### Example

```json
{
  "search": "Flag"
}
```

- `search` → Key
- `"Flag"` → Value

### JSON in Web Requests

JSON is commonly sent in an HTTP request body with the following header:

```http
Content-Type: application/json
```

**Example:**

```json
{
  "username": "omar",
  "role": "student",
  "active": true
}
```

---

## 🗺️ 7. Nmap Commands

**Definition:** Nmap (Network Mapper) is a network scanning tool used to discover hosts, identify open ports, and detect services running on target systems.

### Common Commands

| Command | Description |
|---|---|
| `nmap <IP>` | Scans the default set of common ports |
| `nmap -p- <IP>` | Scans all TCP ports from 1 to 65535 |
| `nmap -p 80,443 <IP>` | Scans specific ports |
| `nmap -sV <IP>` | Attempts to identify service versions |
| `nmap -sC <IP>` | Runs the default NSE script set |
| `nmap -sV -p- <IP>` | Scans all TCP ports and attempts service-version detection |
| `nmap -Pn <IP>` | Skips host discovery and treats the target as online |

### Example

```bash
nmap -sV -p- 192.168.1.10
```

This command scans all TCP ports on the target and attempts to identify the services and their versions.

**Important Notes:**
- `nmap <IP>` does not scan every port by default; it scans the most common ports.
- `-p-` scans all TCP ports, not all UDP ports.
- Use `-sU` when you need to scan UDP ports.
- Only scan systems you own or have explicit authorization to test.

---

## 📚 Key Takeaways

- **URL:** Identifies the location of a resource.
- **DNS:** Resolves domain names to IP addresses.
- **cURL:** Sends requests and transfers data from the command line.
- **HTTPS/TLS:** Protects data exchanged between clients and servers.
- **SHA-256:** A cryptographic hashing algorithm, not an encryption algorithm.
- **HTTP Methods:** Define the intended operation on a resource.
- **JSON:** A common format for exchanging structured data.
- **Nmap:** Discovers open ports and helps identify network services.

---

*These notes are part of my personal Web Security self-study journey.*
