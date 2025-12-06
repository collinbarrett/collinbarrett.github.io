# Maintenance Guide for collinbarrett.github.io

## Current Status

### Dependencies Overview
This site uses GitHub Pages' managed Jekyll environment, which means GitHub automatically handles most maintenance. You're currently using:

- **Jekyll**: 3.10.x (GitHub Pages default)
- **jekyll-feed**: ~0.15.x (managed by GitHub)
- **jekyll-sitemap**: ~1.4.x (managed by GitHub)
- **No Gemfile**: Relying on GitHub's defaults (recommended for simplicity)

### Dependency Status: ✅ UP TO DATE
GitHub Pages dependencies are automatically managed and updated. No immediate action required.

## Maintenance Philosophy

This site follows a **"minimal maintenance"** approach:
- Let GitHub Pages handle Jekyll and plugin versions
- Focus on content quality and DNS configuration
- Only add dependencies when absolutely necessary

**Benefits:**
- Less to maintain
- Fewer security vulnerabilities
- Automatic updates from GitHub
- Simpler troubleshooting

## Regular Maintenance Tasks

### Weekly (5 minutes)
- [ ] Check website is accessible
- [ ] Review any GitHub Dependabot alerts
- [ ] Monitor email for domain expiration notices

### Monthly (15 minutes)
- [ ] Review recent commits for any issues
- [ ] Check GitHub Actions workflow status
- [ ] Test website on mobile devices
- [ ] Scan for broken links (see tools below)

### Quarterly (30 minutes)
- [ ] Review and update professional bio/content
- [ ] Update resume PDF if changed
- [ ] Check all external links are valid
- [ ] Review Google Search Console (if configured)
- [ ] Verify DNS records haven't changed
- [ ] Test site load time and performance

### Annually (1-2 hours)
- [ ] Comprehensive content review
- [ ] Update copyright year in footer
- [ ] Review and update privacy policy (if present)
- [ ] Audit social media links
- [ ] Consider Jekyll 4.x migration (optional)
- [ ] Review analytics (if configured)

## Dependency Management

### Option 1: GitHub Managed (Current - Recommended)

**Status**: ✅ You're using this approach

**Pros:**
- Zero maintenance
- Automatic security updates
- Guaranteed compatibility
- No Gemfile to manage

**Cons:**
- Limited to Jekyll 3.10.x
- Can't use latest plugin features
- Less control over versions

**Who should use this:**
- Sites that don't need bleeding-edge features
- Users who want minimal maintenance
- Small to medium personal sites

**No action needed** - Continue as-is!

### Option 2: Custom Build with GitHub Actions

**Status**: Not currently implemented

**Pros:**
- Use Jekyll 4.x
- Latest plugin versions
- More customization options
- Access to newer features

**Cons:**
- More complex setup
- Need to maintain workflow
- Potential breaking changes
- Requires more testing

**Who should use this:**
- Sites needing Jekyll 4.x features
- Advanced users comfortable with CI/CD
- Sites with custom plugins

**To implement:**
See `UPGRADE_TO_JEKYLL_4.md` (if you decide to create one)

## Recommended Tools

### Link Checking
```bash
# Install broken link checker
npm install -g broken-link-checker

# Check the site
blc https://collinmbarrett.com -ro
```

Or use online tools:
- https://validator.w3.org/checklink
- https://www.deadlinkchecker.com

### Performance Testing
- **PageSpeed Insights**: https://pagespeed.web.dev
- **GTmetrix**: https://gtmetrix.com
- **WebPageTest**: https://www.webpagetest.org

### Security Headers
- **Security Headers**: https://securityheaders.com
- **Mozilla Observatory**: https://observatory.mozilla.org

### SEO Audit
- **Google Search Console**: https://search.google.com/search-console
- **Ahrefs Webmaster Tools**: https://ahrefs.com/webmaster-tools

## Monitoring Setup

### Uptime Monitoring (Recommended)

**UptimeRobot** (Free tier includes 50 monitors)
1. Sign up at https://uptimerobot.com
2. Add monitor: `https://collinmbarrett.com`
3. Set check interval: 5 minutes
4. Add email/SMS alerts
5. Set up status page (optional)

**Other options:**
- Pingdom (paid)
- StatusCake (free tier available)
- Better Uptime (paid)

### DNS Monitoring

**DNSPerf** (Free)
- Track DNS resolution globally
- https://www.dnsperf.com

**HetrixTools** (Free tier)
- DNS change notifications
- https://hetrixtools.com

## Security Best Practices

### Current Security Measures ✅
- [x] Content Security Policy (CSP) headers configured
- [x] HTTPS enforced
- [x] No sensitive data in repository
- [x] No hardcoded credentials
- [x] Minimal JavaScript (reduced attack surface)

### Additional Recommendations

