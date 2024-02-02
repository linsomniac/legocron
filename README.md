LetsEncrypt automation wrapper for LeGo

# Overview

This script automates the request and renew of LetsEncrypt certs using
[LeGo](https://github.com/go-acme/lego).  It makes it as easy as to
create a cron job that runs: 

    legocron www.example.com altname.example.com foo.example.com

to request and refresh the certs.  If you add or remove names, legocron
will detect it and issue a new cert with those names.

# Getting Started

- Install lego.  That may be via your system package manager ("apt install lego")
or by downloading a binary from [the lego Releases page](https://github.com/go-acme/lego/releases).

- Download legocron:

```shell
wget https://raw.githubusercontent.com/linsomniac/legocron/1.0/legocron
chmod 755 legocron
mv legocron /usr/local/sbin
```

- Edit "legocron" and set "EMAIL_ADDR" and select a "LEGO_ARGS" provider option.

- Run "legocron" with any certificate names you want to request (use staging for testing):

```shell
legocron --staging www.example.com
# or:
legocron --staging www.example.com altname.example.com foo.example.com
```

- Check the certs in "/usr/local/lib/legocron/certificates"

- Remove the "--staging" when you are done testing and want a real cert.

- Set up legocron in cron:

    0 0 * * * root /usr/local/sbin/legocron www.example.com

- Optionally: Write a post-cert script in "/usr/local/lib/legocron/post_cert_hook" and
  make it executable, with any steps to take after a certificate has been issued/renewed.
  For exmaple, you may want to "systemctl restart apache2" in there.

## License

Creative Commons Zero v1.0 Universal

[//]: # ( vim: set tw=90 ts=4 sw=4 ai: )
