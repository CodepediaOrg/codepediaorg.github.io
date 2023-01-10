---
layout: post
title: Renew Let's Encrypt ssl certificates with certbot on nginx server in terminal
description: "Renew Let's Encrypt ssl certificates with certbot on nginx server in terminal code snippet"
author: ama
permalink: /snippets/61a8ba79fb0221040e492e1e/renew-let-s-encrypt-ssl-certificates-with-certbot-on-nginx-server-in-terminal
published: true
snippetId: 61a8ba79fb0221040e492e1e
categories: [snippets]
tags: [ssl, certbot, nginx, letsencrypt, ubuntu, linux, codever-snippets]
---

First list available certificates with the following command `sudo certbot certificates`.
 Should look something like the following:

```shell
$ sudo certbot certificates
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Found the following certs:
  Certificate Name: codever.dev
    Domains: codever.dev www.codever.dev
    Expiry Date: 2022-03-02 11:13:46+00:00 (VALID: 89 days)
    Certificate Path: /etc/letsencrypt/live/codever.dev/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/codever.dev/privkey.pem
  Certificate Name: codever.land
    Domains: codever.land www.codever.dev
    Expiry Date: 2021-12-21 13:06:54+00:00 (VALID: 19 days)
    Certificate Path: /etc/letsencrypt/live/codever.land/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/codever.land/privkey.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Select the **Certificate Name** from the list and do a **dry run** before executing the actual command,
 with the help of `--dry-run` flag - e.g. `sudo certbot renew --cert-name codever.land --dry-run`.
  The result should look something like the following:

```shell
sudo certbot renew --cert-name codever.land --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/codever.land.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert is due for renewal, auto-renewing...
Plugins selected: Authenticator nginx, Installer nginx
Starting new HTTPS connection (1): acme-staging-v02.api.letsencrypt.org
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for codever.land
http-01 challenge for www.codever.dev
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed with reload of nginx server; fullchain is
/etc/letsencrypt/live/codever.land/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/codever.land/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Execute the actual renewal by removing the `--dry-run` flag

```shell
$ sudo certbot renew --cert-name codever.land

Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/codever.land.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert is due for renewal, auto-renewing...
Plugins selected: Authenticator nginx, Installer nginx
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for codever.land
http-01 challenge for www.codever.dev
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed with reload of nginx server; fullchain is
/etc/letsencrypt/live/codever.land/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/codever.land/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Use the `sudo cerbot certificates` command to verify the validity and check the **new** expiration date:


```shell
$ sudo certbot certificates
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Found the following certs:
  Certificate Name: codever.dev
    Domains: codever.dev www.codever.dev
    Expiry Date: 2022-03-02 11:13:46+00:00 (VALID: 89 days)
    Certificate Path: /etc/letsencrypt/live/codever.dev/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/codever.dev/privkey.pem
  Certificate Name: codever.land
    Domains: codever.land www.codever.dev
    Expiry Date: 2022-03-02 11:18:39+00:00 (VALID: 89 days)
    Certificate Path: /etc/letsencrypt/live/codever.land/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/codever.land/privkey.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```



<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="61a8ba79fb0221040e492e1e" %}