#### 1. Enable GitHub Security Features
- Go to Settings > Security > Code scanning
- Enable Dependabot alerts
- Enable secret scanning

#### 2. Review Permissions
- Audit who has write access to repository
- Use branch protection rules
- Require pull request reviews (if multiple contributors)

#### 3. Domain Security
- Lock domain transfers at registrar
- Enable two-factor authentication on domain account
- Set up domain auto-renewal

#### 4. Backup Strategy
- [x] Git history serves as backup
- [ ] Consider periodic snapshots of rendered site
- [ ] Document DNS configuration (✅ done in DNS_CONFIGURATION.md)

## Content Maintenance

### Blog Posts
- Review for outdated information
- Update code examples if syntax changed
- Fix broken links
- Update screenshots if UI changed

### Professional Information
- Keep resume PDF current
- Update job title/company if changed
- Refresh bio content
- Update skills/technologies

### External Links
Check these regularly:
- Social media profiles
- Company websites
- Technical documentation links
- Resume references

## When to Update Dependencies

### Stay on GitHub Managed (Recommended) If:
- ✅ Site works correctly
- ✅ No need for Jekyll 4.x features
- ✅ Happy with current plugin versions
- ✅ Want minimal maintenance

### Consider Custom Build If:
- ❌ Need Jekyll 4.x features
- ❌ Need newer plugin versions
- ❌ Want more control over environment
- ❌ Require custom plugins

**Current Recommendation**: Stay on GitHub managed approach. The site is working well and doesn't need more complexity.

## Troubleshooting Common Issues

### Site Not Building
1. Check Actions tab for error messages
2. Review recent commits for syntax errors
3. Verify YAML front matter in markdown files
4. Check for unsupported plugins

### Styling Issues
1. Clear browser cache
2. Check CSS version number in `_config.yml`
3. Verify SCSS files compile correctly
4. Test in incognito/private browsing

### DNS/Domain Issues
See `DNS_CONFIGURATION.md` and `WEBSITE_STATUS.md`

### Performance Issues
1. Optimize images (compress, resize)
2. Enable lazy loading for images
3. Minimize CSS/JS files
4. Use CDN for assets (if needed)

## Migration Considerations

### If You Need Jekyll 4.x

**Current** state: Jekyll 3.10.x via GitHub Pages  
**Target** state: Jekyll 4.x via GitHub Actions

**Steps required:**
1. Create `Gemfile` with Jekyll 4.x
2. Set up GitHub Actions workflow
3. Update plugins to compatible versions
4. Test locally thoroughly
5. Deploy via Actions to gh-pages branch

**Estimated effort**: 2-4 hours  
**Complexity**: Medium  
**Benefit**: Access to newer features

**Not recommended unless** you have a specific need for Jekyll 4.x features.

## Support Resources

### Documentation
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Jekyll Docs](https://jekyllrb.com/docs/)
- [Jekyll GitHub Pages Plugin](https://github.com/github/pages-gem)

### Community
- [Jekyll Forum](https://talk.jekyllrb.com)
- [GitHub Community](https://github.com/orgs/community/discussions)
- Stack Overflow: `[jekyll]` tag

### Version Information
Check current GitHub Pages versions:
https://pages.github.com/versions/

## Maintenance Schedule Recommendations

### Minimal Approach (Current)
**Time commitment**: ~1 hour/month
- Monthly link checks
- Quarterly content review
- Annual comprehensive audit
- Ad-hoc updates as needed

### Proactive Approach
**Time commitment**: ~2 hours/month
- Weekly uptime checks
- Bi-weekly link validation
- Monthly performance testing
- Quarterly security audits
- Annual full site review

**Recommendation**: Start with minimal approach. It's sufficient for a personal site.

## Checklist: Next Maintenance Tasks

### Immediate (This Week)
- [ ] Verify DNS configuration (see DNS_CONFIGURATION.md)
- [ ] Set up uptime monitoring
- [ ] Enable GitHub Dependabot alerts

### Short-term (This Month)
- [ ] Run broken link checker
- [ ] Test site on multiple devices
- [ ] Review content for accuracy
- [ ] Update professional information if needed

### Long-term (Next Quarter)
- [ ] Consider adding Google Search Console
- [ ] Set up analytics (if desired)
- [ ] Review and update privacy policy
- [ ] Audit security headers

## Conclusion

Your site is currently well-maintained and follows best practices. The main focus should be:

1. **DNS Configuration** (see DNS_CONFIGURATION.md) - highest priority
2. **Regular content updates** - keep information current
3. **Link checking** - ensure external resources work
4. **Monitoring** - catch issues early

The Jekyll 3.x dependency managed by GitHub is perfectly fine for your use case. No urgent updates needed.

---

**Document Version**: 1.0  
**Review Frequency**: Quarterly (every 3 months)
