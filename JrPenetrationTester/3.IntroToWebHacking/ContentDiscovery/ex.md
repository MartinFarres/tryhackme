# Manual Discovery

## Favicon

Check for favicon database: https://wiki.owasp.org/index.php/OWASP_favicon_database

Get chekcsum of favicon:

```bash
user@machine$ curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
```

## Sitemap.xml

Unlike the robots.txt file, which restricts what search engine crawlers can look at, the sitemap.xml file gives a list of every file the website owner wishes to be listed on a search engine. These can sometimes contain areas of the website that are a bit more difficult to navigate to or even list some old webpages that the current site no longer uses but are still working behind the scenes.

## HTTP Headers

When we make requests to the web server, the server returns various HTTP headers. These headers can sometimes contain useful information such as the webserver software and possibly the programming/scripting language in use. In the below example, we can see the webserver is NGINX version 1.18.0 and runs PHP version 7.4.3. Using this information, we could find vulnerable versions of software being used.

```BASH
curl https://link.com -v
```

# Osint

## Google Hacking / Dorking

| Filter            | Example                       | Description                                                           |
| ----------------- | ----------------------------- | --------------------------------------------------------------------- |
| **site**          | `site:tryhackme.com`          | Returns results only from the specified website address.              |
| **inurl**         | `inurl:admin`                 | Returns results that have the specified word in the URL.              |
| **filetype**      | `filetype:pdf`                | Returns results which are a particular file extension.                |
| **intitle**       | `intitle:admin`               | Returns results that contain the specified word in the title.         |
| **allintitle**    | `allintitle:login panel`      | Returns results that contain _all_ specified words in the title.      |
| **allinurl**      | `allinurl:admin login`        | Returns results that contain _all_ specified words in the URL.        |
| **cache**         | `cache:example.com`           | Shows Google’s cached version of a page.                              |
| **related**       | `related:example.com`         | Finds sites related to the specified URL.                             |
| **intext**        | `intext:"username"`           | Finds pages containing the specified word or phrase in the body text. |
| **allintext**     | `allintext:password username` | Finds pages containing _all_ words in the body text.                  |
| **ext**           | `ext:log`                     | Alias of `filetype:`; often used to find log or backup files.         |
| **"exact match"** | `"confidential report"`       | Forces search to match exact phrase.                                  |
| **OR**            | `login OR signin`             | Searches for either one keyword or another.                           |
| **- (minus)**     | `admin -login`                | Excludes results containing the specified word.                       |
| **\* (wildcard)** | `filetype:log inurl:*`        | Matches anything in place of the asterisk; good for broad searches.   |

| Google Dork                       | Ejemplo de Uso                             | ¿Qué podrías encontrar?                                                  |
| --------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------ |
| `site:example.com`                | `site:github.com`                          | Solo resultados dentro de GitHub.                                        |
| `inurl:admin`                     | `inurl:admin login`                        | Paneles de administración expuestos.                                     |
| `filetype:pdf`                    | `filetype:pdf site:gov.ar`                 | Documentos PDF públicos de sitios gubernamentales.                       |
| `intitle:index of`                | `intitle:"index of" passwords`             | Directorios listados públicamente con archivos potencialmente sensibles. |
| `intext:"password"`               | `intext:"password" filetype:log`           | Archivos `.log` con posibles credenciales.                               |
| `ext:sql`                         | `ext:sql site:edu`                         | Archivos de bases de datos expuestos en sitios educativos.               |
| `"confidential"`                  | `"confidential" site:drive.google.com`     | Documentos marcados como confidenciales públicos por error.              |
| `cache:example.com`               | `cache:facebook.com`                       | Ver versión en caché de una página web.                                  |
| `allinurl:auth login`             | `allinurl:auth login site:org`             | URLs que contienen autenticación y login, en sitios `.org`.              |
| `allintext:username password`     | `allintext:username password filetype:txt` | Archivos `.txt` con credenciales visibles.                               |
| `related:nytimes.com`             | `related:nytimes.com`                      | Sitios similares a The New York Times.                                   |
| `site:pastebin.com intext:apikey` | `site:pastebin.com intext:apikey`          | Claves API publicadas en Pastebin.                                       |
| `site:s3.amazonaws.com`           | `site:s3.amazonaws.com confidential`       | Buckets de Amazon S3 públicos con archivos confidenciales.               |
| `intitle:login`                   | `intitle:login site:edu`                   | Páginas de login en sitios educativos.                                   |

## Wappalyzer

https://www.wappalyzer.com/

## Wayback Machine

https://archive.org/web/

## Github

## S3 Buckets

S3 Buckets are a storage service provided by Amazon AWS, allowing people to save files and even static website content in the cloud accessible over HTTP and HTTPS. The owner of the files can set access permissions to either make files public, private and even writable. Sometimes these access permissions are incorrectly set and inadvertently allow access to files that shouldn't be available to the public. The format of the S3 buckets is http(s)://{name}.s3.amazonaws.com where {name} is decided by the owner, such as tryhackme-assets.s3.amazonaws.com. S3 buckets can be discovered in many ways, such as finding the URLs in the website's page source, GitHub repositories, or even automating the process. One common automation method is by using the company name followed by common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.

# Automated Discovery

## What is Automated Discovery?

Automated discovery is the process of using tools to discover content rather than doing it manually. This process is automated as it usually contains hundreds, thousands or even millions of requests to a web server. These requests check whether a file or directory exists on a website, giving us access to resources we didn't previously know existed. This process is made possible by using a resource called wordlists.

## What are wordlists?

Wordlists are just text files that contain a long list of commonly used words; they can cover many different use cases. For example, a password wordlist would include the most frequently used passwords, whereas we're looking for content in our case, so we'd require a list containing the most commonly used directory and file names. An excellent resource for wordlists that is preinstalled on the THM AttackBox is https://github.com/danielmiessler/SecLists which Daniel Miessler curates.

## Automation Tools

Although there are many different content discovery tools available, all with their features and flaws, we're going to cover three which are preinstalled on our attack box, ffuf, dirb and gobuster.

On the AttackBox execute the following three commands, targeting the Acme IT Support website and see what results you get.

### Using ffuf:

```bash
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.209.62/FUZZ
```

### Using dirb:

```bash
user@machine$ dirb http://10.10.209.62/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

### Using Gobuster:

```bash
user@machine$ gobuster dir --url http://10.10.209.62/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```
