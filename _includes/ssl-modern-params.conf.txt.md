```bash
ssl_session_timeout 1d;
ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
ssl_session_tickets off;

# modern configuration
ssl_protocols TLSv1.3;
ssl_prefer_server_ciphers off;

# HSTS (ngx_http_headers_module is required) (63072000 seconds)
#add_header Strict-Transport-Security "max-age=63072000" always;

# OCSP stapling
ssl_stapling on;
ssl_stapling_verify on;

# verify chain of trust of OCSP response using Root CA and Intermediate certs
ssl_trusted_certificate /etc/letsencrypt/authelia.<domain.com>/chain.pem;

# replace with the IP address of your resolver
resolver 10.0.1.1;
```
