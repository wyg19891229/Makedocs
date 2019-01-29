NGINX Web Server
----------------

NGINX is a standard HTTP/SMTP server that also functions as a reverse proxy, and a generic TCP/UDP proxy server. The reverse proxy functionality is used to pipe external client-requests through a firewall (in a private network) and on to the appropriate backend server. This provides an additional level of abstraction, plus improved security and control, ensuring the secure flow of network traffic between clients/servers.

### **Settings and Configuration**

You may modify the highlighted fields in the following configuration files, if necessary.

-   The first config file **\<nginx.conf\>** allows you to customize certain **performance** aspects of the BioConnect ID solution.

-   The second config file **\<default.conf\>** allows you to configure **security** related settings, including location of SSL certificates, and proxy routing rules.

#### Sample NGINX Config File

>**\</etc/nginx/nginx.conf\>**
```
user			nginx;
worker_processes 	8;
error_log		/var/log/nginx/error.log warn;
pid			/var/run/nginx.pid;

events {
	worker_connections 1024;
}

http {
	include	/etc/nginx/mime.types;
	default_type 	application/octet-stream;

	log_format main	'\$remote_addr - \$remote_user [\$time_local] "\$request" '
			'\$status \$body_bytes_sent "\$http_referer" '
			'"\$http_user_agent" "\$http_x_forwarded_for"';

	client_body_buffer_size 	10K;
	client_header_buffer_size 	1k;
	client_max_body_size 		8m;
	large_client_header_buffers 2 	1k;
	access_log 	/var/log/nginx/access.log main;
	sendfile 		on;
	tcp_nopush 		on;
	keepalive_timeout 	65;
	gzip 			on;
	include 	/etc/nginx/conf.d/\*.conf;
}
```


| VARIABLE               | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **worker_processes**   | The number of concurrent ```{worker_processes}``` the server can run simultaneously.   **Tip**: The number of worker processes should match how many CPU cores the server has at its disposal. **Default value: ```{8;}```**                                                                                                                                                                                                                                                                                                     |
| **error_log**          | 2-part value determines the Error Log’s directory location and log type. Available types include, ```<warn>```, ```<error>```, ```<crit>```, ```<alert>```, & ```<emerg>```. **Default value(s): ```{/var/log/nginx/error.log warn;}```**                                                                                                                                                          |
| **pid**                | Configures the directory location of the NGINX system process. **Default value: ```{/var/run/nginx.pid;}```**                                                                                                                                                                                                                                                                                      |
| **worker_connections** | The maximum number of concurrent connections per worker. **Default value: ```{1024;}```**                                                                                                                                                                                                                                                                                                          |
| **log_format**         | 2-part value determines the log type and the format used when outputting the log to a file, and the format directory location of the log. The log format used when outputting the log data to a file. **Default value: ```{main '\$remote_addr - \$remote_user [\$time_local] "\$request" ' '\$status \$body_bytes_sent "\$http_referer" ' ' "\$http_user_agent" "\$http_x_forwarded_for" ';}```** |
| **access_log**         | 2-part value determines the directory location for the stored logs, and the log type being used. **Default value:``` {/var/log/nginx/access.log main;}```**                                                                                                                                                                                                                                        |
| **sendfile**           | Once enabled, ```{sendfile}``` allows NGINX to send static files extremely fast with low latency (transferring data from file-descriptor to file-descriptor, directly in kernel space).   **Note**: Should be used in combination with ```{tcp_nopush}```. **Default value: ```{on;}```**                                                                                                                                                                                                      |
| **tcp_nodelay**        | Once enabled, ```{tcp_nodelay}``` forces network sockets to send the data in their buffer, no matter what the packet size is. To avoid network congestion, ```{tcp_nodelay}``` waits up to 0.2 sec so the packets won’t be too small.   **Note**: Can be used in combination with ```{sendfile}``` and ```{tcp_nopush}``` to accelerate network traffic further. **Default value: ```{on;}```**                                                                                                                                                          |
| **tcp_nopush**         | Once enabled, ```{tcp_nopush}``` blocks data until the packet-size is equal to the MTU, minus the 40-60 bytes for the IP header.   **Note**: ```{tcp_nopush}``` must be activated with ```{sendfile}```.  **Default value: ```{on;}```**                                                                                                                                                                                                                                                                                                                      |
| **keepalive_timeout**  | The ```{keepalive_timeout}``` setting forces sockets to stay open for a set amount of time after sending data. This enables establishing new connections without the need for a TCP 3-Way Handshake on every connection. **Note**: Should be used in combination with ```{tcp_nodelay}```. **Default value: ```{65;}```**                                                                                                                                                                   |
| **gzip**               | Enables data compression when generating system log files. **Default value: ```{on;}```**                                                                                                                                                                                                                                                                                                                                                                        |
| **include**            | Specifies additional site configuration files located in the **\<conf.d\>** folder. **Default value: ```{/etc/nginx/conf.d/\*.conf;}```**                                                                                                                                                                                                                                                          |



