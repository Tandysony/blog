---
layout: post
title: How to set up SSL for a domain on an AWS AMI instance
tags: [SSL, AWS, AMI]
---

> NOTE: This process is for AWS **AMI** instance setup using a paid SSL certificate. If you are using a **Linux** instance, use [Let's Encrypt](https://letsencrypt.org/) instead, as it is free, automated and open Certificate Authority. You might use `Let's Encrypt` for an AWS **AMI** instance, but it is not as easy as a **Linux** one.

1.  Purchase your ssl from an ssl provider. For example: [https://www.ssls.com/](https://www.ssls.com/)

2.  Prepare your CSR (Certificate Signing Request)

3.  Download and configure OpenSSL by adding the directory to environment variables from [here](https://slproweb.com/products/Win32OpenSSL.html). Download the `Win32 OpenSSL v1.x.xx Light` version.

4.  Open command line as administrator, and change your directory to `C:\OpenSSL-Win32\bin` on a windows machine to generate CSR file.

5.  Create a CSR (certificate signing request) and a private key files for your domain. `www.yourdomain.com` is your server domain use the following command.


    ```powershell
    openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.com.key -out yourdomain.com.csr
    ```

6.  Fill out the form as following, and your `yourdomain.com.key` and `yourdomain.com.csr` files will be generated under this directory.


    ```
    Country:                    CA
    State:                      Quebec
    Locality:                   Montreal
    Organization:               Your company name
    Division:                   Your division/department
    Common name:                www.yourdomain.com
    Email:                      admin@yourdomain.com
    A challenge password:       YourPsw
    A optional company name:    CompanyName
    ```

7.  Open the `.csr` file, copy the whole content and paste it to required `CSR` field to to sign/active the certificate file. You will see the certificate is now **In progress**.

8.  Save the `aserialnumber.txt` file and put it on the server project root folder. This file must be accessable from your root domain: [www.yourdomain.com/.well-known/pki-validation/aserialnumber.txt](www.yourdomain.com/.well-known/pki-validation/aserialnumber.txt)


    > **NOTE:** Starting July 20th of 2017, the file path which is checked by Comodo during the HTTP-based validation has changed. Comodo no longer checks the file accessibility via the path `http://(sub).yourdomain.com/file.txt`, and uses the link `http://(sub).yourdomain.com/.well-known/pki-validation/file.txt`

9.  Once done, go to your purchase history, you should be able to see the status is now "Activated" with an expire date. Now go to the detailed page of this certificate to download the certificate archive. There are three files included: `yourdomain.com.ca-bundle`, `yourdomain.com.crt` and `yourdomain.com.p7b`.

10. Upload the private key file, `yourdomain.com.key` generated in step 4, and the certificate files to a folder of your website project directory, say, `xxx/yourproject/ssl`.

11. Configure the virtualhost settings to enable SSL on your server, Apache 2.4 in this case.


    ```bash
    LoadModule ssl_module modules/mod_ssl.so

    Listen 443
    <VirtualHost ipaddress:443>
        SSLEngine on
        SSLCertificateFile /xxx/yourproject/ssl/yourdomain.com.crt
        SSLCertificateKeyFile /xxx/yourproject/ssl/yourdomain.com.key
        SSLCertificateChainFile /xxx/yourproject/ssl/yourdomain.com.ca-bundle
    </VirtualHost>
    ```

12. Go to [https://www.sslchecker.com/sslchecker](https://www.sslchecker.com/sslchecker) to make sure the SSL is working properly.

## Reference

- Install Openssl on a windows machine - [https://www.tbs-certificates.co.uk/FAQ/en/openssl-windows.html](https://www.tbs-certificates.co.uk/FAQ/en/openssl-windows.html)

- Creating a server certificate - [http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs_create.html#d0e16902](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs_create.html#d0e16902)

- Apache 2.4 SSL settings - [https://httpd.apache.org/docs/2.4/ssl/](https://httpd.apache.org/docs/2.4/ssl/)

- OpenSSL Error - 'Unable to load config info from `<path>` - [https://supportforums.cisco.com/document/116521/openssl-error-unable-load-config-info-error-req](https://supportforums.cisco.com/document/116521/openssl-error-unable-load-config-info-error-req)

- Install SSL on amazon EC22 - [https://www.youtube.com/watch?v=\_a4wRsT6LaI](https://www.youtube.com/watch?v=_a4wRsT6LaI)
