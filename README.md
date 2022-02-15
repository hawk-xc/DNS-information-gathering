# DNS-information-gathering
mendapatkan informasi victim melalui dns (53) dengan tools dig

### action

*Getting information via DNS*

You can easily identify the interesting targets of a company by searching for corresponding names in the network.
This can be done with the DNS protocol. The protocol is used both on the Internet to resolve website names and, by default, in Microsoft Windows Active Directory environments for name resolution. 
You can use the ```dig``` program to query information about a domain. DNS has several records, called resource records:

Type | Name | Description

`A` | Address record | Contains the IPv4 address of the host

`AAAA` | IPv6 address record | Contains the IPv6 address of the host

`CNAME` | Canoncial name record | Canonical name of the host

`MX` | Mail exchanger record | Responsible mail server of the domain

`NS` | Name server record | Hostname of the authoritative name server

`PTR` | Pointer record | Reverse resolution from IP address to canonical names

`SOA` | Start of authority | Information about the DNS zone (e.g. update period)

`SRV` | Service locator | general entry for offered services

`TXT` | Text record | freely selectable entry, mostly used by SPI DMARD or DNS-SD

For example, if you want to resolve the A record of website.com, use the command ```dig A website.com``` The output of the MX-record allows the identification of the mail server.
It is more difficult to identify other sub-domains. Instead of trying every possible sub-domain manually, this process can be automated by different tools. One of these tools is `dnsrecon`. As input parameters it needs the domain to be examined (-d) and a list of words (-D) whose triggering is to be tried as subdomain. With the type (-t) you specify which test should be performed. "Std" stands for the determination song of "standard records", like A or MX, while "brt" specifies the brute forcing of sub-domains.

```bash
dnsrecon -d website.com -D /usr/share/wordlists/dnsmap.txt -t brt
```

The quality of the results also depends on the quality of the word list used. See also the options (-w). Here dnsrecon performs a reverse resolution of the IP addresses of the identified network areas to DNS names. This also allows to identify additional servers.
A another example of dnsrecon would be this:

```bash
dnsrecon -d website.com -D /usr/share/wordlists/dnsmap.txt -w -f -a -s -b -y -k -w -z --threads 20 -c out.csv -t brt
```

This example of dnsrecon will use the wordlist (-D) to bruteforce subdomains(-t brt), filter records that match to wildcard (-f), perform axfr enumeration (-a), perform a reverse lookup ipv4 range (-s), perform bing enumeration (-b), perform yandex enumeration (-y), perform ctr.sh enumeration(-k), perform deep whois enumeration(-w) and perform DNSSEC enumeration(-z) with 20 threads(--threads) and save output(-c out.csv)
With the DNS zone transfer it is possible to replicate DNS databases between DNS servers. In this way, all sub-domains can be read out with just one command. 

Normally this option should only be possible for selected servers. However, you can test whether the function is still available for others by using the "dig" command, for example:

dig axfr website.com