#### Sample ‘default.conf’ File

>**\</etc/nginx/conf.d/default.conf\>**
```
upstream bcid {
server 				127.0.0.1:4434;
}
server {
listen 				443 ssl;
server_name 			internal.bioconnectid.com;
ssl_certificate 		/etc/nginx/ssl/bmo_bundle_certificate.crt;
ssl_certificate_key 		/etc/nginx/ssl/bmo_certificate_key.key;
ssl_protocols 			TLSv1.2;
ssl_ciphers			ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256;
root 				/var/www/html;
location /v2 {
   proxy_pass 			https://bcid;
   proxy_set_header 		Host \$host;
   proxy_set_header 		X-Forwarded-For \$proxy_add_x_forwarded_for;
   proxy_ssl_server_name 	on;
}
location \~\* \^/assets/ {
   expires 			1y;
   add_header 			Cache-Control public;
   add_header 			Last-Modified "";
   add_header 			ETag "";
   break;
   }
}
```
| VARIABLE                  | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                    |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **server**                | The IP address and Port that your Puma application server is using. **Default value: ```{127.0.0.1:4434;}```**                                                                                                                                                                                                                                                       |
| **listen**                | The port the server should be listening on (most commonly either port 443 (HTTPS), or port 80 (HTTP). **Default value: ```{443 ssl;}```**                                                                                                                                                                                                                            |
| **server_name**           | The publicly accessible hostname of the BCID cloud solution. **IMPORTANT**: This must be replaced with your server’s actual Hostname or IP address. **Default value: ```{internal.bioconnectid.com;}```**                                                                                                                                                                                                                                                                                                   |
| **ssl_certificate**       | The path and filename of the SSL Encryption Certificate for NGINX. **Default value: ```{/etc/nginx/ssl/ssl_cert.crt;}```**                                                                                                                                                                                                                                           |
| **ssl_certificate_key**   | The path and filename of the SSL Certificate Key. **Default value: ```{/etc/nginx/ssl/ssl_cert_key.key;}```**                                                                                                                                                                                                                                                        |
| **ssl_protocols**         | The SSL encryption protocols explicitly used by the server. **Default value: ```{TLSv1.2;}```**                                                                                                                                                                                                                                                                      |
| **ssl_ciphers**           | The allowed cipher encryption keys used by the server. **Default value: ```{ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256;}```** |
| **root**                  | The path to the “Administration Console” files. **Default value: ```{/var/www/html;}```**                                                                                                                                                                                                                                                                            |
| **proxy_pass**            | Sets the address of the proxied server (as “http” or “https”). The address can be specified as a domain name or IP address, and an optional port. E.g., ```{http://localhost:8000/uri;}```. **Default value: ```{https://bcid;}```**                                                                                                                                       |
| **proxy_set_header**      | **Default value (Host): ```{\$host;}```**                                                                                                                                                                                                                                                                                                                                   |
|                         | **Default value (X-Forwarded-For): ```{\$proxy_add_x_forwarded_for;}```**                                                                                                                                                                                                                                                                                                              |
| **proxy_ssl_server_name** | **Default value: ```{on;}```**                                                                                                                                                                                                                                                                                                                                       |
| **expires**               | This specifies the expiration time for server cache. We have set it to a 1-year maximum, as per “RFC2616”. **Default value: ```{1y;}```**                                                                                                                                                                                                                            |
| **add_header**            | **Default value (Cache-Control): ```{public;}```**                                                                                                                                                                                                                                                                                                                                   |
|                           | **Default value (Last-Modified): ```{“”;}```**                                                                                                                                                                                                                                                                                                                                       |
|                           | **Default value (Etag): ```{“”;}```**                                                                                                                                                                                                                                                                                                                                       |
