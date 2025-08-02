# Subdomain Enumeration

## OSINT - SSL/TLS Certificates

When an SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority), CA's take part in what's called "Certificate Transparency (CT) logs". These are publicly accessible logs of every SSL/TLS certificate created for a domain name. The purpose of Certificate Transparency logs is to stop malicious and accidentally made certificates from being used. We can use this service to our advantage to discover subdomains belonging to a domain, sites like https://crt.sh offer a searchable database of certificates that shows current and historical results.

## OSINT - Search engines

| Search Engine | Filter/Operator          | Example                      | Description                                                     |
| ------------- | ------------------------ | ---------------------------- | --------------------------------------------------------------- |
| **Google**    | `site:`                  | `site:linkedin.com`          | Limits results to a specific domain.                            |
| **Google**    | `intitle:`               | `intitle:"login"`            | Searches for pages with the term in the title.                  |
| **Google**    | `inurl:`                 | `inurl:admin`                | Searches for pages with the term in the URL.                    |
| **Google**    | `filetype:`              | `filetype:xls`               | Finds specific file types (e.g. `.xls`, `.pdf`).                |
| **Google**    | `intext:`                | `intext:"SSN"`               | Searches for text inside the body of a page.                    |
| **Google**    | `cache:`                 | `cache:example.com`          | Shows cached version of a page by Google.                       |
| **Shodan**    | `org:`                   | `org:"Amazon"`               | Finds devices or services belonging to a specific organization. |
| **Shodan**    | `port:`                  | `port:22`                    | Filters by open port.                                           |
| **Shodan**    | `country:`               | `country:"US"`               | Limits results to a specific country.                           |
| **Shodan**    | `hostname:`              | `hostname:gov`               | Filters by hostname or domain pattern.                          |
| **Censys**    | `services.service_name:` | `services.service_name:HTTP` | Filters by service type (HTTP, FTP, etc).                       |
| **Censys**    | `location.country:`      | `location.country:"Germany"` | Filters hosts by geographic location.                           |
| **Bing**      | `ip:`                    | `ip:192.168.1.1`             | Returns results from a specific IP address.                     |
| **Bing**      | `contains:`              | `contains:pdf`               | Finds pages containing links to specific file types.            |
| **Bing**      | `language:`              | `language:es`                | Filters search results by language.                             |
| **Yandex**    | `host:`                  | `host:example.com`           | Restricts results to a specific domain (similar to `site:`).    |
| **ZoomEye**   | `app:`                   | `app:"Apache"`               | Filters results by application/service name.                    |
| **ZoomEye**   | `os:`                    | `os:"Windows"`               | Filters results based on operating system.                      |

## DNS Bruteforce

```bash
dnsrecon -t brt -d acmeitsupport.thm
```

## OSINT - Sublist3r

```bash
./sublist3r.py -d acmeitsupport.thm
```

To speed up the process of OSINT subdomain discovery, we can automate the above methods with the help of tools like Sublist3r,

## Virtual Hosts

Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\windows\system32\drivers\etc\hosts file for Windows users), which maps domain names to IP addresses.

Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the Host header. We can utilize this host header by making changes to it and monitoring the response to see if we've discovered a new website.

Like with DNS Bruteforce, we can automate this process by using a wordlist of commonly used subdomains.

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP
```

The above command uses the -w switch to specify the wordlist we are going to use. The -H switch adds/edits a header (in this instance, the Host header), we have the FUZZ keyword in the space where a subdomain would normally go, and this is where we will try all the options from the wordlist.

Because the above command will always produce a valid result, we need to filter the output. We can do this by using the page size result with the -fs switch. Edit the below command replacing {size} with the most occurring size value from the previous result and try it on the AttackBox.

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs {size}
```
