## Install redpanda console

Latest redpanda-console is available at https://github.com/redpanda-data/console/releases.  Pick the latest stable release.
At the time of this writing 2.7.2 is the latest release of redpanda-console.  The commands assume RHEL vm instance

`wget https://github.com/redpanda-data/console/releases/download/v2.7.2/redpanda-console-2.7.2-1.x86_64.rpm`

`sudo rpm -i redpanda-console-2.7.2-1.x86_64.rpm`

## Configuration

Redpanda console configuration is at `/etc/redpanda/redpanda-console-config.yaml`

## https setup for redpanda-console

You would need a certificate in pem format and a key for this.  Redpanda-console config file would need to be updated like this 

```
   server:
  listenPort: 8080
  advertisedHttpsListenPort: 443
  httpsListenPort: 443
  tls:
    enabled: true
    certFilepath: /etc/redpanda/certs/certificate.pem
    keyFilepath: /etc/redpanda/certs/keyfile.key
```

Most often, it is likely you have a .pfx file for https. `.pfx` has both certificate and key embedded in it and needs to be seperated out

  Export the private key:
  `openssl pkcs12 -in certificate.pfx -nocerts -out private.key`

  Export the certificate:
  `openssl pkcs12 -in certificate.pfx -clcerts -nokeys -out certificate.pem`

  Remove the passphrase from the private key:
  `openssl rsa -in private.key -out decrypted.key`

Once you execute these commands you can use `certificate.pem` and `decrypted.key` for `certFilepath` and `keyFilepath` respectively

### Permissions on the cert and key file

Ensure ownership of certificate and keyfile is given to `redpanda:redpanda`
 `chown redpanda:redpanda <filename>`




