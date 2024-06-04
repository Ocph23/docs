#.htaccess for spa apps (blazor, angular) in share hosting 
<code>


RewriteEngine On
RewriteCond %{REQUEST_FILENAME} -s [OR]
RewriteCond %{REQUEST_FILENAME} -l [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^.*$ - [NC,L]
RewriteRule ^(.*) /index.html [NC,L]"
 DO NOT REMOVE OR MODIFY. CLOUDLINUX ENV VARS CONFIGURATION BEGIN
`<IfModule Litespeed>
</IfModule>`
 DO NOT REMOVE OR MODIFY. CLOUDLINUX ENV VARS CONFIGURATION END

</code>


#.htaccess for Laravel 11 
<code>
`<IfModule mod_rewrite.c>
RewriteEngine On
RewriteRule ^(.*)$ public/$1 [L]
</IfModule>`
</code>

