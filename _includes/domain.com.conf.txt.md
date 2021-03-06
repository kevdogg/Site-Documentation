```bash
server {
        listen [::]:443 ssl http2;
        listen      443 ssl http2;

        server_name <domain.com>;

        #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
        #                  '$status $body_bytes_sent "$http_referer" '
        #                  '"$http_user_agent" "$http_x_forwarded_for"';

        access_log /var/log/nginx/<domain.com>.access.log main buffer=32k;
        error_log /var/log/nginx/domain.com>.error.log;

        include snippets/<domain.com>.cert.conf;
        
        # Choose either modern or intermediate parameters for SSL configuration
        #include snippets/ssl-params.conf;
        include snippets/ssl-modern-params.conf;

        keepalive_timeout 65;

        # Allow large attachments
        client_max_body_size 128M;
	
        include snippets/authelia.location.conf;

        location / {
          
          # This is an example of a typical reverse proxy block that does not require authelia authentication.

          #### BEGIN UNPROTECTED BLOCK #####

          # Optionally include access rules if needing to restrict certain domains
          #include snippets/internal-access-rules.conf;	
          include snippets/proxy-params.conf;
          
          # May include http or https: pass statement. 
          # https: requires:
            # An SSL certificate (corresponding in the example to domain.com ) to be installed on the reverse proxy
            # An optional proxy-ssl-pararms.conf directive file
          #proxy_pass http://backend.<domain.com>:80;
          proxy_pass https://backend.<domain.com>;
          include snippets/proxy-ssl-params.conf;
          
          #### END UNPROTECTED BLOCK #####
          
          #### BEGIN PROTECTED BLOCK #####

          ## Use of .htaccess files no longer needed as authelia is doing the authentication for this location
          ##auth_basic "Private";
          ##auth_basic_user_file /etc/nginx/private/admin.scr;

          ## Optionally include access rules if needing to restrict certain domains
          ##include snippets/internal-access-rules.conf;	
    
          ## May include http or https: pass statement. 
          ## https: requires:
          #  # An SSL certificate (corresponding in the example to domain.com ) to be installed on the reverse proxy and
          #  # An optional proxy-ssl-pararms.conf directive file
          ##set $upstream http://backend.<domain.com>:80;
          #set $upstream https://backend.<domain.com>:443;
          #proxy_pass $upstream;
          
          #include snippets/authelia_auth.conf;
          #include snippets/authelia_proxy.conf;
          ##proxy_redirect off;
          
          #### END PROTECTED BLOCK #####
        }
        
        location /admin {

          # This is an example of a typical reverse proxy block that REQUIRES authelia authentication.

          #### BEGIN PROTECTED BLOCK #####

          # Use of .htaccess files no longer needed as authelia is doing the authentication for this location
          #auth_basic "Private";
          #auth_basic_user_file /etc/nginx/private/admin.scr;

          # Optionally include access rules if needing to restrict certain domains
          #include snippets/internal-access-rules.conf;	
          
          # May include http or https: pass statement. 
          # https: requires:
            # An SSL certificate (corresponding in the example to domain.com ) to be installed on the reverse proxy and
            # An optional proxy-ssl-pararms.conf directive file
          #set $upstream http://backend.<domain.com>:80;
          set $upstream https://backend.<domain.com>:443;
          proxy_pass $upstream;
          include snippets/proxy-ssl-params.conf;

          include snippets/authelia_auth.conf;
          include snippets/authelia_proxy.conf;
          #proxy_redirect off;
          
          #### END PROTECTED BLOCK #####

        }
}

```
