---
title: "Site Prelaunch Check"
---

1. Check Webfonts are being pulled in. Disable all relevant fonts and check site in all browsers. Also check downloads on Webfont provider if using one.

2. Check Google Analytics code is properly configured. (Check Status under property's 'Tracking Info' tab).

3. Check Page Titles

4. Check meta - description

5. Check 404 Page is personalised

6. Check robots.txt allows all agents

7. Check humans.txt is filled out

8. Check site is using gzip - gzipped files are being created and the server is supplying those files. Check headers or use http://www.whatsmyip.org/http-compression-test/ (works on site or individual fles)

9. Run the site through [Gtmetrics](http://gtmetrix.com/dashboard.html) / YSlow. Modify until it gets an A.

10. Run the site through HTML Validator: http://validator.w3.org/

11. Check how deprecated browsers are handled. Chromeframe / Warning etc.

12. Check related Social Media use current image/icon and Name

13. Check HTML pages are **not** set to cache (check response headers).

14. Check assets **are** set to cache (check response headers).

[Also a few other bits here: http://www.codebeerstartups.com/2012/12/some-basic-guidelines-i-always-follow-before-launching-an-ruby-on-rails-application/]