# DNS Configuration Guide for collinmbarrett.com

## Quick Reference

This document provides the DNS configuration needed for collinmbarrett.com to work with GitHub Pages.

## Required DNS Records

### A Records (Apex Domain)
Point the apex domain `collinmbarrett.com` to GitHub Pages servers:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | @ | 185.199.108.153 | 3600 |
| A | @ | 185.199.109.153 | 3600 |
| A | @ | 185.199.110.153 | 3600 |
| A | @ | 185.199.111.153 | 3600 |

> **Note**: The `@` symbol typically represents the apex/root domain at most DNS providers.

### CNAME Record (www subdomain)
Point the www subdomain to your GitHub Pages site:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| CNAME | www | collinbarrett.github.io | 3600 |

## Configuration by DNS Provider

### Cloudflare
1. Log into Cloudflare dashboard
2. Select your domain
3. Go to "DNS" section
4. Add the 4 A records above
5. Add the CNAME record above
6. **Important**: Set proxy status to "DNS only" (gray cloud) initially

### Namecheap
1. Log into Namecheap account
2. Go to Domain List > Manage
3. Click "Advanced DNS"
4. Add the A records with Host `@`
5. Add CNAME record with Host `www`

### GoDaddy
1. Log into GoDaddy account
2. Go to My Products > DNS
3. Click "Add" for each record
4. Type: Select A or CNAME
5. Name: Enter `@` for apex or `www` for subdomain
6. Value: Enter corresponding IP or domain

### Google Domains / Squarespace Domains
1. Log into account
2. Select your domain
3. Click "DNS" or "DNS Settings"
4. Click "Manage custom records"
5. Add records as specified above

## Verification

After configuring DNS records, verify they're working:

### Command Line
```bash
# Check A records (apex domain)
dig collinmbarrett.com

# Expected output should show the 4 GitHub IPs

# Check CNAME record (www subdomain)
dig www.collinmbarrett.com

# Expected output should show: collinbarrett.github.io
```

### Online Tools
- **DNS Checker**: https://dnschecker.org
  - Enter: `collinmbarrett.com`
  - Checks propagation globally
  
- **MX Toolbox**: https://mxtoolbox.com/DNSLookup.aspx
  - Comprehensive DNS analysis

- **WhatsMyDNS**: https://whatsmydns.net
  - Checks propagation worldwide

## Troubleshooting

### DNS Not Resolving
**Problem**: `dig` or `nslookup` returns NXDOMAIN or REFUSED

**Solutions**:
1. Verify records are saved at DNS provider
2. Wait 15-60 minutes for changes to take effect
3. Clear your local DNS cache (see commands below)
4. Try from a different network or device

### Wrong IP Addresses
**Problem**: A records point to incorrect IPs

**Solution**: Update to the current GitHub Pages IPs listed above. GitHub occasionally updates these IPs.

### www Not Working
**Problem**: www.collinmbarrett.com doesn't work but apex does (or vice versa)

**Solution**: Ensure CNAME record is configured correctly. Both apex and www need separate records.

### SSL/HTTPS Issues
**Problem**: Site shows SSL error or "Not Secure"

**Solutions**:
1. Wait 24 hours after DNS configuration
2. In GitHub Settings > Pages, remove and re-add custom domain
3. Wait for GitHub to issue SSL certificate (usually 15 minutes)
4. Enable "Enforce HTTPS" in GitHub Pages settings

## DNS Propagation Timeline

- **Local DNS Server**: 5-15 minutes
- **ISP DNS Servers**: 1-4 hours  
- **Global Propagation**: 24-48 hours

During propagation, different users may see different versions of the site or get DNS errors. This is normal.

## Clear Local DNS Cache

### Windows
```cmd
ipconfig /flushdns
```

### macOS
```bash
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

### Linux
```bash
# systemd-resolved
sudo systemd-resolve --flush-caches

# nscd
sudo /etc/init.d/nscd restart

# dnsmasq
sudo /etc/init.d/dnsmasq restart
```

### Browsers
Chrome/Edge:
```
chrome://net-internals/#dns
```
Click "Clear host cache"

Firefox:
```
about:networking#dns
```
Click "Clear DNS Cache"

## Maintaining DNS Configuration

### Regular Checks (Quarterly)
- Verify DNS records haven't changed unexpectedly
- Check for GitHub Pages IP updates
- Test site accessibility from multiple locations

### When to Update
- GitHub announces new IP addresses for Pages
- Migrating to a different DNS provider
- Enabling additional services (email, etc.)

### Documentation
- Keep this file updated with any changes
- Document your DNS provider in comments
- Note any provider-specific configurations

## Additional Records (Optional)

### CAA Record (Certificate Authority Authorization)
Specify which CAs can issue certificates:

| Type | Name | Value |
|------|------|-------|
| CAA | @ | 0 issue "letsencrypt.org" |
| CAA | @ | 0 issue "pki.goog" |

> GitHub uses Let's Encrypt, but Google's certificates may also be used.

### TXT Record (Domain Verification)
May be needed for external services:

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| TXT | @ | "verification-string" | Google, etc. |

## Support

### GitHub Pages DNS Documentation
https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site

### GitHub Status
Check if GitHub Pages is experiencing issues:
https://www.githubstatus.com

### Contact
For site-specific issues:
https://github.com/collinbarrett/collinbarrett.github.io/issues

---

**Last Updated**: December 6, 2025  
**Next Review**: March 6, 2026
