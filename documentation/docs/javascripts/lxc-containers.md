# Nginx is in temporary test Container's config:

+----------+---------+---------------------+------+------------+-----------+
| NAME | STATE | IPV4 | IPV6 | TYPE | SNAPSHOTS |
+----------+---------+---------------------+------+------------+-----------+
| proxy | RUNNING | 192.168.0.2 (eth0) | | PERSISTENT | 0 |
+----------+---------+---------------------+------+------------+-----------+

## Folders Layout

    /etc/nginx/
    ├── conf.d
    │   ├── default.conf
    │   ├── gzip.conf
    │   ├── performance.conf
    │   ├── security.conf
    │   ├── ssl-files.conf
    │   └── ssl.conf
    ├── fastcgi_params
    ├── mime.types
    ├── modules -> /usr/lib/nginx/modules
    ├── nginx.conf
    ├── scgi_params
    ├── ssl
    │   └── dhparam.pem
    ├── upstream
    │   ├── mis-glowroot.conf
    │   ├── mis.conf
    │   ├── munin.conf
    │   ├── pgadmin.conf
    │   ├── staging-glowroot.conf
    │   └── staging.conf
    └── uwsgi_params

## DHIS2 Main Instance `mis.conf`

    # Proxy pass to servlet container
    location /mis {
        proxy_pass                http://192.168.0.10:8080/mis;
        proxy_redirect            off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto  $scheme;
            proxy_hide_header X-Frame-Options;
        proxy_hide_header Strict-Transport-Security;
        proxy_hide_header X-Content-Type-Options;
        proxy_hide_header X-XSS-protection;
            proxy_hide_header X-Powered-By;
            proxy_hide_header Server;

        proxy_connect_timeout  480s;
        proxy_read_timeout     480s;
        proxy_send_timeout     480s;

        proxy_buffer_size        128k;
        proxy_buffers            8 128k;
        proxy_busy_buffers_size  256k;
    }
