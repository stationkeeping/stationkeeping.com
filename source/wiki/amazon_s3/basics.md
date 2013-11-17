---
title: "Basics"
---

There is a nice (free) Mac client called [3Hub](www.3hubapp.com). It supports a number of things that can't be done from the web panel (for example setting headers on bulk).

Don't use periods or capital letters in bucket names. Stick to `a-z`, `0-9` and `-`. Any other characters risk causing problems so don't use them, especially periods which cause problems with SSL.

To support Cross-site XHR requests, you need to add a CORS file which can be done by clicking once on the bucket, clicking *Properties* on the right, then opening the Accordion to *Permissions* and choosing *Add CORS Configuration*. Here is an example of a CORS Configuration that allows any domain to read from the bucket:

```
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```