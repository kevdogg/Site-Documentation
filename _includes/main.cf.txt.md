```bash
# See /usr/share/postfix/main.cf.dist for a commented, more complete version

## MANDATORY to adjust these parameters
myhostname = <hostname>.<domain.com>
mydomain = <domain.com>

# Verify this file exists or you will need to install the correct package for your distribution
# ubuntu apt install ca-certificates
# arch pacman -S ca-certificates-utils
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt


# PLEASE CHOOSE ONLY ONE OPTION AT BOTTOM OF FILE
# Mail can either be sent over port 587 or 465 (Not both!).
# Both are valid Ports for Gmail
## TLS Option (Port 587)
## TLS Option (Port 465)
# Comment the Other Port Option That your are not Using

# Debian specific:  Specifying a file name will cause the first
# line of that file to be used as the name.  The Debian default
# is /etc/mailname.
#myorigin = /etc/mailname

smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h

readme_directory = no

# See http://www.postfix.org/COMPATIBILITY_README.html -- default to 2 on
# fresh installs.
compatibility_level = 2
inet_protocols = all 

# mydestination =  localhost.localdomain, localhost
#mailbox_size_limit = 0
#recipient_delimiter = +
#inet_interfaces = all
#relayhost = 
#mynetworks = 127.0.0.1/32 10.0.1.195/32 172.17.0.1/32 [::1]/128 [fe80::8444:f0ff:fe47:f619]/128 [fe80::42:1ff:feb6:8a82]/128


# Add following
myorigin = $myhostname
alias_maps = hash:/etc/postfix/aliases
alias_database = $alias_maps
sendmail_path = /usr/sbin/sendmail
newaliases_path = /usr/bin/newaliases
mailq_path = /usr/bin/mailq
home_mailbox = Mail/
mynetworks="127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128"
inet_interfaces=loopback-only
disable_vrfy_command=yes
smtpd_helo_required=yes

## SASL Option
smtp_sasl_auth_enable = yes
smtp_sasl_security_options = noanonymous


## Global Client TLS Options
tls_random_source = dev:/dev/urandom
smtp_use_tls = yes
#
### SMTP Parameters - (Medium) - Valid for Mailgun and Gmail
#smtp_tls_mandatory_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
#smtp_tls_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
#smtp_tls_mandatory_ciphers = medium
#tls_medium_cipherlist = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-E@@@
#
## SMTP Parameters - (Modern) - Valid for Gmail
smtp_tls_mandatory_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1, !TLSv1.2
smtp_tls_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1, !TLSv1.2
#

## Enable Options for Either Port 587 or Port 465

## TLS Option (Port 587)
#smtp_tls_security_level = encrypt
#Gmail SMTP
#relayhost = [smtp.gmail.com]:587
#smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd_GMAIL_587
## End TLS Options (Port 587)

## TLS Option (Port 465)
smtp_tls_security_level = encrypt
smtp_tls_wrappermode = yes
#Gmail SMTP
relayhost = [smtp.gmail.com]:465
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd_GMAIL_465
## End TLS Option (Prot 465)
```
