```bash
# certs sent to the client in SERVER HELLO are concatenated in ssl_certificate

# Please adjust directories and filenames to represent where certicate files are located
# fullchain.pem and privkey.pem files (or equivalents if using a different provider than Let's Encrypt) are absolutely necessary

ssl_certificate /etc/letsencrypt/<domain.com>/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/<domain.com>/privkey.pem;
ssl_trusted_certificate /etc/letsencrypt/<domain.com>/chain.pem;.
```
