The configuration of the original community official document, the system could not effectively listen on port 9311, 
so the barbican-api service failed to start.
		<VirtualHost [::1]:9311>
		    ServerName <CONTROLLER-HOSTNAME>
		
		    ## Logging
		    ErrorLog "/var/log/httpd/barbican_wsgi_main_error_ssl.log"
		    LogLevel debug
		    ServerSignature Off
		    CustomLog "/var/log/httpd/barbican_wsgi_main_access_ssl.log" combined
		
		    WSGIApplicationGroup %{GLOBAL}
		    WSGIDaemonProcess barbican-api display-name=barbican-api group=barbican processes=2 threads=8 user=barbican
		    WSGIProcessGroup barbican-api
		    WSGIScriptAlias / "/usr/lib/python2.7/site-packages/barbican/api/app.wsgi"
		    WSGIPassAuthorization On
		</VirtualHost>

The WSGI gateway configuration file for the barbican-api service must be configured as follows for the system to have a 
normal quirion 9311 service port
Listen 9311
    <VirtualHost *:9311>
        #ServerName controller

        ## Logging
        ErrorLog "/var/log/httpd/barbican_wsgi_main_error_ssl.log"
        LogLevel debug
        ServerSignature Off
        CustomLog "/var/log/httpd/barbican_wsgi_main_access_ssl.log" combined

        WSGIApplicationGroup %{GLOBAL}
        WSGIDaemonProcess barbican-api display-name=barbican-api group=barbican processes=2 threads=8 user=barbican
        WSGIProcessGroup barbican-api
        WSGIScriptAlias / "/usr/lib/python2.7/site-packages/barbican/api/app.wsgi"
        WSGIPassAuthorization On

        <Directory /usr/lib>
           <IfVersion >= 2.4>
               Require all granted
           </IfVersion>
           <IfVersion < 2.4>
               Order allow,deny
               Allow from all
           </IfVersion>
        </Directory>
    </VirtualHost>

