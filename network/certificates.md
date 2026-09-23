# Certificates

## LAN Certificate Using mkcert

The certificate must contain every hostname or IP address users will enter, and every LAN client must trust the same mkcert root CA.

Prefer a stable LAN DNS name over accessing the machine by IP. For example:

concierge-dev.example.internal → 192.168.1.50

Then generate the server certificate:

```bash
mkdir -p tls

mkcert \
-cert-file tls/concierge.pem \
-key-file tls/concierge-key.pem \
concierge-dev.example.internal \
192.168.1.50
```

This permits both:

https://concierge-dev.example.internal
https://192.168.1.50

Only include names and addresses you actually control. If the address changes, regenerate the certificate.

### Trusting it throughout the LAN

Find the CA:

```mkcert -CAROOT```

That directory contains:

rootCA.pem
rootCA-key.pem

Distribute only rootCA.pem to LAN clients. Never distribute rootCA-key.pem; possession of it permits forging trusted certificates. The mkcert documentation emphasizes this distinction. mkcert documentation
(https://github.com/FiloSottile/mkcert/blob/master/README.md)

Install rootCA.pem as a trusted root CA on every client:

macOS:

```bash
sudo security add-trusted-cert \
-d -r trustRoot \
-k /Library/Keychains/System.keychain \
rootCA.pem
```

Windows, from an Administrator terminal:

certutil -addstore -f Root rootCA.pem

Debian/Ubuntu:

sudo cp rootCA.pem /usr/local/share/ca-certificates/concierge-dev-ca.crt
sudo update-ca-certificates

For company-managed machines, distribute it through Group Policy, Intune, MDM, or your normal endpoint-management system. Browsers may need restarting afterward. Firefox can require separate NSS trust-store handling on some
systems.

### Configure the server

Mount concierge.pem and concierge-key.pem into the HTTPS reverse proxy. For nginx, the relevant configuration is:

server {
listen 443 ssl;
server_name concierge-dev.example.internal;

      ssl_certificate     /etc/nginx/tls/concierge.pem;
      ssl_certificate_key /etc/nginx/tls/concierge-key.pem;

      location / {
          proxy_pass http://frontend:80;
          proxy_set_header Host $host;
          proxy_set_header X-Forwarded-Proto https;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
}

server {
listen 80;
server_name concierge-dev.example.internal;
return 301 https://$host$request_uri;
}

Protect the private key:

chmod 600 tls/concierge-key.pem

Finally, register the exact HTTPS URL in Entra as a SPA redirect URI, for example:

https://concierge-dev.example.internal/

or, if mounted under a prefix:

https://concierge-dev.example.internal/concierge/

Installing the CA only on the server is insufficient: the certificate becomes trusted across the LAN only after rootCA.pem is installed on every client device.
