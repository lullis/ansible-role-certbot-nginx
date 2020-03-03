certbot-nginx
=============

Install and configures nginx and certbot with the proper plugin, and
provides an easy configuration to have all sites hosted by nginx
automatically redirected to the HTTPS port

This assumes that you have one nginx server as the TLS endpoint as a
proxy for different websites you have. Each site is then defined by
the domain you want to obtain a certificate through certbot.


Requirements
------------

Right now this role is done with Ubuntu in mind. PR's welcome!


Role Variables
--------------

Location of Let's Encrypt CA Certificate
```
ca_certificate_path: /etc/ssl/certs/letsencrypt_ca.pem
```

Where certbot will store the downloaded certificates.
```
certificate_store_live_dir: /etc/letsencrypt/live
certificate_store_dir: /etc/letsencrypt/archive
```

Users belonging to this group will be able to access the certificate folders
```
certificate_store_access_group_name: ssl-cert
```

A Diffie–Hellman (D-H) key exchange file is generated and used for all
SSL sites. The variables below control its creation.
```
ssl_dhparam_key_size: 2048
ssl_dhparam_file_location: /etc/ssl/certs/dhparam.pem
```

There is only one certificate
```
certbot_nginx_snippet_ssl_certificates_path: /etc/nginx/snippets/ssl_certificate_files.conf
certbot_nginx_snippet_ssl_params_path: /etc/nginx/snippets/ssl-params.conf
```

Where certbot will place the files with the responses for the challenges from Let's Encrypt
```
nginx_webroot_path: /var/www/html
```

The name of the nginx site file that will contain the redirect from
HTTP to HTTPS and the rules for .well-known files
```
nginx_ssl_site_name: default
```

A mapping of domains you have SSL certificate to their upstream servers
```
sites: {}
```

Dependencies
------------

Depends on `geerlingguy.nginx` to install nginx


Example Playbook
----------------

From the variables described above, the only one you really need to
define for this role is the `sites` one.

```
    - hosts: ssl proxy
      roles:
         - role: lullis.certbot-nginx
           vars:
               - sites:
                   myblog.mydomain.com:
                     upstream_name: blog
                     upstream_host: blog-server
                     upstream_port: 80
                   myapp.mydomain.com:
                     upstream_name: app
                     upstream_host: appserver
                     upstream_port: 3000
```

License
-------

BSD


Author Information
------------------

This role was created in 2019 by [Raphael Lullis](https://raphael.lullis.net)
