#default index.html to specific filename
DirectoryIndex first.html orThis.html mayCanBeThis.html

#error message custom
ErrorDocument 403 "Sorry, you are not permitted to access this."
ErrorDocument 404 /404.php
ErrorDocument 500 "Oops! Our server encountered an error. Try again later."

#force www | non www
RewriteEngine on
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteRule ^(.*)$ http://www.example.com/$1 [L,R=301,NC]


#force https
RewriteEngine on
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteRule ^(.*)$ http://www.example.com/$1 [L,R=301,NC]

#force trailing slash
RewriteEngine on
RewriteCond %{REQUEST_URI} /+[^\.]+$
RewriteRule ^(.+[^/])$ %{REQUEST_URI}/ [R=301,L]
#remove it
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [R=301,L]

#cruise new domain
RewriteEngine on
RewriteCond %{HTTP_HOST} ^OldDomain.com
RewriteRule ^(.*) http://NewDomain.com/$1 [P]

#keep url clean(fileformat remover)
RewriteEngine on
RewriteCond %{SCRIPT_FILENAME} !-d
RewriteRule ^([^.]+)$ $1.(TypeofFile) [NC,L]

#keep hidden_file hidden
RedirectMatch 404 /\..*$

#keep bandwith from stealing by linking my image
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?yourdomain.com [NC]
RewriteRule \.(jpg|jpeg|png|gif)$ - [NC,F,L]
#bymessage
RewriteRule \.(jpg|jpeg|png|gif)$ http://www.yourdomain.com/no-hotlinking.jpg [R,L]

#powerup mysite with compressing
AddOutputFilterByType DEFLATE text/text text/html text/plain text/xml text/css application/x-javascript application/javascript

#header expires
ExpiresActive On
ExpiresByType image/jpg "access plus 1 year"
ExpiresByType image/jpeg "access plus 1 year"
ExpiresByType image/gif "access plus 1 year"
ExpiresByType image/png "access plus 1 year"
ExpiresByType text/css "access plus 1 month"
ExpiresByType application/pdf "access plus 1 month"
ExpiresByType text/x-javascript "access plus 1 month"
ExpiresByType application/x-shockwave-flash "access plus 1 month"
ExpiresByType image/x-icon "access plus 1 year"
ExpiresDefault "access plus 2 days"

#upload limit
LimitRequestBody 1048576
#htpasswd
"#!/bin/bash \
 work_dir=$($HOME/myweb)
 cd $workdir
#protect file
 echo "AuthUserFile /home/username/.htpasswd\n
	AuthName "Login Required"\n
	AuthType basic\n
	<Files mypage.html>\n
		Require valid-user\n
	</Files>"
 >> .htpasswd
#protect dir
 echo "AuthUserFile /home/username/.htpasswd\n
	AuthName "Login Required"\n
	AuthType basic\n
	Require valid-user"
 >> .htpasswd
#fully block file
 echo "<files filename>\n
	order allow,deny\n
	deny from all\n
	</files>"
 >> .htpasswd

