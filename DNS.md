Resolves domain names to I.P addresses
Like yellow pages (phone book)

If locally, the OS cannot find the IP address in its own cache memory,
it will send a request to a `Resolver Server` (ISP)
  - it will then check its own cache
  - if it still cannot find, it will send to `Root Server`
    - The `Root Server` will _redirect_ the ISP to the `Top Level Domain` (TLD) server
      (for .e.g. to the dot com TLD server, .com, .net, .org ...)
      - The TLD server will then _redirect_ the request to the Authoritative Name Server,
        which will tell the ISP server what is the IP address

### Root Server
- The top or the root of DNS hierachy
- 13 sets of root servers placed around the word
- operated by 12 different organizations
- each set has their own unique IP address

### Top Level Domain Server
- Stores the address information for top level domains
- Such as .com, .net, .org, etc

### Authoritative Name Server
- Responsible for knowing everything about the domain
- Including the IP address

## DNS records
- In the Authoritative Name Server database, there's a zone file with all the DNS records

#### A record
- resolves domain names to IPv4 addresses

#### AAAA record
- (pronounced quad A record)
- resolves domain names to IPv6 (128bit alphanumeric) addresses (replacing old IPv4 addresses)
[AAAA record](./AAAA-record.png)

#### CNAME record
[Canonical Name Record](./canonical-name.png)
** Computers read domain names from RIGHT to LEVEL
  - there's a hidden DOT on the right most, that's the root domain
  - so the right -> left will be root domain, top level domain, second level domain, and a subdomain
  [CNAME resolves subdomain to domain](./CNAME-resolves.png)
    - its up to the Web Server handle the url and direct user to its FTP (sub) service

#### MX record
- MX - mail changer
- used for email
[MX](./mx.png)
- if an email is sent to `tom@example.com`, the DNS need to forward it to the correct host (server)
- MX record will usually have 2 entries, primary and secondary

#### SOA record
- Start of Authority
- stores administrative information about a `DNS zone`
- `DNS zone`s are created for manageability purpose, each will have their own DNS zone files, which contain an SOA record
[SOA](./SOA.png)
- the MNAME is the primary name server
- the RNAME is the email address of the adminitrator for this zone. The dot represents the @ symbol

#### NS record
[NS](./NS.png)
- generally list 2 servers, primary and secondary

#### SRV record
[SRV](./SRV.png)
- for services that require a PORT number
- like Printer, VOIP etc

#### PTR record
[PTR](./PTR.png)
- PTR records are attached to email and used to prevent spam
- Email server will use PTR to make sure sender is authentic by matching domain name to its authentic IP address
  (AKA reverse DNS lookup)

### TXT record
[TXT](./TXT.png)
- contains general / contact information about a domain
- also used to prevent email spam