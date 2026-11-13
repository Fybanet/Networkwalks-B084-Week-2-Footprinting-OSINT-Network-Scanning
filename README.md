# Networkwalks B084 | Week 2 | Footprinting, OSINT & Network Scanning

Cybersecurity internship at Networkwalks. This repo holds my Week 2 practical work: gathering public information about a target domain (footprinting and OSINT) and scanning my own lab network.

**Full write-up with screenshot evidence:** [report/Week2_Penetration_Testing_Report_B084.pdf](https://github.com/Fybanet/Networkwalks-B084-Week-2-Footprinting-OSINT-Network-Scanning/blob/main/Week2_Penetration_Testing_Report_B084.pdf)

> **Disclaimer:** Everything here is for education only. I only tested the domain set as the practical target, used passive public sources for OSINT, and scanned a network I own. Don't run these tools against systems you don't own or have written permission to test.

## Environment

- Kali Linux in VirtualBox
- Target for PM1 and PM3: `networkwalks.com`
- OSINT target for PM4: `microsoft.com` (passive search only)
- Lab network: `10.0.0.0/24` (gateway `10.0.0.1`, Kali VM `10.0.0.2`)

## Modules

| Module | Topic | Tools |
|---|---|---|
| W2-PM1 | Footprinting with multiple Kali tools | whois, whatweb, nslookup, curl, wafw00f, dnsrecon |
| W2-PM2 | Google Hacking Database | Google dorks |
| W2-PM3 | Email discovery with Maltego | Maltego 4.13.0 |
| W2-PM4 | Email and subdomain collection | theHarvester 4.10.1 |
| W2-PM5 | Local network discovery | Zenmap (Nmap) |

## W2-PM1: Footprinting networkwalks.com

```bash
whois networkwalks.com
whatweb networkwalks.com
nslookup networkwalks.com
curl -I https://networkwalks.com
wafw00f networkwalks.com
dnsrecon -d networkwalks.com
```

| Tool | What I found |
|---|---|
| whois | Registrar GoDaddy.com, LLC. Created 2019-11-06, expires 2027-11-06. Four client locks set. Owner hidden by a privacy service. DNSSEC unsigned. |
| whatweb | Apache, WordPress 7.1.4, WP Download Manager 3.3.76, jQuery 3.7.1, Google Tag Manager. 301 redirect to HTTPS. |
| nslookup | Resolved to `192.232.216.135` using 8.8.8.8 |
| curl -I | HTTP/2 200, `x-powered-by: WordPress`, nginx cache header, `/wp-json/` link |
| wafw00f | ModSecurity (SpiderLabs) WAF detected |
| dnsrecon | 8 records: A, MX, NS (HostGator), TXT/SPF, SRV |

## W2-PM2: Google Hacking Database

I used search operators such as `intitle:"webcamxp 5"` and `filetype:pdf` to find indexed camera interfaces and maths PDFs, and recorded ten results of each in the worksheet. Camera addresses are left out of this repo on purpose. I did not try to log in to or view any device.

## W2-PM3: Maltego

Added `networkwalks.com` as a Domain entity and ran **To Email Addresses [Search Engine]**. It found `info@networkwalks.com`, the same address WhatWeb reported.

## W2-PM4: theHarvester

```bash
theHarvester -d microsoft.com -l 1000 -b baidu
```

Two email addresses and seven hostnames from the Baidu source.

## W2-PM5: Zenmap

Scanned my lab network and exported the topology as a PDF. The export lists `10.0.0.1`, `10.0.0.2`, `10.0.0.3` and `localhost`.

## Key takeaways

- The same IP address appeared in nslookup, WhatWeb and dnsrecon, and the same email in WhatWeb and Maltego, so the tools backed each other up.
- Exposed versions, headers and DNS records are observations, not proof of a vulnerability.
- Nothing was exploited or validated. This was information gathering only.

## Repo structure

```
.
├── README.md
└── report/    Full Week 2 report with screenshots (.pdf)
```

## Author

**Iwunze Ogochukwu** | Cybersecurity, Networkwalks B084 | GitHub: [Fybanet](https://github.com/Fybanet)
